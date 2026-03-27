---
name: generate-component-tests
description: Analyzes staged git changes to find modified components in packages/excalidraw/components/, then generates colocated Vitest tests following project conventions. Use when the user asks to generate tests, write tests for staged changes, or test new components.
---

# Skill: Generate Component Tests

## When to use

When the user asks to generate or write tests for staged component changes, or for a specific named component.
Triggered by: "generate tests", "write tests for staged changes", "test my components", "generate tests for <ComponentName>".

## Inputs

- **Component name** (optional, from skill args): if provided, resolve the component file directly — skip git diff entirely
- Staged git changes (auto-detected via `git diff --cached`) — used only when no component name is given

## Steps

### 1. Identify target components

**If a component name was provided as an argument** (e.g. `/generate-component-tests LayerUI`):
- Search `packages/excalidraw/components/` for a file matching `<ComponentName>.tsx` (case-sensitive)
- Use that file as the sole target; skip `git diff`
- If the file is not found, tell the user and stop

**Otherwise (no argument)**:
- Run `git diff --cached --name-only` to get staged files. Filter for files matching:
  `packages/excalidraw/components/**/*.tsx` (exclude existing `*.test.tsx` files)
- If no staged component files are found, tell the user and stop.

### 2. Read and understand each target component

For each target component file:
- Read the full file to understand its props, behavior, and dependencies
- If the target was found via staged changes (not a named argument), also read the staged diff (`git diff --cached <file>`) to understand what specifically changed and focus tests there
- Check if a colocated test file already exists (`ComponentName.test.tsx`)
  - If yes: read it and ADD new test cases for the changed/untested behavior
  - If no: create a new test file from scratch

### 3. Fetch documentation when needed

Use the **context7 MCP** to look up documentation when:
- The component uses a library API you are unsure about (e.g., Radix UI, React APIs)
- You need to verify correct testing-library query usage
- You encounter an unfamiliar pattern

To use context7:
1. First call `mcp__plugin_context7_context7__resolve-library-id` with the library name (e.g., "vitest", "@testing-library/react")
2. Then call `mcp__plugin_context7_context7__query-docs` with the resolved ID and your specific question

### 4. Generate test file

Create the test file colocated with the component: `ComponentName.test.tsx`

**Follow these project conventions strictly:**

#### Imports pattern
```typescript
import React from "react";
import { vi } from "vitest";

// Project imports
import { Excalidraw } from "../../index";
import {
  act,
  fireEvent,
  render,
  waitFor,
  queryByTestId,
} from "../../tests/test-utils";

// Component under test
import { ComponentName } from "./ComponentName";
```

#### Test structure pattern
```typescript
describe("ComponentName", () => {
  beforeEach(async () => {
    // Reset state between tests
  });

  it("should [specific behavior]", async () => {
    const { container } = await render(
      <Excalidraw>
        <ComponentName {...props} />
      </Excalidraw>,
    );

    // Assertions
  });
});
```

#### Key rules
- **Vitest only**: use `vi.fn()`, `vi.spyOn()` — NEVER `jest.*`
- **Rendering**: use `render()` from `../../tests/test-utils` — NOT from `@testing-library/react` directly
  - Exception: simple leaf components that don't need Excalidraw context (like `<Trans/>`) may use `render` from `@testing-library/react` with `<EditorJotaiProvider>` wrapper
- **State access**: use `window.h` for app state in tests
- **Interactions**: use `Pointer`, `UI`, `Keyboard` helpers from `../../tests/helpers/ui`
- **Async**: wrap state changes in `act()`, use `waitFor()` for async assertions
- **Snapshot testing**: use `toMatchSnapshot()` for rendered UI output where appropriate
- **Named exports only**: no default exports in test files
- **i18n**: if the component uses `t()`, verify translation keys exist in `en.json`

### 5. Verify the generated tests

After writing tests, run:
```bash
yarn test -- --run <path-to-test-file>
```

If tests fail:
1. Read the error output
2. Fix the test (NOT the component code)
3. Re-run (max 3 attempts)

If tests still fail after 3 attempts, report the failure to the user with the error output.

## Outputs

- New or updated `*.test.tsx` files colocated with changed components
- Test run results (pass/fail)
- Summary of what was tested

## How to verify

- `yarn test -- --run <test-file>` passes
- `yarn test:typecheck` reports no errors in new test files
- No `jest.fn` or `jest.spyOn` in generated code
- Tests are colocated next to their component
- Test file uses project import patterns (not raw `@testing-library/react` for app-level components)

## Safety

- Do NOT modify component source code — only create/update test files
- Do NOT modify protected files (see `.claude/rules/protected-files.md`)
- If a component is too complex to test reliably, tell the user and suggest which parts can be tested
- Do NOT add new npm dependencies for testing
