# AGENTS.md

## Project Structure

Excalidraw is a **monorepo** with a clear separation between the core library and the application:

- **`packages/excalidraw/`** - Main React component library published to npm as `@excalidraw/excalidraw`
- **`excalidraw-app/`** - Full-featured web application (excalidraw.com) that uses the library
- **`packages/`** - Core packages: `@excalidraw/common`, `@excalidraw/element`, `@excalidraw/math`, `@excalidraw/utils`
- **`examples/`** - Integration examples (NextJS, browser script)

## Development Workflow

1. **Package Development**: Work in `packages/*` for editor features
2. **App Development**: Work in `excalidraw-app/` for app-specific features
3. **Testing**: Always run `yarn test:update` before committing
4. **Type Safety**: Use `yarn test:typecheck` to verify TypeScript

## Development Commands


<CodeBlockWrapper v-bind="{}" :ranges='[]'>

```bash
yarn test:typecheck  # TypeScript type checking
yarn test:update     # Run all tests (with snapshot updates)
yarn fix             # Auto-fix formatting and linting issues
```

</CodeBlockWrapper>

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
