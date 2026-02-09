# Scout Node

External monitoring for the OpenClaw system via a Raspberry Pi.

## Overview

The scout is a lightweight daemon running on a separate device (Raspberry Pi) that monitors the main OpenClaw system from the outside. It detects outages, watches for changes, and sends Telegram alerts independently — it works even when the Mac Mini is completely down.

**Repo**: [clawpi-scout](https://github.com/your-username/clawpi-scout)

## Why a separate device?

The Mac Mini can't tell you it's down. A scout on a separate device can:

- Detect gateway outages and alert via Telegram
- Monitor from outside the system boundary
- Survive Mac Mini reboots, crashes, and network issues
- Run cheap, always-on, low-power (Raspberry Pi ~5W)

## Network topology

```
Mac Mini (<mac-mini-hostname>)
├── OpenClaw Gateway (ws://127.0.0.1:18789)
├── Tailscale Serve → https://<hostname>.<tailnet>.ts.net
└── Tailscale IP: <mac-mini-tailscale-ip>

            │ Tailscale (encrypted, no port forwarding)

Raspberry Pi (<pi-hostname>)
├── clawpi-scout daemon (systemd)
├── Tailscale IP: <pi-tailscale-ip>
└── @your_scout_bot (Telegram)
```

The gateway only listens on `127.0.0.1`. Tailscale Serve exposes it securely to the tailnet. Only devices on the tailnet can reach it.

## Capabilities

| Feature | Interval | Description |
|---------|----------|-------------|
| Health monitor | 60s | Pings gateway, alerts after 3 consecutive failures |
| Web watchers | 5 min | Monitors URLs/APIs, notifies on change |
| Morning briefing | Daily 8AM | System summary — gateway, CPU, disk, memory |
| Telegram alerts | On event | Independent from OpenClaw, works when system is down |

## Alert flow

```
Gateway down for 3 min
  → clawpi-scout detects failure
    → Telegram Bot API (direct, not via OpenClaw)
      → Scout bot sends alert to your chat

Gateway recovers
  → clawpi-scout detects recovery
    → Telegram recovery message
```

## Setup

Full setup guide: [clawpi-scout/docs/SETUP.md](https://github.com/your-username/clawpi-scout/blob/main/docs/SETUP.md)

Quick version:
1. Flash Raspberry Pi OS to SD card
2. Enable SSH, connect to network
3. Install Tailscale, join same tailnet as Mac Mini
4. Enable Tailscale Serve on Mac Mini (`tailscale serve --bg 18789`)
5. Clone and install clawpi-scout
6. Configure `scout.yaml` with Telegram bot token + chat ID
7. Start systemd service

## Integration with agents

The scout currently operates independently. Future integration paths:

- **Scout → BigDawg**: Scout sends structured reports to BigDawg via the OpenClaw gateway WebSocket
- **Scout → Brain**: Overnight data collection for Brain to analyze in morning sessions
- **Scout as data source**: Pre-filtered web data staged on the Pi for agent consumption
