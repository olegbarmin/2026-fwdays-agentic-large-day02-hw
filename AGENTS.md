# AGENTS.md

## Project Overview

Excalidraw is an open-source virtual whiteboard for sketching hand-drawn-like diagrams. This repo is a **monorepo** containing both the core library (`@excalidraw/excalidraw`) and the full web application (excalidraw.com).

## Project Structure

- **`packages/excalidraw/`** - Main React component library published to npm as `@excalidraw/excalidraw`
- **`excalidraw-app/`** - Full-featured web application (excalidraw.com) that uses the library
- **`packages/`** - Core packages: `@excalidraw/common`, `@excalidraw/element`, `@excalidraw/math`, `@excalidraw/utils`
- **`examples/`** - Integration examples (NextJS, browser script)

## Tech Stack

- **Language**: TypeScript (strict mode)
- **UI Framework**: React (functional components + hooks)
- **Rendering**: Canvas 2D API (not React DOM for drawing)
- **Build**: esbuild for packages, Vite for the app
- **Monorepo**: Yarn workspaces
- **Testing**: Vitest
- **State Management**: Custom actionManager (not Redux/Zustand/MobX)

## Key Commands

```bash
yarn test:typecheck  # TypeScript type checking
yarn test:update     # Run all tests (with snapshot updates)
yarn fix             # Auto-fix formatting and linting issues
```

## Conventions

- Functional components + hooks ONLY (no class components)
- Props interface: `{ComponentName}Props`
- Named exports only (no default exports)
- `type` over `interface` for simple types
- Import types: `import type { X } from "..."`
- kebab-case for files: `element-utils.ts`
- PascalCase for components: `LayerUI.tsx`
- Colocated tests: `ComponentName.test.tsx`

## Do-Not-Touch / Constraints

NEVER modify these files without explicit approval:

- `packages/excalidraw/scene/renderer.ts` — render pipeline
- `packages/excalidraw/data/restore.ts` — file format compatibility
- `packages/excalidraw/actions/manager.ts` — action system core
- `packages/excalidraw/types.ts` — core type definitions

Additional constraints:

- No new npm packages without explicit approval
- No Redux/Zustand/MobX — use actionManager only
- No react-konva/fabric.js/pixi.js — Canvas 2D only

## Development Workflow

1. **Package Development**: Work in `packages/*` for editor features
2. **App Development**: Work in `excalidraw-app/` for app-specific features
3. **Testing**: Always run `yarn test:update` before committing
4. **Type Safety**: Use `yarn test:typecheck` to verify TypeScript

## Architecture Notes

### Package System

- Uses Yarn workspaces for monorepo management
- Internal packages use path aliases (see `vitest.config.mts`)
- Build system uses esbuild for packages, Vite for the app
- TypeScript throughout with strict configuration

### Memory Bank

The memory bank is located in `docs/memory/` and contains:

- **`projectbrief.md`** - High-level project overview and goals
- **`productContext.md`** - Product requirements, features, and user needs
- **`techContext.md`** - Technical architecture, stack, and design decisions
- **`systemPatterns.md`** - Coding patterns, conventions, and best practices
- **`activeContext.md`** - Current work in progress, active issues, and blockers
- **`decisionLog.md`** - Record of major decisions made during development
- **`progress.md`** - Development progress, milestones, and status

These documents serve as reference material for agents working on the project, providing context for understanding the codebase, requirements, and development approach.
