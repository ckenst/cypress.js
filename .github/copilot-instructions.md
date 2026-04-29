# Copilot Instructions

This repository uses Cypress for end-to-end testing. When suggesting or editing code, follow these project conventions.

## Cypress Conventions

- Put specs in `cypress/e2e`.
- Use the `.cy.js` suffix for Cypress specs.
- Keep tests focused on user-visible behavior.
- Prefer `data-test` selectors and `cy.getByData()` over CSS classes or DOM structure.
- Do not generate `it.only`, `describe.only`, `it.skip`, or `describe.skip`.
- Do not use arbitrary waits such as `cy.wait(1000)`.
- Prefer retryable assertions like `should("be.visible")`, `should("contain", text)`, or waiting on aliased network calls.

## Preferred Pattern

```js
describe("feature name", () => {
  beforeEach(() => {
    cy.visit("/")
  })

  it("shows the expected content", () => {
    cy.getByData("hero-heading")
      .should("be.visible")
      .and("contain", "Expected text")
  })
})
```

## Selector Guidance

Use stable selectors that describe product behavior:

```html
<button data-test="submit-order">Submit order</button>
```

```js
cy.getByData("submit-order").click()
```

Avoid selectors tied to styling or layout, such as `.btn.primary`, `:nth-child()`, or generated class names.

## Configuration Guidance

Prefer a Cypress `baseUrl` in `cypress.config.js` when the app URL is known. Then use relative URLs in tests:

```js
cy.visit("/")
```

## Review Checklist

Before finalizing generated tests, check that:

- Selectors exist in the app markup
- Tests can run independently
- Assertions verify meaningful behavior
- No focused or skipped tests are present
- No secrets or local-only credentials are committed
