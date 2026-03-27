---
name: import-cursor-skill
description: Converts a Cursor rule definition into a Claude Code skill (SKILL.md). Use when the user provides a cursor rule or asks to import a cursor rule/skill into Claude Code.
disable-model-invocation: true
argument-hint: [cursor-rule-text or file-path]
---

# Skill: Import Cursor Skill

Convert a Cursor rule definition into a Claude Code skill.

## Cursor Rule Format

Cursor rules look like this:

```
---
name: skill-name
description: What this skill does
---

# Instructions
...rule body...
```

They may also be provided as plain markdown without frontmatter, or as raw text pasted by the user.

## Conversion Steps

1. **Parse the input** from `$ARGUMENTS` or the current conversation:
   - If it's a file path, read the file
   - If it's inline text, use it directly
   - Extract: `name`, `description`, and the body content

2. **Derive the skill name**:
   - Use the `name` field from frontmatter if present
   - Otherwise, slugify the first heading or first line (lowercase, hyphens)

3. **Determine the install scope**:
   - Ask the user: project-level (`.claude/skills/`) or personal (`~/.claude/skills/`)?
   - Default to project-level if not specified

4. **Create the skill directory and SKILL.md**:
   - Path: `<scope>/<skill-name>/SKILL.md`
   - Write the file with proper frontmatter:

```
---
name: <skill-name>
description: <description>
---

<body content from cursor rule>
```

5. **Verify** the skill was imported correctly (mandatory):
   - Confirm the file exists at the expected path (`<scope>/<skill-name>/SKILL.md`)
   - Read the created file and confirm frontmatter has valid `name` and `description`
   - Confirm body content matches the original cursor rule (no additions or omissions)

6. **Report** to the user:
   - Full path of created skill
   - How to invoke it: `/skill-name`
   - Whether Claude will auto-invoke it (based on `disable-model-invocation`)

## How to verify

- Confirm the file exists at the expected path (`<scope>/<skill-name>/SKILL.md`)
- Read the created file and verify:
  - Frontmatter contains valid `name` and `description` fields
  - Body content matches the original cursor rule (no additions or omissions)
- Confirm the skill is discoverable by checking it appears under `.claude/skills/` or `~/.claude/skills/`
- Invoke the skill with `/skill-name` and confirm Claude Code recognizes it

## Rules

- Do NOT add `@ts-ignore`, extra comments, or boilerplate not present in the original rule
- Preserve all original instructions verbatim — only add/adjust YAML frontmatter
- If the cursor rule already has a `description`, use it as-is
- If no description exists, infer a concise one from the rule's content (1 sentence)
- Never overwrite an existing skill without user confirmation
