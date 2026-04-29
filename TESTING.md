# Testing Guide

This project uses Cypress for end-to-end tests.

## Test Scope

Use Cypress for browser-level user flows and visible page behavior. Prefer tests that verify what a user can see or do rather than implementation details.

Good Cypress test candidates:

- Page loads and primary content appears
- Navigation and links work
- Forms accept valid input and show validation errors
- Important UI states render correctly
- Critical user paths remain functional

Avoid Cypress for:

- Pure utility functions
- Component internals that are better covered by unit or component tests
- Styling details that are not meaningful user-facing behavior

## Running Tests

Install dependencies first:

```sh
npm install
```

Open Cypress interactively:

```sh
npx cypress open
```

Run Cypress headlessly:

```sh
npx cypress run
```

The application under test is expected to be available locally before running the specs. Update `cypress.config.js` with a `baseUrl` when the app URL is stable.

## Conventions

- Place end-to-end specs in `cypress/e2e`.
- Name specs with the `.cy.js` suffix.
- Keep tests independent; one test should not rely on another test's state.
- Use `beforeEach` for shared setup such as visiting a page.
- Prefer stable `data-test` attributes over CSS classes, text fragments, or DOM position.
- Do not commit focused tests such as `it.only`, `describe.only`, `it.skip`, or `describe.skip` unless the skip is intentional and documented.

## Selectors

Prefer custom commands for test selectors:

```js
cy.getByData("hero-heading")
```

This should map to markup like:

```html
<h1 data-test="hero-heading">...</h1>
```

Use selector names that describe the user's concept, not the implementation. For example, prefer `hero-heading` over `large-blue-title`.

## Test Structure

Use a simple arrange, act, assert shape:

```js
describe("home page", () => {
  beforeEach(() => {
    cy.visit("/")
  })

  it("shows the hero heading", () => {
    cy.getByData("hero-heading")
      .should("be.visible")
      .and("contain", "Testing Next.js Applications with Cypress")
  })
})
```

## Fixtures

Put reusable test data in `cypress/fixtures`. Keep fixture data small, explicit, and named after the behavior or API it supports.

## AI-Assisted Test Writing

When using AI to write tests, provide the assistant with:

- The user behavior being tested
- The relevant page or component markup
- Existing selector conventions
- The command used to run Cypress
- Any required local server URL or environment variables

AI-generated tests should be reviewed for:

- Real selectors that exist in the app
- No focused or skipped tests
- No arbitrary waits such as `cy.wait(5000)`
- Assertions that verify meaningful behavior
- Independence from previous test state
