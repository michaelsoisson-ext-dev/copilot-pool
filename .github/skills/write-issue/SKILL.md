---
name: write-issue
description: "Create or refactor a comprehensive issue from a ticket or specification by synthesizing business rules, requirements add acceptance criteria in a testable format.  Use this skill when the user asks for 'refine spec into issue', 'convert feature to issue' and 'turn ticket/spec into structured issue' prompt. you need to anchor planned behavior to testable guarantees and document it in a strict issue schema without interviewing the user. Do NOT interview the user — just synthesize what you already know."
---

# Operational Workflow

1.  Explore the repo to understand the current state of the codebase, if you haven't already. Use the project's domain glossary vocabulary throughout the context

2.  Read a ticket or specification provided as raw text.
    Convert each approved planned behavior into a testable guarantee. Identify direct dependencies (imports/exports) and Look for similar patterns in existing code

3.  Generate the document using the [**Strict Issue Schema**](references/issue-template.md) below and save it `.ai-camm-bac/issues/issue-<unique-name>`

4.  After generating the issue, verify that it contains a short [validation checklist](#validation-checklist) as below

## Validation checklist

```md
[] Summary
[] User Stories
[] Scope
[] Acceptance Criteria
[] Technical Specs
[] Risks
[] Reference Patterns
[] Dependencies
```
