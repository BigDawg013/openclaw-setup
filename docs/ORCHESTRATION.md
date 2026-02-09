# Orchestration

## How Agents Coordinate

OpenClaw agents operate independently but can coordinate through several mechanisms.

### 1. Routing (Automatic)

The gateway routes inbound messages to the right agent based on bindings:

```json
{
  "bindings": [
    {"agentId": "coder", "match": {"channel": "telegram", "peer": {"kind": "group", "id": "coding-group-id"}}},
    {"agentId": "brain", "match": {"channel": "telegram", "peer": {"kind": "group", "id": "research-group-id"}}}
  ]
}
```

Default: everything goes to BigDawg unless a binding says otherwise.

### 2. Agent-to-Agent Messaging

Agents can delegate tasks to each other via `sessions_send`:

```
BigDawg receives: "Deploy the latest code to production"
BigDawg delegates to Coder: "Deploy latest to Vercel production"
Coder executes and replies back
BigDawg relays result to user
```

Max ping-pong turns configurable via `session.agentToAgent.maxPingPongTurns`.

### 3. CLI Dispatch

Direct agent invocation from scripts or cron:

```bash
# Ask Brain to research something
openclaw agent --agent brain --message "Research best practices for MCP servers"

# Ask Coder to deploy
openclaw agent --agent coder --message "Deploy openclaw-setup to Vercel"

# Chain: Brain plans, Coder executes
PLAN=$(openclaw agent --agent brain --message "Plan the auth refactor" --json | jq -r '.reply')
openclaw agent --agent coder --message "Execute this plan: $PLAN"
```

### 4. Heartbeat Coordination

Each agent has HEARTBEAT.md for periodic tasks. Use for:
- BigDawg: Check inbox, calendar, notifications
- Coder: Run tests, check CI status, review open PRs
- Brain: Review memory, update long-term notes, check Linear

### 5. Shared Context

All agents share:
- `~/.openclaw/.env` — secrets and API keys
- `USER.md` — same user profile across all agents
- Auth profiles — same Anthropic API key

Each agent owns:
- Their workspace files (SOUL.md, MEMORY.md, etc.)
- Their skill set
- Their session history

## Anti-Patterns

- Don't create circular delegation (A -> B -> A)
- Don't duplicate skills across all agents — specialize
- Don't use Brain for simple tasks that Haiku can handle
- Don't bypass routing — let the gateway do its job
