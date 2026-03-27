# Evidence: GitHub MCP Access

## What was done

Claude Code was asked to check the comments under a GitHub PR using the GitHub MCP plugin:

> Check the comments under this PR:
> https://github.com/koldovsky/2026-fwdays-agentic-large-day02-hw/pull/7

Claude invoked the `plugin:github:github` MCP tool and retrieved the CodeRabbit review results from PR #7.

## Result returned via MCP

**Pre-merge checks: 4 passed, 3 failed**

Failed (3 warnings):
1. **Minimum 6 Rules** — 6 rules exist in `.claude/rules/`, but only 3 have the required `How to verify` section. Missing from: `architecture.md`, `code-conventions.md`, `protected-files.md`.
2. **Minimum 2 Custom Commands** — 5 command files in `.claude/skills/`, but in the wrong directory. Checker expects `.cursor/commands/`.
3. **AGENTS.md Complete** — Only 2 of 7 required sections present. Missing: Project Overview, Tech Stack, Key Commands, Conventions, Do-Not-Touch / Constraints. Also contains a `<CodeBlockWrapper>` artifact.

Passed (4 checks):
- Description Check — Skipped (CodeRabbit summary enabled)
- Title Check — Correct format "Day 2: Oleg Barmin — Workshop Assignment"
- Docstring Coverage — No functions to evaluate
- A/B Validation Document — All 5 required elements present

## Conclusion

Claude Code successfully accessed GitHub via the MCP plugin (`plugin:github:github`) and retrieved live PR review data without any manual API calls or tokens provided by the user.

## Original Conversation Log

```
 ▐▛███▜▌   Claude Code v2.1.84
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/…/2026-fwdays-agentic-large-day02-hw

❯ /plugin
  ⎿  (no content)

❯ Check the comments under this PR:
https://github.com/koldovsky/2026-fwdays-agentic-large-day02-hw/pull/7

  Queried plugin:github:github (ctrl+o to expand)

⏺ Here's a summary of the CodeRabbit review on PR #7:

  Pre-merge checks: 4 passed, 3 failed

  Failed (3 warnings):

  1. Minimum 6 Rules - 6 rules exist in .claude/rules/, but only 3 have the
  required How to verify section. Missing from: architecture.md,
  code-conventions.md, protected-files.md.
  2. Minimum 2 Custom Commands - You have 5 command files in .claude/skills/,
  but they're in the wrong directory. The checker expects them in
  .cursor/commands/ (which doesn't exist).
  3. AGENTS.md Complete - Only 2 of 7 required sections are present. Missing:
  Project Overview, Tech Stack, Key Commands, Conventions, Do-Not-Touch /
  Constraints. Also has a <CodeBlockWrapper> artifact that should be plain
  markdown.

  Passed (4 checks):

  - Description Check - Skipped (CodeRabbit summary enabled)
  - Title Check - Correct format "Day 2: Oleg Barmin — Workshop Assignment"
  - Docstring Coverage - No functions to evaluate
  - A/B Validation Document - All 5 required elements present

  ---
  Want me to fix any of these issues? The most impactful ones are:
  1. Add How to verify sections to the 3 rule files
  2. Expand AGENTS.md with the missing sections
```
