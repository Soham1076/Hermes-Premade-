# 🤖 MOC — Skills Registry

> **Cluster**: Operations • **Sub-Cluster**: Skills Registry
> **Purpose**: Skill lifecycle management — catalog, templates, incubation, deprecation
> **Links**: `[[Ops/MOC-Ops]]` • `[[Domains/Software/MOC-Software]]`

---

## 📁 Contents

| File | Purpose | Tags |
|------|---------|------|
| `[[Ops/Skills-Registry/Catalog]]` | Complete skill index (85 skills, 14 categories) | `#hub #catalog` |
| `[[Ops/Skills-Registry/Skill-Templates]]` | SKILL.md template, frontmatter standards | `#template` |
| `[[Ops/Skills-Registry/Incubation-Pipeline]]` | New skill ideas → development → graduation | `#process` |
| `[[Ops/Skills-Registry/Deprecation-Log]]` | Archived/merged skills with absorption mapping | `#archive` |

---

## 📊 Skill Catalog Stats

```dataview
TABLE
  length(rows) as Count,
  rows.file.link as Skills
FROM "Ops/Skills-Registry"
WHERE file.name != "Catalog" AND file.name != "MOC-Skills-Registry"
GROUP BY Category
SORT Count DESC
```

---

## 🔄 Skill Lifecycle

```
IDEA → INCUBATING → ACTIVE → DEPRECATED
  ↓        ↓         ↓         ↓
Log in    Develop   Document  Merge/Archive
Catalog   + Test    + Sync    + Update refs
```

**Incubation Criteria**:
- [ ] Solves real recurring problem (≥3 uses)
- [ ] Has clear trigger conditions
- [ ] Includes verification steps
- [ ] Tested in 2+ sessions

---

## 🆕 Current Incubation Pipeline

| Skill Idea                     | Trigger                 | Status    | Owner  |
| ------------------------------ | ----------------------- | --------- | ------ |
| JEE Mains Study Tracker        | Practice score tracking | 🟡 Design | Cosmos |
| Neko-chan Message Generator    | Cron heartbeat messages | 🟡 Design | Cosmos |
| Netflix Watch History Logger   | Daily note enrichment   | 🔴 Idea   | Cosmos |
| Discord Gateway Health Monitor | Silent bot alerting     | 🔴 Idea   | Cosmos |
| Obsidian Graph Analyzer        | Orphan/hub detection    | 🔴 Idea   | Cosmos |
|                                |                         |           |        |

---

## 🔗 Graph Connections

```dataview
LIST
FROM "Ops/Skills-Registry"
WHERE file != this.file
SORT file.name
```

---

*Skills Registry — Where capabilities are born, tested, and immortalized* 🧬