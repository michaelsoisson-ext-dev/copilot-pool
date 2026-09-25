---
name: tdd-plan
description: 'Turn a brief, issue, or BDD scenario into a deterministic TDD plan for the tdd-cycle skill. Produces a `tdd-plan.md` with Feature, Rules, ordered Test Cases (TC-NNN), and a Red-to-Green execution strategy. Planning only, never writes tests or code. Use before implementation to define test cases and acceptance criteria, or whenever tdd-cycle needs a plan to execute.'
---

# Goal

Produce exactly one deterministic artifact, `tdd-plan.md`, that the `tdd-cycle` skill can execute mechanically, one Test Case at a time, without re-deriving requirements or inventing test cases mid-implementation.

## Scope

**In scope:** requirement extraction, rule decomposition, test case specification, execution ordering, output formatting.

**Out of scope — delegate to `tdd-cycle`:** writing test code, running tests, implementing code, refactoring, git commits. If asked to do any of these, stop and point to `tdd-cycle` instead.

## Inputs

Accept any of: a `brief`, a written issue/spec, a BDD scenario (Given/When/Then), or a raw feature request. Resolve missing details from the referenced issue/spec — do not interview the user.

## Steps

1. **Extract & decompose** — identify the Feature and every business Rule from the input. Apply [decomposition principles](references/decomposition-principles.md) to break each Rule into one or more ordered Test Cases (TC-NNN), outside-in (acceptance → unit).
2. **Specify each Test Case** — purpose, preconditions, input data, steps, expected result, mock/stub requirements, priority. State the deterministic payload only, no Given/When/Then prose duplication.
3. **Define the TDD Strategy** —
   The exact order of the cycle (which test case turns red first, which turns green next) so that `tdd-cycle` can interpret it unambiguously.
   Explicitly show the dependencies between test cases.
4. **Validate completeness** against the [Plan Completeness Checklist](#plan-completeness-checklist) before saving.
5. **Save** the plan strictly following [the template](references/tdd-template.md) to `.ai-camm-bac/plans/tdd-plan-<uac-id>.md`.
6. **Hand off** — tell the user the plan is ready and give the file path for `tdd-cycle` to consume.

## Plan Completeness Checklist

- [ ] Every Rule maps to at least one Test Case
- [ ] Every Test Case has an unambiguous expected result
- [ ] Every Test Case lists its mock/stub requirements
- [ ] Test Cases are numbered and ordered outside-in (TC-001, TC-002, …)
- [ ] The TDD Strategy states the Red → Green order across all Test Cases
- [ ] The TDD Strategy states the TC00x → TC00y dependencies order across all Test Cases
- [ ] No test code, implementation code, or file edits were produced while planning
