---
aliases: [Chat Sessions MOC, Chat Hub, Session Index]
tags: [chats, moc, hub, sessions]
---

# 💬 MOC — Chat Sessions

> **Cluster**: Chats • **Purpose**: Session logs, decisions, context per conversation
> **Links**: `[[Home]]` • `[[Core/MOC-Core]]` • `[[Raw/Session-Context/MOC-Session-Context]]` • `[[Wiki/Decisions/MOC-Decisions]]`

---

## 📋 Session Index

**Main Index:** `[[Chats/Chat-Sessions-Index]]`

---

## 📁 Session Structure

```
Chats/
├── Chat-Sessions-Index.md      # Master index of all sessions
├── MOC-Chats.md                # This file
├── Templates/
│   └── MOC-Templates.md        # Session templates
└── YYYY-MM-DD/                 # One folder per day
    └── Session-Topic.md        # Individual session notes
```

---

## 🔄 Session Lifecycle

| Stage | Action | Output |
|-------|--------|--------|
| **Start** | Use template from `Templates/MOC-Templates.md` | New session note |
| **During** | Link decisions → `[[Wiki/Decisions/]]`, blockers → `[[Raw/Session-Context/Open-Loops]]` | Cross-links |
| **End** | Summarize in `Chat-Sessions-Index.md` | Updated index |
| **Archive** | Move to dated folder (auto-organized by date) | Archived |

---

## 📋 Templates

| Template | Use For | Link |
|----------|---------|------|
| Daily Session | General chat sessions | `[[Chats/Templates/MOC-Templates#daily-session-template]]` |
| Project Sprint | Project-focused sessions | `[[Chats/Templates/MOC-Templates#project-sprint-template]]` |

---

## 🔗 Cross-Links

- **Upstream**: `[[Home]]` (dashboard)
- **Parallel**: `[[Raw/Session-Context/MOC-Session-Context]]` (live session state)
- **Downstream**: `[[Wiki/Decisions/MOC-Decisions]]` (decisions from chats), `[[Tracking/Daily/]]` (daily notes reference chats)
- **Skills**: `[[Ops/Skills-Registry/Catalog]]` (skills used per session)

---

*Chat Sessions — Every conversation captured, linked, and learnable* 💬