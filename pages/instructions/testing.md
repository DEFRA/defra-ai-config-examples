---
layout: default
title: Testing Instructions
---

# Testing Instructions — Example

This is an example scoped instruction file for test code. It activates when Copilot is working on test files. Copy it into `.github/instructions/testing.instructions.md` in your repository.

## Example file contents

The content below goes into your `.github/instructions/testing.instructions.md` file:

---

````markdown
---
applyTo: "**/*.test.js,**/*.test.mjs,**/*.spec.js,**/test/**/*.js,**/tests/**/*.js"
---

# Testing

## Test framework

- Use **Jest** for Node.js unit and integration tests
- Use **Supertest** (via `createServer()`) for HTTP route testing — do not start the server on a real port in tests
- Use **Playwright** for end-to-end journey tests
- Use **xUnit v3** for .NET projects with FluentAssertions and NSubstitute

## File naming and location

- Co-locate unit test files next to the source file they test:
  - `src/services/application.js` → `src/services/application.test.js`
- Place integration tests in a top-level `test/integration/` directory
- Place Playwright journey tests in `test/journey/`

## Test structure — BDD naming

Use `describe` and `it` blocks with names that describe behaviour, not implementation:

✅ Good:
```javascript
describe('submitApplication', () => {
  describe('when the application data is valid', () => {
    it('should create a new record and return its reference number', async () => {})
  })
  describe('when required fields are missing', () => {
    it('should throw a validation error listing the missing fields', async () => {})
  })
})
```

❌ Bad:
```javascript
describe('submitApplication', () => {
  it('test1', () => {})
  it('works', () => {})
  it('should work correctly', () => {})
})
```

## Arrange-Act-Assert (AAA) pattern

Structure every test in three clearly separated sections:

```javascript
it('should return a formatted address when all fields are present', () => {
  // Arrange
  const address = { line1: '1 The Street', town: 'Testville', postcode: 'TE1 1ST' }

  // Act
  const result = formatAddress(address)

  // Assert
  expect(result).toBe('1 The Street, Testville, TE1 1ST')
})
```

## Coverage targets

| Code area | Minimum coverage |
|-----------|-----------------|
| Global (all files) | 90% |
| Business logic (services, validators) | 95% |
| Error handling and security-critical paths | 100% |

Coverage must not decrease from the project or SonarCloud baseline. Check the SonarCloud quality gate (SonarWay profile) before raising a PR.

## Route handler testing (Node.js / Hapi)

Test route handlers via `createServer()` — do not use `hapi.server()` directly in tests:

```javascript
import { createServer } from '../../src/server.js'

describe('GET /applications/{id}', () => {
  let server

  beforeAll(async () => {
    server = await createServer()
  })

  afterAll(async () => {
    await server.stop()
  })

  describe('when the application exists', () => {
    it('should return 200 and the application details', async () => {
      const response = await server.inject({
        method: 'GET',
        url: '/applications/ABC-123'
      })
      expect(response.statusCode).toBe(200)
    })
  })
})
```

## Mocking discipline

- Mock at the module boundary — mock imported modules, not internal functions
- Use `jest.mock()` at the top of the file, not inside `it` or `describe` blocks
- Reset mocks between tests with `jest.clearAllMocks()` in `beforeEach`
- Do not mock modules you own — favour testing real internal implementations
- Always mock: external HTTP calls, database operations, file system operations, `Date.now()`, `Math.random()`

```javascript
import { getApplicationByReference } from '../../src/services/applicationService.js'

jest.mock('../../src/services/applicationService.js')

beforeEach(() => {
  jest.clearAllMocks()
})
```

## What to avoid

- Do not use `.skip` or `.only` in committed test files — every test must run in CI
- Do not test implementation details — test observable behaviour
- Do not write tests that always pass (e.g. `expect(true).toBe(true)`)
- Do not use `setTimeout` or `sleep` to manage async timing — use `await` and proper async patterns
- Do not share mutable state between tests — each test must be independent and able to run in any order
````

---

[Back to instructions index]({{ "/pages/instructions" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
