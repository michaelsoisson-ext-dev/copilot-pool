---
name: glab-issue
description: 'Create GitLab issues from plans,requirements or specifications and generate draft merge requests (MRs) linked to issue IDs.Use this skill to automate GitLab issue/MR workflows with structured content'
---

# Prerequisites

1. Shell Check GitLab CLI availability with test `glab` command

```bash
 git remote -v && echo '---' && ( glab --version || true)
```

2. If the command isn't found, try reloading the shell by sourcing ~/.zshrc.

```bash
source ~/.zshrc && glab --version
```

3. Verify GitLab authentication and repository access via glab

```bash
    glab auth status && echo '---' &&  glab repo view
```

4. From a cloned repository, verify access: `glab repo view` to ensure `glab` detects the correct remote

## Workflow: Phase 1 → Create Issue from Plan

### Input

- Plan document (markdown, text, or specifications)
- Issue title and description content

### Steps

1. **Prepare Issue Content**
    - Extract title from plan (or use provided title)
    - Use issue content as issue description
    - Preserve formatting and structure

2. **Create Issue**

    ```bash
    glab issue create \
      --title "{issue-file-name}: {title}" \
      --description "$(cat issue.md)" \
      -R /mail/agenda-contacts/ommt
    ```

    - example title: UAC-1625: Preserve invitee status
    - Output: Issue created with IID (e.g., #42)
    - Note the IID for next phase

3. **Verify Creation**
    - Check issue URL:

    ```bash
        glab issue view {IID}
    ```

    - Confirm issue is visible and properly formatted

## Workflow: Phase 2 → Create Draft MR from Issue

### Input

- Issue IID from Phase 1 (or provided directly)

### Steps

1. **Create Draft Merge Request**

```bash
    glab mr create -i {IID}  --target-branch next-ai  --draft  -R mail/agenda-contacts/ommt
```

    - Links MR to issue automatically
    - Marks as draft (ready for work, not for review)
    - Creates branch with naming convention: `{IID}-{UAC-number}`
    - if a problem is thrown then  Create a local branch, commit the  plans, push it, and create a draft merge request

2. **Checkout Branch**

    ```bash
    glab mr checkout {MR_IID}
    ```

    - Creates branch with naming convention: `{IID}-{UAC-number}`
    - Switches to newly created branch
    - Ready for development

3. **Verify Creation** - Check MR URL output - Confirm draft status and linked issue

### Update MR description

```bash
 glab mr update {IID} --description "$(cat <plan_path>)
```

### Create a bdd issue related to main Issue

- create a linked BDD work item from the BDD plan and attaching it to the main UAC work item.

```bash
glab issue create  \
  --title "bdd {uac-title}"
  --description "$(cat <plan_path.md>)   --linked-issues {IID}  --label "BDD"  -R  mail/agenda-contacts/ommt
```

## Commands Reference

See [guidelines](references/guidelines.md) for detailed `glab` command documentation including:

- **Quick reference**: Common commands for issues, MRs, and CI/CD
- **Comments & discussions**: How to add comments, threaded replies, and diff comments
- **API calls**: Using `glab api` for advanced operations
- **Common mistakes**: Patterns to avoid and best practices

Quick reference for this workflow:

| Task             | Command                                                                                                  |
| ---------------- | -------------------------------------------------------------------------------------------------------- |
| Create issue     | `glab issue create --title "..." --description "..."`                                                    |
| Create bdd issue | `glab issue --title "bdd {uac-title}" --description "..." --linked-issues {IID} --label "BDD" -R <ommt>` |
| List issues      | `glab issue list --all`                                                                                  |
| View issue       | `glab issue view {IID}`                                                                                  |
| Create MR        | `glab mr create -i {IID}   --target-branch next-ai --draft --push`                                       |
| List MRs         | `glab mr list --all`                                                                                     |
| View MR          | `glab mr view {IID}`                                                                                     |
| Checkout branch  | `glab mr checkout {IID}`                                                                                 |

## Best Practices

- Always confirm issue/MR creation with the user before executing remote operations
- Use descriptive titles that reflect feature/issue scope
- Preserve plan content structure in issue description
- Verify GitLab connectivity before running commands
- Handle authentication failures gracefully
- Report issue/MR URLs and IIDs clearly for user reference

## Output Format

**After Issue Creation:**

```
✅ Issue Created Successfully
- Title: Feature: {title}
- IID: #{issue_id}
- URL: https://gitlab.com/org/repo/-/issues/{issue_id}
```

**After MR Creation:**

```
✅ Draft MR Created Successfully
- Title: {issue_title}
- MR IID: !{mr_id}
- Branch: {branch_name}
- URL: https://gitlab.com/org/repo/-/merge_requests/{mr_id}
```

**After Local branch Creation:**

```
✅ Local Branch Created Successfully
- Branch: {branch_name}
- IID: #{issue_id}
- URL: https://gitlab.com/org/repo/-/merge_requests/{mr_id}
```
