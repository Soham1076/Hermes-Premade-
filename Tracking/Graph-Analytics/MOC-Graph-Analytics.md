---
aliases: [Graph Analytics, Vault Topology, Knowledge Graph]
tags: [graph, analytics, topology, moc]
---

# 🕸️ MOC — Graph Analytics

> **Cluster**: Tracking • **Sub-Cluster**: Graph Analytics
> **Purpose**: Vault topology analysis — orphans, hubs, clusters, broken links
> **Cadence**: Monthly (1st of month) via cron
> **Links**: `[[Tracking/MOC-Tracking]]` • `[[Ops/Automation/MOC-Automation]]`

---

## 📁 Structure

```
Tracking/Graph-Analytics/
├── MOC-Graph-Analytics.md     ← This file
├── Graph-View-Config.md       # Force Graph settings for spaced layout
├── Orphans.md                 # Notes with 0 links
├── Hubs.md                    # Notes with most links (centrality)
├── Clusters.md                # Community detection, modularity
├── Broken-Links.md            # [[Wikilinks]] that don't resolve
└── Graph-Queries.md           # Saved graph view queries
```

---

## 🔍 Orphan Notes (Zero Links)

```dataview
LIST
WHERE length(file.inlinks) = 0 AND length(file.outlinks) = 0
AND !contains(file.folder, "Templates")
AND !contains(file.folder, ".obsidian")
SORT file.path
```

**Target**: < 5% of total notes

---

## 🎯 Hub Notes (High Centrality)

```dataview
TABLE
  length(file.inlinks) + length(file.outlinks) as Total_Links,
  length(file.inlinks) as Inlinks,
  length(file.outlinks) as Outlinks,
  file.link as Note
WHERE file != this.file
SORT Total_Links DESC
LIMIT 20
```

**Hubs Should Be**: MOCs, Index pages, Core concepts

---

## 🌐 Cluster Detection (Manual Analysis)

| Cluster | Core MOC | Member Count | Density |
|---------|----------|--------------|---------|
| Core Intelligence | `Core/MOC-Core` | TBD | TBD |
| Operations | `Ops/MOC-Ops` | TBD | TBD |
| AI/ML Domain | `Domains/AI-ML/MOC-AI-ML` | TBD | TBD |
| Software Domain | `Domains/Software/MOC-Software` | TBD | TBD |
| Creative Domain | `Domains/Creative/MOC-Creative` | TBD | TBD |
| Projects | `Projects/MOC-Projects` | TBD | TBD |
| Tracking | `Tracking/MOC-Tracking` | TBD | TBD |

---

## 🔗 Broken Links

```dataview
LIST
FROM ""
WHERE contains(file.outlinks, "[[") AND !file.outlinks
```

*Run in Dataview: Check for `[[Wikilink]]` that don't resolve to existing files*

---

## 📊 Graph View Presets (Save in Obsidian)

| Preset Name | Groups | Filters | Use Case |
|-------------|--------|---------|----------|
| **Cluster View** | By folder (Core, Ops, Domains, Projects, Tracking) | All | Daily navigation |
| **Project View** | By project state | `Projects/**` | Sprint planning |
| **Orphan Hunt** | By link count | `links:0` | Cleanup |
| **Hub Map** | By centrality | `links:>10` | Architecture review |
| **Spaced View** | Three Drawers + Skills | All | **Clean spaced layout** |

---

## 📈 Monthly Graph Health Report

| Metric | Current | Target | Trend |
|--------|---------|--------|-------|
| Total Notes | TBD | Growing | — |
| Orphan Rate | TBD | <5% | — |
| Avg Links/Note | TBD | >3 | — |
| Cluster Count | TBD | 6-10 | — |
| Broken Links | TBD | 0 | — |
| Largest Component | TBD | 100% | — |

---

*Graph Analytics — Seeing the shape of your knowledge, finding the gaps* 🕸️