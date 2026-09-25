# Testing Patterns

## Core Sections (Required)

### 1 Test Stack and Commands

- Primary test framework: Jest `^30.3.0`
- Assertion/mocking tools: Jest (`expect`, `jest.mock`, `jest.spyOn`), `nock` for HTTP boundary mocking
- Commands:

```bash
npm test
npm run test:unit
npm run test:integration
npm run test:coverage
```

### 2 Test Layout

- Test file placement pattern: centralized under `__tests__/unit/**` and `__tests__/integration/**`; shared test utilities are in `__tests__/helpers/**`
- Naming convention: `*.spec.js`
- Setup files and where they run: `__tests__/jest.setup.mjs` configured in `jest.config.mjs` via `setupFiles`, so it runs before each test file

### 3 Test Scope Matrix

| Scope       | Covered? | Typical target                               | Notes                                                                     |
| ----------- | -------- | -------------------------------------------- | ------------------------------------------------------------------------- |
| Unit        | yes      | worker modules, SOAP/API helpers, utils      | Split into Jest projects such as `unit:mq`, `unit:soap`, `unit:ox`        |
| Integration | yes      | DB, MQ workers, BAC/CST/OX/WASSUP boundaries | `integration` Jest project under `__tests__/integration/**`               |
| E2E         | [TODO]   | [TODO]                                       | `__tests__/helpers/e2eEnvironment.js` exists, but no dedicated e2e script |

### 4 Mocking and Isolation Strategy

- Main mocking approach: module and collaborator mocks with `jest.mock`/`jest.fn`, plus helper factories (`__tests__/helpers/mock-workers-apis.js`, `__tests__/helpers/mq-fixtures.js`)
- Isolation guarantees: many suites clear mocks in `afterEach` via `jest.clearAllMocks()`, and bootstrap env defaults are injected from `__tests__/jest.setup.mjs`
- Common failure mode in tests: configuration/env drift can break tests that parse numeric queue settings unless defaults are present in setup

### 5 Coverage and Quality Signals

- Coverage tool + threshold: Jest coverage with global thresholds (branches/functions/lines/statements = 80%) in `jest.config.mjs`
- Current reported coverage: [TODO]
- Known gaps/flaky areas: some test command aliases referenced in docs are not present in `package.json` (`test:bac`, `test:cst`, `test:ox710`), indicating testing documentation drift

### 6 Evidence

- `package.json`
- `jest.config.mjs`
- `__tests__/jest.setup.mjs`
- `__tests__/unit/mq/mq.spec.js`
- `__tests__/helpers/mock-workers-apis.js`
- `__tests__/helpers/create-http-test-server.js`
- `.github/instructions/test.instructions.md`
