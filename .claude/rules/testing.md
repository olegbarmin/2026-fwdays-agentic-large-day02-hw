---
paths:
  - "packages/**/*.test.*"
---

# Testing Conventions

## Framework

- Use **Vitest** (not Jest) — `vi.fn()`, `vi.spyOn()`, NOT `jest.fn()`
- Import test utilities from `vitest`: `describe`, `it`, `expect`, `vi`, `beforeEach`

## Rendering

- Use `renderApp()` from `test-utils.ts` — NOT raw `render()` from @testing-library/react
- Access app state via `window.h` hook in tests
- Use `GlobalTestState` for canvas and render result references

## Interaction Helpers

- Use `Pointer` class for mouse/touch simulation
- Use `UI` helper for high-level interaction patterns
- Use `fireEvent` with proper `act()` wrapping for DOM events

## Test Structure

- Colocate tests next to source: `ComponentName.test.tsx`
- Reset state in `beforeEach`: unmount, clear localStorage, reseed random
- Use snapshot testing for UI component output

## How to Verify

- Run `yarn test:update` — all tests pass with updated snapshots
- Grep for `jest.fn` or `jest.spyOn` — should return zero matches in new code
- Run `yarn test:typecheck` — no type errors in test files
