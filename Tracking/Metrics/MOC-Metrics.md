---
aliases: [Metrics Dashboard, Skill Velocity, System Health]
tags: [tracking, metrics, moc, template]
---

# 📈 MOC — Metrics Dashboard

> **Cluster**: Tracking • **Sub-Cluster**: Metrics
> **Purpose**: Skill velocity, system health, performance indicators
> **Auto-Generated**: Via Dataview queries
> **Links**: `[[Tracking/MOC-Tracking]]` • `[[Ops/Skills-Registry/Catalog]]` • `[[Raw/Skills-Registry/]]`

---

## 📊 Skill Velocity

```dataview
TABLE
  file.name as Skill,
  Category,
  file.ctime as Created,
  file.mtime as Last_Updated
FROM "Ops/Skills-Registry"
WHERE file.name != "Catalog" AND file.name != "MOC-Skills-Registry"
SORT file.mtime DESC
LIMIT 20
```

---

## 🏥 System Health

```dataview
TABLE
  file.name as Component,
  Status,
  Last_Check
FROM "Raw"
WHERE contains(file.folder, "Hermes-Config") OR contains(file.folder, "Cron-Jobs") OR contains(file.folder, "Daily-Ops")
SORT file.mtime DESC
```

---

## 📈 Custom Metrics (Add your own)

| Metric | Query | Target |
|--------|-------|--------|
| Daily active skills | `FROM "Ops/Skills-Registry" WHERE file.mtime >= date(today)` | > 5 |
| Weekly session count | `FROM "Chats" WHERE file.ctime >= date(today) - dur(7 days)` | > 10 |
| Graph connectivity | Custom script | > 95% |
| Orphan rate | `FROM "" WHERE length(file.inlinks) = 0 AND length(file.outlinks) = 0` | < 5% |

---

*Metrics — What gets measured gets improved* 📈