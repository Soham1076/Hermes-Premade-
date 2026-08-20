---
aliases: [Tracking Hub, Analytics Hub, Metrics Dashboard]
tags: [tracking, moc, hub, analytics, metrics]
---

# 📊 MOC — Tracking & Analytics Cluster

> **Cluster**: Tracking & Analytics • **Type**: Map of Content (MOC)
> **Purpose**: Daily pulse, weekly reviews, progress metrics, graph analytics
> **Links**: `[[Home]]` • `[[Projects/MOC-Projects]]` • `[[Domains/MOC-Domains]]` • `[[Raw/Home]]` • `[[Wiki/Home]]`

---

## 🗂️ Sub-Clusters

| MOC | Description | Auto-Generated |
|-----|-------------|----------------|
| `[[Tracking/Daily/MOC-Daily]]` | Daily notes, templates, standups | 🟢 Cron (hourly sync) |
| `[[Tracking/Weekly/MOC-Weekly]]` | Weekly reviews, sprint retros | 🟡 Manual + Cron |
| `[[Tracking/Monthly/MOC-Monthly]]` | Monthly planning, metrics rollup | 🟡 Manual |
| `[[Tracking/Metrics/MOC-Metrics]]` | Skill velocity, system health | 🟢 Dataview |
| `[[Tracking/Graph-Analytics/MOC-Graph-Analytics]]` | Orphans, hubs, clusters, broken links | 🟢 Monthly cron |

---

## 📅 Today's Daily Note

```dataview
LIST
FROM "Tracking/Daily"
WHERE file.day = date(today)
```

---

## 📈 This Week's Review

```dataview
LIST
FROM "Tracking/Weekly"
WHERE file.day >= date(today) - dur(7 days)
SORT file.day DESC
```

---

## 🔄 Raw → Tracking Flow

| Raw Source | Tracking Target |
|------------|-----------------|
| `[[Raw/Daily-Ops/Gateway-Health]]` | `[[Tracking/Metrics/MOC-Metrics]]` (system health) |
| `[[Raw/Cron-Jobs/]]` | `[[Tracking/Metrics/MOC-Metrics]]` (cron success rate) |
| `[[Raw/Skills-Registry/Skill-Usage-Log]]` | `[[Tracking/Metrics/MOC-Metrics]]` (skill velocity) |
| `[[Raw/Session-Context/]]` | `[[Tracking/Daily/]]` (daily notes) |

---

## 🔗 Cross-Links

- **Upstream**: `[[Home]]`, `[[Raw/Home]]`
- **Parallel**: `[[Projects/MOC-Projects]]`, `[[Domains/MOC-Domains]]`
- **Downstream**: `[[Wiki/Operations/]]` (procedures from metrics)

---

*Tracking — The feedback loop that turns effort into insight* 📊