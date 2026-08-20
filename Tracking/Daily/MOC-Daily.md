---
aliases: [Daily Notes Index, Daily Standups, Daily Log]
tags: [daily, tracking, index, moc]
---

# 📅 MOC — Daily Notes Index

> **Cluster**: Tracking • **Sub-Cluster**: Daily Notes
> **Purpose**: Daily standups, session summaries, config snapshots
> **Links**: `[[Tracking/MOC-Tracking]]` • `[[Home]]` • `[[Raw/Daily-Ops/MOC-Daily-Ops]]`

---

## 📋 Daily Notes

```dataview
TABLE
  file.name as Date,
  file.ctime as Created,
  file.mtime as Modified
FROM "Tracking/Daily"
WHERE file.name != "MOC-Daily"
SORT file.name DESC
```

---

## 📅 Index

- `[[Tracking/Daily/YYYY-MM-DD]]` — Daily note template

---

## 📝 Daily Note Template

```markdown
---
date: YYYY-MM-DD
type: daily
tags: [daily, YYYY-MM-DD]
---

# YYYY-MM-DD

## 🎯 Goals
- [ ] Goal 1
- [ ] Goal 2

## 💬 Sessions
- `[[Chats/YYYY-MM-DD/Session-Topic]]`

## 🔧 Config Changes
- [ ] Change 1
- [ ] Change 2

## 📊 Metrics
- Skills used: [N]
- Files changed: [N]
- Cron runs: [N]

## 🔄 Open Loops
- [ ] Item to continue

## 📝 Notes
Free-form notes...
```

---

## 🔄 Auto-Generated

> **Cron job** `obsidian-vault-sync` runs hourly → updates this index

*Daily Notes — The heartbeat of the system* 📅