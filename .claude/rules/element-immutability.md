---
paths:
  - "packages/element/**"
---

# Element Immutability

## Core Principle

All elements are `Readonly<>` at the type level. NEVER mutate element properties directly.

## Creating Elements

- Use factory functions: `newElement()`, `newTextElement()`, `newArrowElement()`, etc.
- Defaults are applied via `DEFAULT_ELEMENT_PROPS`
- Never construct element objects manually with object literals

## Mutating Elements

- Use `mutateElement(element, elementsMap, updates)` for all changes
- `mutateElement` handles versioning, side effects, and arrow recalculation
- Never modify `id`, `updated`, `version`, or `versionNonce` manually
- For React-aware updates, use `scene.mutateElement()` or the imperative API

## Type Safety

- Use type guards: `isTextElement()`, `isLinearElement()`, `isFrameElement()`, etc.
- DO NOT use `as` type casting on element types
- Use `NonDeleted<T>` for filtered element collections
- Use `Mutable<T>` only inside mutation helpers, never in component code

## How to Verify

- Run `yarn test:typecheck` — no type errors related to element mutation
- Grep for direct property assignments on elements (e.g., `element.x =`) — should find none in new code
- Run `yarn test:update` — element-related tests pass
