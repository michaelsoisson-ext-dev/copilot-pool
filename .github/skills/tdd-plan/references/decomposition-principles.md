# Decomposition Principles

Used by `tdd-plan` to break a Feature into Rules and Test Cases, and to order the TDD Strategy outside-in. `tdd-cycle` does not need this file — it only executes the Test Cases already decomposed here.

## Core Philosophy

### Discovery > Development > Delivery

**Development**: Implement guided by examples

- Use examples as specifications
- Automate examples as tests
- Follow outside-in TDD

## Outside-In Ordering

When writing Test Cases and the TDD Strategy, order them from the outside (user-facing behavior) inward so `tdd-cycle` executes them in the right sequence:

1. **Acceptance-level Test Case** — describes the user-facing scenario
2. **Layer-level Test Case(s)** — one per layer the scenario touches (controller, service, …)
3. **Repeat** for each remaining Rule until every scenario is covered

`tdd-cycle` turns each of these Test Cases into its own red → green → refactor cycle; this skill only decides the order and content, not the implementation.

```
Scenario (Acceptance) ─┐
                       ├─> Controller Test ─┐
                       │                    ├─> Service Test ─┐
                       │                    │                 ├─> Code
                       │                    │                 │
                       │                    ├─ Service        │
                       │                    │                 │
                       ├─ Controller        │                 │
                       │                    │                 │
Scenario Passes ───────┴────────────────────┴─────────────────┘
```

## TDD

**TDD** (Test-Driven Development):

- Developer-focused
- Tests implementation
- Red-Green-Refactor cycle
- Unit tests guide design

**They complement each other:**

- BDD: What should we build? (outside-in)
- TDD: How should we build it? (inside-out)
