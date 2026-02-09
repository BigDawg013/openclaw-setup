# Skills Standard

This repo follows the **SKILL.md universal standard** adopted by Claude, Cursor, GitHub Copilot, and OpenAI Codex.

## Creating a New Skill

### File Structure

```
skill-name/
├── SKILL.md          # Required: YAML frontmatter + instructions
├── scripts/          # Optional: executable utilities
├── references/       # Optional: detailed documentation
└── assets/           # Optional: templates, data files
```

### SKILL.md Format

```yaml
---
name: skill-name
description: What it does and when to use it
metadata: {"openclaw": {"emoji": "🔧", "requires": {"bins": ["tool"], "env": ["API_KEY"]}}}
---

# Skill Name

Instructions for the agent. Keep under 500 lines.
Move detailed content to references/.
```

### Key Rules

1. **name**: Lowercase, hyphens only, max 64 characters, must match directory name
2. **description**: Include WHAT it does AND WHEN to use it — this is how agents discover skills
3. **metadata**: Must be single-line JSON
4. **Body**: Keep under 500 lines. Use `references/` for detailed docs
5. **Scripts**: Executable code that runs without loading into context (saves tokens)

### Progressive Disclosure

- **Level 1**: name + description (~100 tokens, always loaded)
- **Level 2**: SKILL.md body (<5k tokens, loaded when triggered)
- **Level 3**: references/ + scripts/ (loaded as needed)

### Naming Conventions

- `verb-noun`: `deploy-vercel`, `test-playwright`
- `domain-action`: `linear-issues`, `github-prs`
- Avoid: `helper`, `utils`, `tools` (too vague)

## Installing Skills

```bash
# From ClawHub
clawhub install skill-name
clawhub install skill-name --workdir ~/.openclaw/workspaces/coder

# From this repo (copy to workspace)
cp -r skills/linear ~/.openclaw/workspaces/brain/skills/
```

## Skill Assignment

Not every agent needs every skill. Specialize:

- **BigDawg**: Quick ops skills (weather, reminders, basic linear)
- **Coder**: Build tools (vercel, playwright, github, mcp-builder)
- **Brain**: Research tools (deep-research, linear planning, doc-coauthoring)
