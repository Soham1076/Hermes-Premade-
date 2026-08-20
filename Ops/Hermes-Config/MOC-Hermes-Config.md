---
aliases: [Hermes Config, Live Config, System Config]
tags: [ops, config, template, hermes]
---

# ⚙️ MOC — Hermes Configuration

> **Cluster**: Operations • **Sub-Cluster**: Hermes Config
> **Purpose**: Live configuration state — model, provider, TTS, gateway, cron
> **Auto-synced**: Hourly via `obsidian-vault-sync` cron
> **Links**: `[[Ops/MOC-Ops]]` • `[[Raw/Hermes-Config/MOC-Hermes-Config]]`

---

## 🤖 Model & Provider

| Setting | Value | Source |
|---------|-------|--------|
| **Model** | [e.g., nemotron-3.5-lightning-30b-a3b] | `hermes model` |
| **Provider** | [e.g., nvidia] | `hermes model` |
| **Base URL** | [e.g., https://integrate.api.nvidia.com/v1] | `hermes config get` |
| **Reasoning Effort** | [none / low / medium / high] | `hermes config get` |

---

## 🎙️ TTS & Voice

| Setting | Value | Source |
|---------|-------|--------|
| **Provider** | [piper / edge / openai / elevenlabs / gemini] | `hermes config get tts` |
| **Voice** | [voice name] | `hermes config get tts` |
| **Speed** | [0.25-4.0] | `hermes config get tts` |

---

## 👁️ Vision

| Setting | Value | Source |
|---------|-------|--------|
| **Provider** | [google-ai-studio / openai / local] | `hermes config get vision` |

---

## 🌐 Gateway

| Setting | Value | Source |
|---------|-------|--------|
| **Status** | [Running / Stopped / Error] | `hermes gateway status` |
| **Platforms** | [Discord / Telegram / Slack] | `hermes gateway status` |
| **PID** | [Process ID] | `hermes gateway status` |

---

## ⏰ Cron Jobs

| Job ID | Name | Schedule | Status | Skills |
|--------|------|----------|--------|--------|
| [ID] | obsidian-vault-sync | every 60m | [Active/Paused] | obsidian, hermes-agent |
| [ID] | github-vault-backup | 0 2 * * * | [Active/Paused] | (script) |

---

## 🔗 Cross-Links

- **Upstream**: `[[Ops/MOC-Ops]]`, `[[Home]]`
- **Parallel**: `[[Ops/Automation/MOC-Automation]]`
- **Downstream**: `[[Raw/Hermes-Config/]]` (live state), `[[Wiki/Integrations/]]` (platform configs)

---

*Hermes Config — The source of truth for your agent's runtime* ⚙️