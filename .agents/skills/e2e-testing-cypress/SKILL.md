---
name: e2e-testing-cypress
description: 'Cypress testing conventions for Vue frontends. Use when writing or reviewing Cypress browser tests, selectors, intercepts, fixtures, or Cypress TypeScript setup.'
argument-hint: 'A short description of the Cypress behavior to test'
user-invocable: false
---

# Cypress Testing Conventions

Use Cypress for browser-level coverage of user flows and behavior that depends on real browser APIs, routing, or cross-boundary integration.

## Selectors And Assertions

- Organize Cypress specs around features and user flows, such as `login-page.cy.ts` or `public-registration.cy.ts`; do not use generic names such as `smoke.cy.ts`.
- Name every root `describe` suite `"{page/feature} test suite"`, for example `describe('Login page test suite', () => {`.
- Always prefer a stable `data-testid` and `cy.getByTestID(TestID.SomeElement)` when one exists or can be added reasonably.
- Use accessible roles, labels, and meaningful visible text when no suitable test ID exists or when the text itself is the behavior under test.
- Use narrowly scoped `data-testid` attributes when a stable selector does not already exist. Cypress's official guidance recommends resilient `data-*` selectors isolated from CSS and JavaScript changes; this project standardizes on `data-testid`.
- Use the typed `getByTestID` command for `data-testid` selectors instead of calling `cy.get()` directly:

    ```ts
    Cypress.Commands.add('getByTestID', (testID: string) => {
      return cy.get(`[data-testid="${testID}"]`);
    });
    ```

- Declare the custom command in the Cypress type entry point:

    ```ts
    declare namespace Cypress {
      interface Chainable {
        getByTestID(testID: string): Chainable<JQuery<HTMLElement>>;
      }
    }
    ```

- Treat `cy.getByTestID(TestID.SomeFeatureElement)` as the default selector and assertion style:

    ```ts
    cy.getByTestID(TestID.AnnouncementBannerCloseButton).should('be.visible').click();
    ```

- Keep `TestID` values feature/page-specific and explicit. Do not create a catch-all selector registry.
- Keep the `TestID` object sorted alphabetically by property name, with a blank line between groups whose first letter changes, such as between `Login...` and `Public...`.
- Use `cy.contains()` when a text change should fail the test. Use `getByTestID` when the text is incidental and should not couple the test to copy.
- Group related assertions that describe one meaningful behavior. Do not split end-to-end tests into tiny one-assertion tests.
- Keep each test independently runnable. Establish shared state in `beforeEach`, not by relying on test order.
- Use route aliases and visible assertions for synchronization. Keep `cy.wait(...)` on its own logical line with a blank line before and after it when adjacent to assertions. Never use arbitrary `cy.wait(<number>)` calls.

## API Boundaries

- Always stub application API requests with `cy.intercept()` and deterministic responses. Cypress tests must not depend on a live backend, production data, Azure Key Vault, or third-party APIs.
- Store reusable interception responses in JSON fixtures under `frontend/cypress/fixtures/` and load them with `cy.fixture()`.
- Keep reusable public registration interception configuration in `frontend/cypress/support/public-registration-test-config.ts`. Export the PascalCase objects `ApiEndpoints`, `Aliases`, and `FixtureNames` from that file and reuse them in specs instead of repeating endpoint patterns, route aliases, or fixture paths.
- Alias intercepted routes and wait for the alias only when the user action requires a response before the next assertion.
- Keep external authentication and third-party integrations outside the browser flow unless an approved test seam exists.
- Keep secrets out of specs. Use `Cypress.env()` for sensitive test values and expose only non-sensitive configuration to the browser.

## TypeScript Setup

- Keep Cypress source under `frontend/cypress/` and configure it with `frontend/cypress/tsconfig.json`.
- Include `"types": ["cypress", "node"]` in the Cypress tsconfig so `cy`, `Cypress`, and config types are available without leaking Cypress globals into application or Vitest projects.
- Add a `/// <reference types="cypress" />` directive to Cypress support/type entry points when the editor needs explicit global type resolution.

## Test Location And Naming

- Keep Cypress specs under `frontend/cypress/e2e/` and name them after the feature or page flow, using `*.cy.ts`.
- Follow the repository's existing file naming conventions. If the repository does not define one, use `camelCase.cy.ts`.
- Use `TestID` enum values for selectors instead of hardcoded strings.
- Follow the fixture pattern already used in `frontend/cypress/fixtures/`.

## Verification

Run frontend commands from `frontend/`:

When `rtk` is installed, prefix terminal commands with `rtk`. If it is not installed, run the commands without the prefix.

Use these package scripts for Cypress:

```json
{
  "cy": "cypress open",
  "cy-run": "cypress run",
  "cy-ci": "start-server-and-test dev:ci {port} cy-run"
}
```

- Use `npm run cy` for interactive local development.
- Use `npm run cy-run` for a headless Cypress run against an already running app.
- Use `npm run cy-ci` to start the CI development server, wait for `{port}`, and run Cypress.

```sh
npm run test:unit -- --run
npm run type-check
npm run build
```

Run the narrowest relevant command during development, then run the broader checks before committing.
