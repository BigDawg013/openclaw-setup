<p align="center">
  <img src="https://img.shields.io/badge/AI-OpenClaw-ff6600?style=flat-square" alt="OpenClaw" />
  <img src="https://img.shields.io/badge/model-Claude%204.5%2F4.6-6c47ff?style=flat-square&logo=anthropic&logoColor=white" alt="Claude" />
  <img src="https://img.shields.io/badge/platform-Mac%20Mini-000000?style=flat-square&logo=apple&logoColor=white" alt="Mac Mini" />
  <img src="https://img.shields.io/badge/channel-Telegram-26a5e4?style=flat-square&logo=telegram&logoColor=white" alt="Telegram" />
  <img src="https://img.shields.io/badge/deploy-Vercel-000000?style=flat-square&logo=vercel&logoColor=white" alt="Vercel" />
  <img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" alt="MIT License" />
</p>

# openclaw-setup

A multi-agent AI system powered by [OpenClaw](https://openclaw.ai/) and Anthropic Claude. Three specialized agents running 24/7 on a dedicated Mac Mini, accessible through Telegram.

> One inbox. Three minds. Pay for intelligence only when you need it.

---

## Agents

| Agent | Model | Role | Handles |
|-------|-------|------|---------|
| **BigDawg** | Haiku 4.5 | Front desk | Triage, quick tasks, routing (~80% of messages) |
| **Coder** | Opus 4.5 | Engineer | Code, GitHub, Vercel deploys, Playwright tests |
| **Brain** | Opus 4.6 | Strategist | Deep research, architecture, sprint planning |

All messages land on **BigDawg** first — the cheapest, fastest model. It handles simple requests directly and delegates complex work to the right specialist:

- Code, deploy, or test something &#8594; **Coder**
- Research, plan, or analyze something &#8594; **Brain**

---

## Architecture

```
         You (anywhere)
              |
              | Telegram
              v
   +----------+-----------+
   |    OpenClaw Gateway   |
   |   Mac Mini · 24/7     |
   |   ws://localhost:18789 |
   +---+------+------+----+
       |      |      |
       v      v      v
    BigDawg  Coder  Brain
    Haiku    Opus   Opus
    4.5      4.5    4.6
       |             |
       |  Tailscale  |
       v  (encrypted) v
   +--------+   +--------+
   | clawpi |   | scout  |
   | Pi #2  |   | Pi #3  |
   | OpenClaw   | LED dash|
   +--------+   +--------+
```

Three machines on a [Tailscale](https://tailscale.com) mesh:

| Machine | Role | Repo |
|---------|------|------|
| **Mac Mini** | Primary AI gateway — 3 agents, 9 skills | this repo |
| **Raspberry Pi** (clawpi) | Secondary always-on gateway via Telegram | [clawpi-ai](https://github.com/BigDawg013/clawpi-ai) |
| **Raspberry Pi** (scout) | Health monitor with physical LED dashboard | [clawpi-scout](https://github.com/BigDawg013/clawpi-scout) |

---

## Skills

Each agent has a focused skill set — no duplication where it doesn't belong.

| Skill | BigDawg | Coder | Brain | Description |
|-------|:-------:|:-----:|:-----:|-------------|
| `vercel-deploy` | x | x | | Deploy and manage Vercel projects |
| `web-scraper` | x | x | | Extract structured data from websites |
| `deep-research-pro` | x | | x | Multi-source research with citations |
| `automation-workflows` | x | | x | Design and implement automations |
| `linear-issues` | x | | x | Manage Linear tickets and sprints |
| `playwright-testing` | | x | | E2E browser testing on deployments |
| `artifacts-builder` | | x | | Build React artifacts for Claude.ai |
| `mcp-builder` | | x | | Build MCP servers for LLM integrations |
| `doc-coauthoring` | | | x | Collaborative document creation |

All skills follow the [SKILL.md universal standard](https://agentskills.io/specification) — compatible with Claude, Cursor, GitHub Copilot, and Codex.

---

## Cost model

| Agent | Model | Input $/1M | Output $/1M | Traffic share |
|-------|-------|-----------|------------|---------------|
| BigDawg | Haiku 4.5 | $0.80 | $4.00 | ~80% |
| Coder | Opus 4.5 | $10.00 | $50.00 | ~15% |
| Brain | Opus 4.6 | $15.00 | $75.00 | ~5% |

Additional optimizations:

| Optimization | Effect |
|---|---|
| Prompt caching | Long-retention cache on Opus models — up to 90% savings on system prompts |
| Context pruning | Auto-clears old tool results after 5 minutes |
| Memory flush | Persists important context to disk before compaction |
| Low thinking default | Agents start lean — dial up with `/think:high` when needed |
| Model fallbacks | Haiku &#8594; Sonnet &#8594; Opus chain if a model is unavailable |

---

## Project structure

```
openclaw-setup/
├── agents/
│   ├── bigdawg/
│   │   ├── IDENTITY.md           # Model, role, emoji
│   │   └── SOUL.md               # Personality, boundaries, behavior
│   ├── coder/
│   │   ├── IDENTITY.md
│   │   └── SOUL.md
│   └── brain/
│       ├── IDENTITY.md
│       └── SOUL.md
├── skills/
│   ├── vercel-deploy/SKILL.md
│   ├── web-scraper/SKILL.md
│   ├── deep-research-pro/SKILL.md
│   ├── automation-workflows/SKILL.md
│   ├── linear-issues/SKILL.md
│   ├── playwright-testing/SKILL.md
│   ├── artifacts-builder/SKILL.md
│   ├── mcp-builder/SKILL.md
│   └── doc-coauthoring/SKILL.md
├── docs/
│   ├── ARCHITECTURE.md           # System design, routing, cost model
│   ├── ORCHESTRATION.md          # Agent coordination patterns
│   └── SKILLS.md                 # Skill creation standards
└── README.md
```

---

## How agents coordinate

**Routing** — The gateway routes messages based on channel + peer bindings. All Telegram DMs go to BigDawg by default. Group chats can be bound to specific agents.

**Delegation** — Agents pass tasks to each other via `sessions_send`. BigDawg triages inbound work and delegates to Coder or Brain as needed, then relays the result back.

**CLI dispatch** — Direct agent invocation from scripts or cron:

```bash
openclaw agent --agent brain --message "Research MCP server best practices"
openclaw agent --agent coder --message "Deploy to Vercel production"
```

**Heartbeats** — Each agent runs periodic tasks via HEARTBEAT.md (check inbox, run tests, review PRs, update memory).

See [docs/ORCHESTRATION.md](docs/ORCHESTRATION.md) for the full coordination guide.

---

## Docs

| Document | What it covers |
|----------|---------------|
| [Architecture](docs/ARCHITECTURE.md) | System design, routing, cost model, data flow (with Mermaid diagrams) |
| [Orchestration](docs/ORCHESTRATION.md) | Agent coordination, delegation patterns, CLI dispatch |
| [Skills](docs/SKILLS.md) | SKILL.md standard, how to create and assign skills |

---

## Related

- **[clawpi-ai](https://github.com/BigDawg013/clawpi-ai)** — OpenClaw on a Raspberry Pi (always-on secondary gateway)
- **[clawpi-scout](https://github.com/BigDawg013/clawpi-scout)** — GPIO health monitor with LEDs, LCD, bar graph, 7-segment, dot matrix
- **[OpenClaw](https://openclaw.ai)** — The multi-agent AI platform

---

## License

[MIT](LICENSE)
