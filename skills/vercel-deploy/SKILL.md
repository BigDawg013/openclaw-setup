---
name: vercel-deploy
description: Deploy projects to Vercel, manage deployments, and configure environment variables. Use when deploying web apps, checking deployment status, or managing Vercel projects.
metadata: {"openclaw": {"emoji": "▲", "requires": {"bins": ["vercel"], "env": ["VERCEL_TOKEN"]}, "primaryEnv": "VERCEL_TOKEN"}}
---

# Vercel Deploy

Deploy and manage Vercel projects via the CLI.

## Operations

- **Deploy preview** — `vercel --token $VERCEL_TOKEN`
- **Deploy production** — `vercel --prod --token $VERCEL_TOKEN` (requires confirmation)
- **List deployments** — `vercel ls --token $VERCEL_TOKEN`
- **Environment variables** — `vercel env ls/add/rm --token $VERCEL_TOKEN`
- **Logs** — `vercel logs <url> --token $VERCEL_TOKEN`

## Agents

| Agent | Use Case |
|-------|----------|
| Coder | Deploy code, manage env vars, check logs |
| BigDawg | Quick deployment status checks |
