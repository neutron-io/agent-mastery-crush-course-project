---
name: e2e-testing
description: 'Core testing conventions for frontend behavior. Use when deciding between unit, component, and browser-level tests, or when structuring deterministic end-to-end coverage.'
argument-hint: 'A short description of the behavior to test'
user-invocable: false
---

# Core Testing Conventions

Choose the narrowest test that gives meaningful confidence in the behavior.

## Choose The Narrowest Test

- Use a pure unit test for deterministic utilities and business rules.
- Use Vue Test Utils for component rendering, emitted events, and user-visible state.
- Use a browser or end-to-end test only when the behavior depends on real browser APIs, routing, or cross-boundary integration that component tests cannot cover.

## Test Location And Naming

- Keep Vitest tests near the module they exercise under `frontend/src/`.
- Use the existing Vitest naming convention, such as `*.test.ts` or `*.spec.ts`, for Vitest only.
- Prefer accessible roles, labels, and visible text in component queries. Use stable selectors only when semantic queries cannot express the behavior.
- Name end-to-end tests after the feature or page flow rather than using generic names such as `smoke`.

## Test Structure

- Keep each test focused on one observable behavior.
- Separate logical phases with a blank line: setup, actions, and assertions should be visually distinct in each test.
- Create components with Vue Test Utils and provide only the dependencies required by the test.
- Stub network boundaries explicitly and keep test data deterministic.
- Assert rendered output, emitted events, and public component behavior rather than private implementation details.
- Group related assertions that describe one meaningful behavior. Do not split end-to-end tests into tiny one-assertion tests.
- Keep each test independently runnable. Establish shared state in setup hooks, not by relying on test order.

## Verification

Run the narrowest relevant command during development, then run the broader checks before committing. Frontend checks can be run from `frontend/`:

When `rtk` is installed, prefix terminal commands with `rtk`. If it is not installed, run the commands without the prefix.

```sh
npm run test:unit -- --run
npm run type-check
npm run build
```

Use the dedicated `e2e-testing-cypress` skill for Cypress selectors, fixtures, API interception, TypeScript configuration, and browser test conventions.
