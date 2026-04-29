# Agent Instructions

This repository is a small Cypress test project. Follow these instructions when making changes with an AI coding assistant.

## Project Shape

- Cypress specs live in `cypress/e2e`.
- Cypress support files live in `cypress/support`.
- Fixtures live in `cypress/fixtures`.
- Cypress configuration is in `cypress.config.js`.
- The package currently uses JavaScript, not TypeScript.

## Test Authoring Rules

- Write tests from the user's point of view.
- Prefer stable `data-test` selectors.
- Use `cy.getByData("selector-name")` when available.
- Do not use brittle selectors such as long CSS chains, generated class names, or DOM indexes unless there is no better option.
- Do not add arbitrary waits like `cy.wait(1000)`. Wait on visible UI state, network aliases, or Cypress retryable assertions.
- Keep each test independent.
- Do not commit `it.only`, `describe.only`, `it.skip`, or `describe.skip` unless the user explicitly asks for it.
- Use clear test names that describe expected behavior.

## Preferred Test Style

```js
describe("page or feature name", () => {
  beforeEach(() => {
    cy.visit("/")
  })

  it("does something a user expects", () => {
    cy.getByData("example-selector")
      .should("be.visible")
      .and("contain", "Expected text")
  })
})
```

## Cypress Commands

If a spec uses `cy.getByData()`, make sure `cypress/support/commands.js` defines it:

```js
Cypress.Commands.add("getByData", (selector) => {
  return cy.get(`[data-test=${selector}]`)
})
```

## Configuration Guidance

If the application URL is stable, prefer setting `baseUrl` in `cypress.config.js` and using relative visits:

```js
cy.visit("/")
```

Avoid hardcoding the same localhost URL across many specs.

## Verification

After changing tests, run the most relevant Cypress command available:

```sh
npx cypress run
```

If the local app server is required and is not running, note that clearly in the final response.

## Documentation Updates

Update `TESTING.md` when changing test conventions, selector strategy, fixture strategy, or required test commands.
