---
aliases: [Raw Operations, Live Ops, System State]
tags: [raw, ops, moc, hub, live]
---

# 📥 MOC — Raw (Live Operations)

> **Drawer**: 1 of 3 | **Purpose**: Live system state — what's actually running right now
> **Links**: `[[Home]]` • `[[Clippings/Home]]` • `[[Wiki/Home]]` • `[[Ops/MOC-Ops]]`

---

## 🗂️ Sub-Clusters

| MOC | Description | Status | Auto-Synced |
|-----|-------------|--------|-------------|
| `[[Raw/Hermes-Config/MOC-Hermes-Config]]` | Model, providers, TTS, vision, tools | 🟢 Active | Hourly cron |
| `[[Raw/Cron-Jobs/MOC-Cron-Jobs]]` | Active scheduled jobs, status, logs | 🟢 Active | Hourly cron |
| `[[Raw/Skills-Registry/MOC-Skills-Registry]]` | Installed skills, usage, versions | 🟢 Active | Hourly cron |
| `[[Raw/Daily-Ops/MOC-Daily-Ops]]` | Gateway health, API limits, error patterns | 🟢 Active | Hourly cron |
| `[[Raw/Session-Context/MOC-Session-Context]]` | Current tasks, open loops, context | 🟡 Manual | Per session |

---

## ⚡ Quick Status (Live)

```dataview
TABLE
  file.name as Component,
  Status,
  Last_Update
FROM "Raw"
WHERE file.name != "Home" AND file.name != "MOC-Raw"
SORT file.mtime DESC
```

---

## 🔄 Raw → Clippings → Wiki Flow

| Raw Source | Clippings Target | Wiki Synthesis |
|------------|------------------|----------------|
| `[[Raw/Hermes-Config/]]` | `[[Clippings/Tools/]]` (config refs) | `[[Wiki/Integrations/]]` |
| `[[Raw/Cron-Jobs/]]` | `[[Clippings/Unprocessed/]]` (logs) | `[[Wiki/Operations/]]` |
| `[[Raw/Skills-Registry/]]` | `[[Clippings/Tools/]]` (skill docs) | `[[Wiki/Patterns/]]` |
| `[[Raw/Daily-Ops/]]` | `[[Clippings/Discord-Telegram/]]` (alerts) | `[[Wiki/Troubleshooting/]]` |
| `[[Raw/Session-Context/]]` | `[[Tracking/Daily/]]` (daily notes) | `[[Wiki/Decisions/]]` |

---

## 🔗 Cross-Links

- **Upstream**: `[[Home]]` (dashboard)
- **Parallel**: `[[Clippings/Home]]`, `[[Wiki/Home]]`
- **Downstream**: `[[Ops/MOC-Ops]]`, `[[Tracking/MOC-Tracking]]`

---

*Raw — The live pulse of your agentic system* 📥