# skills-and-prompts

A collection of reusable agent skills and prompts.

## Layout

```
skills/    one folder per skill, each with a SKILL.md
prompts/   standalone prompts, one Markdown file each
```

## Skills

Each skill lives in its own folder under `skills/`:

```
skills/
  my-skill/
    SKILL.md        required: frontmatter + instructions
    scripts/        optional helper scripts
    references/     optional supporting docs
```

`SKILL.md` starts with YAML frontmatter:

```markdown
---
name: my-skill
description: What the skill does and when it should be used.
---

Instructions go here.
```

To use a skill with Claude Code, copy its folder into `~/.claude/skills/` (personal) or `.claude/skills/` in a project.

## Prompts

Each prompt is a single Markdown file under `prompts/`, named for what it does (for example `prompts/code-review.md`). Put a one-line summary at the top, then the prompt itself.
