# TDD Plan: <UAC-ID> <Feature Title>

## Feature

As a <user role>, I want <capability> so that <business value>.

---

## Rules

- **Rule:** <business rule 1>
- **Rule:** <business rule {X}> ...

---

## Test Cases

### TC-001 — <test case title>

- **Related rule:** <rule name>
- **Purpose:** <what this test verifies>
- **Preconditions:** <setup required>
- **Input data:** <sample input>
- **Steps:**
    1. <step 1>
    2. <step 2>
    3. <step 3>
- **Expected result:** <expected outcome>
- **Mock/stub requirements:** <dependencies to mock>
- **Priority:** <high / medium / low>
- **depends_on** <depends_on test case dependencies>
- **shared_files** <shared_files files impacted by dependencies>

### TC-00{X} — <test case title> ...

---

## TDD Strategy

<how the sequence (step N "Red" → step N "Green") is executed>
<depends_on test case dependencies>,
<shared_files files impacted by dependencies>,
<integration_order indicating the order of operations>

---

## Technical Specifications

### Architecture

> Data flow and component interactions.

### Implementation

> Modules to build/modify and interfaces.

### Integration

> API contracts, schema changes, auth.

---
