---
name: tdd-git
description: 'tdd-git manages Git resources during tdd-flow orchestration.Use this tool to resolve the reference Git commit that serves as the base for all branches created during the workflow cycle, merge requests, and worktree operations.'
---

# Resolve base commit

Resolve the immutable commit used as the base of all cycle branches.

input:

- worsflow.input.repo_root
- workflow.input.target_branch

```bash
-lc
|
    set -euo pipefail

    BASE_COMMIT="$(git -C "{{ workflow.input.repo_root }}" rev-parse "{{ workflow.input.target_branch }}^{commit}")"

    printf '{"base_commit":"%s"}\n' "$BASE_COMMIT"
```

**Output Format**

```
✅ Base Commit Resolved
 {"base_commit":"a1b2c3d4e5f6..."}
```

if failed then route to exit with code 0 and show to the user console
`Unable to resolve the initial Git base commit.`

# Worktree Setup

    Create and validate the dedicated branch, isolated worktree and TODO artifact before any source or test file is changed.

```bash
- -lc
- |
set -euo pipefail

REPO_ROOT="{{ workflow.input.repo_root }}"
BASE_COMMIT="{{ workflow.input.base_commit }}"
BRANCH_NAME="{{ workflow.input.branch_name }}"
WORKTREE_PATH="{{ workflow.input.worktree_path }}"
TODO_FILE_PATH="{{ workflow.input.todo_file_path }}"
CYCLE_ID="{{ workflow.input.cycle_id }}"
TC_ID="{{ workflow.input.tc_id }}"

git -C "$REPO_ROOT" rev-parse --verify "$BASE_COMMIT^{commit}" >/dev/null

if [ -e "$WORKTREE_PATH" ]; then
echo "Worktree path already exists: $WORKTREE_PATH" >&2
exit 20
fi

mkdir -p "$(dirname "$WORKTREE_PATH")"

if git -C "$REPO_ROOT" show-ref --verify --quiet "refs/heads/$BRANCH_NAME"; then
git -C "$REPO_ROOT" worktree add "$WORKTREE_PATH" "$BRANCH_NAME"
else
git -C "$REPO_ROOT" worktree add \
    -b "$BRANCH_NAME" \
    "$WORKTREE_PATH" \
    "$BASE_COMMIT"
fi

CURRENT_BRANCH="$(git -C "$WORKTREE_PATH" branch --show-current)"

if [ "$CURRENT_BRANCH" != "$BRANCH_NAME" ]; then
echo "Unexpected branch: $CURRENT_BRANCH; expected: $BRANCH_NAME" >&2
exit 21
fi

mkdir -p "$WORKTREE_PATH/$(dirname "$TODO_FILE_PATH")"

if [ ! -f "$WORKTREE_PATH/$TODO_FILE_PATH" ]; then
cat > "$WORKTREE_PATH/$TODO_FILE_PATH" <<EOF
# TDD cycle TODO

- Cycle ID: $CYCLE_ID
- Test case: $TC_ID
- Branch: $BRANCH_NAME
- Base commit: $BASE_COMMIT
- Overall status: in_progress

## Evidence

- RED command and expected failure:
- GREEN command and passing result:
- REFACTOR command and passing result:
- DOC command and passing result:
- Coverage command and percentage:
- RED checkpoint:
- GREEN checkpoint:
- REFACTOR checkpoint:
- DOC checkpoint:
EOF
fi

printf '{"setup_status":"ready","resolved_base_commit":"%s","branch_name":"%s","worktree_path":"%s","todo_file_path":"%s"}\n' \
"$BASE_COMMIT" \
"$BRANCH_NAME" \
"$WORKTREE_PATH" \
"$TODO_FILE_PATH"

```

## Confirm the branch exists:

    ```bash
    git -C "{{ workflow.input.repo_root }}" show-ref --verify --quiet \
    refs/heads/tdd/{{ workflow.input.run_id }}/{{ tc.tc_id }}
    ```

## Merge only this branch:

```bash
git -C "{{ workflow.input.repo_root }}" merge --no-ff --no-edit \
tdd/{{ workflow.input.run_id }}/{{ tc.tc_id }}
```

## If a merge conflict occurs:

- collect conflicting files;
- execute:

```bash
git -C "{{ workflow.input.repo_root }}" merge --abort
```

- return `integration_status: conflict`.

After a successful merge, run the repository regression command
documented in repository context, if a reliable command exists.

If post-merge validation fails, return:

```text
integration_status: failed_validation
```

Include the test command and failure evidence.

## If merge and validation succeed, return:

- `integration_status: integrated`;
- merge commit SHA;
- changed files where available.

Never modify another cycle branch or worktree.
Never leave the parent repository with an unresolved merge.
