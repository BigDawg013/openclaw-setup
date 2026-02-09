# BigDawg's AI Command Center

Personal AI assistant powered by [OpenClaw](https://openclaw.ai/) — running on a dedicated Mac Mini with a multi-agent architecture designed for real work.

## What Is This

This is my always-on AI setup. Three specialized agents working through Telegram, each with their own model, personality, and skill set. One brain, many hands.

## The Agents

| Agent | Model | Role | Emoji |
|-------|-------|------|-------|
| **BigDawg** | Claude Haiku 4.5 | Front desk — routing, quick tasks, general conversation | `🦞` |
| **Coder** | Claude Opus 4.5 | Software engineer — writes code, manages repos, deploys | `💻` |
| **Brain** | Claude Opus 4.6 | Strategist — deep research, architecture, planning | `🧠` |

### Why Three Agents?

Cost efficiency meets capability. Most messages hit BigDawg on Haiku (fast and cheap). Complex coding goes to Coder on Opus 4.5. Deep thinking goes to Brain on Opus 4.6. You only pay for the intelligence you need.

## Architecture

```
Telegram --> OpenClaw Gateway (ws://127.0.0.1:18789)
                |
                ├── BigDawg (Haiku) ---- everyday tasks, routing
                ├── Coder (Opus 4.5) --- code, GitHub, Vercel, testing
                └── Brain (Opus 4.6) --- research, strategy, planning
```

- **Gateway**: Local WebSocket gateway running as a macOS LaunchAgent
- **Channel**: Telegram (more channels coming)
- **Auth**: Anthropic API keys with model fallback chain (Haiku -> Sonnet -> Opus)
- **Hosting**: Dedicated Mac Mini — always on, always ready

## Skills

### BigDawg (Main)
- `automation-workflows` — Design and run automations
- `deep-research-pro` — Web research with citations
- `vercel-deploy` — Deploy projects to Vercel
- `web-scraper` — Scrape and extract web data

### Coder
- `vercel-deploy` — Deploy and manage Vercel projects
- `web-scraper` — Scrape data for projects
- `playwright-testing` — Test deployed apps with Playwright
- `artifacts-builder` — Rapid React + TypeScript + Tailwind prototyping
- `mcp-builder` — Build MCP servers for external integrations

### Brain
- `deep-research-pro` — Multi-source research with citations
- `automation-workflows` — Design maintainable automations
- `doc-coauthoring` — Structured documentation workflows

### Bundled (All Agents)
GitHub, ClawHub, Skill Creator, Coding Agent, Healthcheck, Weather, and more.

## Optimizations

This setup is tuned for cost and performance:

- **Prompt Caching**: Long-retention caching on expensive models (up to 90% savings on system prompts)
- **Context Pruning**: Automatic soft-trim and hard-clear of old tool results after 5 minutes
- **Compaction**: Memory flush before context summarization so nothing important is lost
- **Thinking Levels**: Default `low` across agents — dial up with `/think:high` when needed
- **Model Fallbacks**: If one model is down, automatically falls back to the next

## Tech Stack

- **Platform**: [OpenClaw](https://openclaw.ai/) v2026.2.6-3
- **Models**: Anthropic Claude (Haiku 4.5, Opus 4.5, Opus 4.6)
- **Channel**: Telegram
- **Deployment**: Vercel
- **Code**: GitHub via `gh` CLI
- **Runtime**: Node.js on macOS (Mac Mini)

## Setup

> This is a personal setup. If you want your own, check [OpenClaw's getting started guide](https://docs.openclaw.ai/start/getting-started).

```bash
# Install OpenClaw
npm i -g openclaw

# Run the onboarding wizard
openclaw onboard --install-daemon

# Add your API keys, channels, and agents
# See OpenClaw docs for details
```

## About Me

I'm BigDawg — entrepreneur, full-stack developer, and builder. I use AI as a force multiplier, not a replacement. This setup lets me move fast on projects while keeping costs under control.

- GitHub: [@BigDawg013](https://github.com/BigDawg013)
- Powered by: [OpenClaw](https://openclaw.ai/) + [Anthropic Claude](https://www.anthropic.com/)

---

*Built with OpenClaw. Ship things that matter.*
