---
name: brief
description:
    Give each prompt a compact brief with a comprehensive goal, affected files, constraints, and acceptance criteria.
    Apply the feeback experience to refine the current prompt so it aligns with proven best practices.DO NOT WRITE ANY CODE.
---

# Steps

- Read the prompt file carefully and refine its structure, wording, and organization to match the successful patterns you have observed.
- Defining expected deliverables and success criteria
- **Codebase Exploration**: Use the `codebase` tool to examine existing code structure, patterns, and architecture
- **Search & Discovery**: Use `search` and `searchResults` tools to find specific patterns, functions, or implementations across the project
- **Usage Analysis**: Use the `usages` tool to understand how components and functions are used throughout the codebase
- **Problem Detection**: Use the `problems` tool to identify existing issues and potential constraints
- **Multiple task Detection** : Use skill `refine` to turn multiple asks and follow-up instructions in one turn into crisp requests.

## Output

A single fenced in English grammar code block containing the optimized prompt.

On user input after showing the brief:

- Questions asked → clarify, or use #tool:vscode/askQuestions for follow-ups
- Approval given → acknowledge, the user can now use handoff buttons

Keep iterating until explicit approval or handoff.

### Sample

> **Example invocation:**: `/brief je souhaite utiliser le skill tdd-cycle pour refine l'uac-1231`

**output** :

```md
**goal** :
Use the`tdd-cycle` skill to refine the UAC-1231 work by improving the TDD workflow, test scenarios, or implementation guidance for that issue.

**affected files**:
The UAC-1231-related artifacts and any files the TDD cycle references, such as the issue, BDD/TDD plan, tests, or implementation files.

**constraints**:
Keep the work aligned with the existing project conventions and the UAC-1231 specifications; do not invent new requirements beyond the provided context; preserve the structure and intent of the current TDD artifacts.

**acceptance criteria**:
The `tdd-cycle` workflow for UAC-1231 is refined clearly and concretely, with the relevant artifacts updated or improved in a way that supports the intended TDD process.
```
