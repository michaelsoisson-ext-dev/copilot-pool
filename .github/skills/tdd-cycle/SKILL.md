---
name: tdd-cycle
description: 'Execute a red-green-refactor TDD cycle from an existing tdd-plan.md (or equivalent BDD/TDD plan). Implements one Test Case at a time with 80%+ coverage, git checkpoints per phase, and JSDoc documentation. Deterministic implementation only, it does not derive requirements or write the plan. Use when the user wants to build features or fix bugs test-first, mentions "red-green-refactor", wants integration tests, or already has a TDD plan ready to execute.'
---

# Goal

Deterministically implement an existing TDD plan, one Test Case at a time, through Red → Green → Refactor → Doc, with a git checkpoint and coverage record at every stage.

## Scope

**In scope:** writing/running tests, writing minimal implementation, refactoring, JSDoc, coverage tracking, git checkpoints.

**Out of scope — delegate to `tdd-plan`:** extracting requirements, decomposing rules into Test Cases, deciding execution order. If given a raw issue/brief with no plan, or a plan missing Test Cases, stop and ask the user to run `tdd-plan` first — do not improvise test cases.

## Input Contract

Requires a `tdd-plan.md` (or equivalent) containing ordered Test Cases (TC-NNN) and a TDD Strategy section. Each cycle below implements exactly one Test Case from that plan, in the order the plan specifies.

# Constraints

1. **DO** strictly follow the [**TDD cycle protocol**](#tdd-cycle-protocol)
2. **DO** strictly follow the [phase execution guidelines for red->green->refactor](references/phases-guidelines.md)
3. **DO** return control to the user **ONLY when ALL** of the following are **true**:
    - Every Test Case in the plan has a completed todo entry
    - Every red phase test has been passed to green by a separate green phase
    - a final refactor phase has been passed
    - Enforce tests-before-code methodology

If any of these conditions are not met, keep going.

4. **DO** use `manage_todo_list` to track progress through every cycle — never skip it.

## Test Quality Standards

follow the [AI pitfalls](references/ai-pitfalls.md) and Refer to [test-execution.md](references/test-executions.md) for comprehensive guidance on:

- [Testing Strategy](references/test-executions.md#test-strategy)
- [Test Structure](references/test-executions.md#test-structure)
- [Effective Mocking](references/test-executions.md#effective-mocking)
- [Test Execution](references/test-executions.md#test-execution)

example of test execution cmd :

```bash
 env-cmd npx jest --runInBand ./__tests__/ox/ox-710.spec.js
```

## TDD cycle Protocol

1. LOAD the plan's Test Cases (TC-NNN) in the order given by its TDD Strategy section. Do not decompose new tasks from the raw request — that is `tdd-plan`'s job.
2. CREATE a todo list with one entry per Test Case.
3. For each Test Case verify the [Checklist per cycle](#checklist-per-cycle) :
    1. Mark it in-progress
    2. **red phase** write/update a failing test for the Test Case's expected behavior.
    3. verify red -- run test -- Verify it FAILS
    4. create a [Git checkpoint](#git-checkpoints) commit immediately with pattern `test: <test case title> for <feature or bug>`
    5. **green phase** write minimal code to pass.
    6. create a [Git checkpoint](#git-checkpoints) commit immediately with pattern `fix: minimal implementation`
    7. verify green -- run test -- Verify it PASSES
    8. **refactor phase** clean up duplication, improve naming, and enhance structure. Tests must stay green.
    9. create a [Git checkpoint](#git-checkpoints) commit immediately with pattern `refactor: clean up after <feature or bug> implementation`
    10. **doc phase** Add/update a concise JSDoc comment above each implemented function or method.
    11. create a [Git checkpoint](#git-checkpoints) commit immediately with pattern `docs: add a concise JSDoc comment above each implemented function or method`
    12. record test coverage targets in %
    13. next Test Case

4. STOP WHEN: only when all Test Cases are green and reviewed TDD cycle end or 8 verify green phase in same cycle reached.
   ON STOP: summarize what changed and what still fails
   Never **refactor** while **red** Get to **green** first.

## Checklist Per Cycle

```
[ ] Test describes behavior, not implementation
[ ] Use descriptive test names that explain what is being tested and the expected outcome
[ ] Keep tests independent - each test should run in isolation without depending on other tests
[ ] Create a checkpoint git commit after each TDD stage
[ ] Test uses public interface only
[ ] Test would survive internal refactor
[ ] One test a time
[ ] Code is minimal for this test
[ ] No speculative features added
```

## Progress Tracking

Use `manage_todo_list` and `git commit` like a checkpoint obsessively:

- Create the full task list (one entry per Test Case from the plan) BEFORE launching the cycle
- Mark tasks in-progress as you launch a phase
- Mark tasks complete only AFTER validation passes
- Create a [Git checkpoint](#git-checkpoints) commit after each TDD stage
- Create a test coverage after each TDD cycle
- Add new tasks only if a phase discovers additional work already implied by the plan's Test Cases — never add tasks that expand scope beyond the plan

This is your memory. Your context window will fill up. The todo list keeps you oriented.
save this todo list in file named `.ai-camm-bac/plans/todo_list_<unique-name>.md`

### Git Checkpoints

- Create a checkpoint commit after each TDD stage
- Do not squash or rewrite these checkpoint commits until the workflow is complete
- Each checkpoint commit message must describe the stage and the exact evidence captured
- Count only commits created on the current active branch for the current task
- Do not treat commits from other branches, earlier unrelated work, or distant branch history as valid checkpoint evidence
- Before treating a checkpoint as satisfied, verify that the commit is reachable from the current `HEAD` on the active branch and belongs to the current task sequence
- The preferred compact workflow is:
    - one commit for failing test added and RED validated
    - one commit for minimal fix applied and GREEN validated
    - one optional commit for refactor complete
- Separate evidence-only commits are not required if the test commit clearly corresponds to RED and the fix commit clearly corresponds to GREEN
- Squash merges are allowed only after the workflow evidence has been preserved . If checkpoint commits will be squashed, copy the RED/GREEN/refactor summary into the MR body, squash commit body, or evidence report so reviewers can still answer what was verified and how.
