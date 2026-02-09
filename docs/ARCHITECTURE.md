# Architecture

## Overview

Three specialized Claude agents running on a dedicated Mac Mini via OpenClaw, accessible through Telegram 24/7.

## Agent Hierarchy

```
User (Telegram)
  └── OpenClaw Gateway (ws://127.0.0.1:18789)
        ├── BigDawg (Haiku 4.5)  ── default, cheapest, fastest
        ├── Coder (Opus 4.5)     ── code, deploy, test
        └── Brain (Opus 4.6)     ── research, strategy, planning
```

## Routing

OpenClaw uses **binding-based routing** (channel + peer), not keyword-based. All Telegram DMs route to BigDawg by default. To reach other agents:

- **CLI**: `openclaw agent --agent coder --message "..."`
- **Bindings**: Configure per-peer routing in `openclaw.json`
- **Subagents**: Agents can delegate to each other via `sessions_send`

## Cost Model

| Agent | Model | Input $/1M | Output $/1M | Use Case |
|-------|-------|-----------|------------|----------|
| BigDawg | Haiku 4.5 | $0.80 | $4.00 | 80% of messages |
| Coder | Opus 4.5 | $10.00 | $50.00 | Complex coding |
| Brain | Opus 4.6 | $15.00 | $75.00 | Deep thinking |

Optimizations applied:
- **Prompt caching**: "long" retention on Opus models (up to 90% savings)
- **Context pruning**: Auto-clear old tool results after 5 minutes
- **Compaction**: Memory flush before context summarization
- **Thinking default**: "low" across all agents, dial up when needed

## Data Flow

```
~/.openclaw/
├── openclaw.json          # Central config
├── .env                   # Secrets (VERCEL_TOKEN, LINEAR_API_KEY)
├── workspace/             # BigDawg (main agent)
│   ├── AGENTS.md, SOUL.md, IDENTITY.md, USER.md
│   ├── TOOLS.md, HEARTBEAT.md, MEMORY.md
│   └── skills/            # Main workspace skills
├── workspaces/
│   ├── coder/             # Coder agent workspace
│   │   ├── AGENTS.md, SOUL.md, IDENTITY.md, USER.md
│   │   ├── TOOLS.md, HEARTBEAT.md, MEMORY.md
│   │   └── skills/        # Coder-specific skills
│   └── brain/             # Brain agent workspace
│       ├── AGENTS.md, SOUL.md, IDENTITY.md, USER.md
│       ├── TOOLS.md, HEARTBEAT.md, MEMORY.md
│       └── skills/        # Brain-specific skills
└── agents/
    ├── main/sessions/     # Session transcripts
    ├── coder/agent/       # Auth profiles
    └── brain/agent/       # Auth profiles
```

## Skill Distribution

| Skill | BigDawg | Coder | Brain |
|-------|:-------:|:-----:|:-----:|
| vercel-deploy | x | x | |
| web-scraper | x | x | |
| deep-research-pro | x | | x |
| automation-workflows | x | | x |
| linear-issues | x | | x |
| playwright-testing | | x | |
| artifacts-builder | | x | |
| mcp-builder | | x | |
| doc-coauthoring | | | x |
