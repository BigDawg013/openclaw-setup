---
name: linear-issues
description: Manage Linear issues — create, update, search, and triage tickets. Use when working with project management, sprint planning, or issue tracking.
metadata: {"openclaw": {"emoji": "📋", "requires": {"env": ["LINEAR_API_KEY"]}, "primaryEnv": "LINEAR_API_KEY"}}
---

# Linear

Manage Linear issues via the GraphQL API.

## Available Operations

- **issues --mine** — List your assigned issues
- **issues --team ID** — List team issues
- **get ID** — Get issue details
- **search "query"** — Search issues
- **create** — Create a new issue (requires --team and --title)
- **update ID** — Update an issue (status, title, assignee, priority)
- **comment ID "text"** — Add a comment
- **teams** — List workspace teams
- **states** — List workflow states
- **users** — List workspace members

## Quick Reference

Priority levels: 0=none, 1=urgent, 2=high, 3=normal, 4=low

## Agents

| Agent | Use Case |
|-------|----------|
| BigDawg | Quick ticket creation, status checks |
| Brain | Sprint planning, brainstorming, triage |
