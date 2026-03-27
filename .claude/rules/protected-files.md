---
paths:
  - "packages/excalidraw/**"
alwaysApply: true
---

# Protected Files

NEVER modify these files without explicit approval:

- `packages/excalidraw/scene/renderer.ts` — render pipeline
- `packages/excalidraw/data/restore.ts` — file format compat
- `packages/excalidraw/actions/manager.ts` — action system
- `packages/excalidraw/types.ts` — core types

Changes to protected files require:

1. Full understanding of dependencies
2. Running complete test suite
3. Manual QA verification

## How to verify
- Audit git diff for changes to protected files: `git diff --name-only | grep -E "renderer\.ts|restore\.ts|manager\.ts|types\.ts"`
- Run full test suite: `yarn test:update`
- Confirm manual QA sign-off before merging
