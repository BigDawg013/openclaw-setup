# openclaw-setup

Multi-agent AI system powered by [OpenClaw](https://openclaw.ai/) and Anthropic Claude. Three specialized agents running 24/7 on a dedicated Mac Mini, accessible through Telegram.

---

### Agents

> **🦞 BigDawg** · Haiku 4.5 — Front desk. Routes messages, handles quick tasks, triages work to specialists.
>
> **💻 Coder** · Opus 4.5 — Engineer. Writes code, manages GitHub, deploys to Vercel, runs tests.
>
> **🧠 Brain** · Opus 4.6 — Strategist. Deep research, architecture decisions, sprint planning.

---

### How it works

All messages come in through Telegram and land on **BigDawg** — the cheapest, fastest model. It handles simple requests directly and delegates complex work:

- Code, deploy, or test something → **Coder**
- Research, plan, or analyze something → **Brain**

Cost stays low because you only pay for intelligence when you need it.

---

### Skills

Each agent has a focused skill set — no duplication where it doesn't belong.

**🦞 BigDawg** — vercel-deploy · web-scraper · deep-research-pro · automation-workflows · linear-issues

**💻 Coder** — vercel-deploy · web-scraper · playwright-testing · artifacts-builder · mcp-builder

**🧠 Brain** — deep-research-pro · automation-workflows · linear-issues · doc-coauthoring

All skills follow the [SKILL.md universal standard](https://agentskills.io/specification) — compatible with Claude, Cursor, GitHub Copilot, and Codex.

---

### Cost optimizations

| Optimization | What it does |
|---|---|
| Prompt caching | Long-retention cache on Opus models — up to 90% savings on system prompts |
| Context pruning | Auto-clears old tool results after 5 minutes |
| Memory flush | Persists important context to disk before compaction |
| Low thinking default | Agents start lean — dial up with `/think:high` when needed |
| Model fallbacks | Haiku → Sonnet → Opus chain if a model is unavailable |

---

### Structure

```
agents/
  bigdawg/          IDENTITY.md, SOUL.md
  coder/            IDENTITY.md, SOUL.md
  brain/            IDENTITY.md, SOUL.md
skills/
  linear/           SKILL.md
  vercel-deploy/    SKILL.md
  deep-research/    SKILL.md
  playwright-testing/  SKILL.md
docs/
  ARCHITECTURE.md   System design, cost model, data flow
  ORCHESTRATION.md  Agent coordination patterns
  SKILLS.md         Skill creation standards
```

---

### Docs

- **[Architecture](docs/ARCHITECTURE.md)** — System design, routing, cost model
- **[Orchestration](docs/ORCHESTRATION.md)** — How agents coordinate and delegate
- **[Skills](docs/SKILLS.md)** — How to create and assign skills

---

<sub>OpenClaw · Anthropic Claude · Telegram · Vercel · Linear · Playwright · GitHub</sub>
