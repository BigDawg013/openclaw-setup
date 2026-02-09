---
name: artifacts-builder
description: Suite of tools for creating elaborate, multi-component claude.ai HTML artifacts using modern frontend web technologies (React, Tailwind CSS, shadcn/ui).
metadata: {"openclaw": {"emoji": "🎨"}}
---

# Artifacts Builder

Create complex React-based Claude.ai HTML artifacts.

## Stack

- React 18 + TypeScript
- Vite + Parcel bundler
- Tailwind CSS + shadcn/ui

## Workflow

1. Initialize project with `scripts/init-artifact.sh`
2. Develop components and logic
3. Bundle to single HTML with `scripts/bundle-artifact.sh`
4. Display as Claude artifact

## Agents

| Agent | Use Case |
|-------|----------|
| Coder | Build and bundle complex artifacts |
