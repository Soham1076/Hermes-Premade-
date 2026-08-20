---
aliases: [Projects Hub, Sprint Hub, Project Tracker]
tags: [projects, moc, hub, sprint]
---

# 🎯 MOC — Projects Cluster

> **Cluster**: Projects • **Type**: Map of Content (MOC)
> **Purpose**: Active sprints, incubating ideas, archived work
> **Links**: `[[Home]]` • `[[Domains/MOC-Domains]]` • `[[Tracking/MOC-Tracking]]`

---

## 📁 Project States

```
Projects/
├── Active/          # Currently in progress
├── Incubating/      # Ideas being explored
└── Archive/         # Completed or paused
```

---

## 📋 Sub-MOCs

| State | MOC | Description |
|-------|-----|-------------|
| **Active** | `[[Projects/Active/index]]` | Current sprints, goals, blockers |
| **Incubating** | `[[Projects/Incubating/index]]` | Proposals, research, prototypes |
| **Archive** | `[[Projects/Archive/index]]` | Completed projects, lessons learned |

---

## 🔄 Project Lifecycle

```
IDEA → INCUBATING → ACTIVE → COMPLETED
  ↓        ↓         ↓         ↓
Propose  Research  Execute   Document
Validate + Design  + Track   + Archive
```

**Active Criteria:**
- [ ] Clear goal & success metrics
- [ ] Defined scope & timeline
- [ ] Assigned owner
- [ ] Weekly review scheduled

---

## 📊 Quick Status

```dataview
TABLE
  file.name as Project,
  State,
  Priority,
  Progress
FROM "Projects"
WHERE file != this.file
SORT State DESC, Priority DESC
```

---

## 🔗 Cross-Links

- **Upstream**: `[[Home]]`, `[[Domains/MOC-Domains]]`
- **Parallel**: `[[Tracking/MOC-Tracking]]` (metrics), `[[Ops/MOC-Ops]]` (automation)
- **Downstream**: `[[Wiki/Decisions/]]` (ADRs), `[[Wiki/Patterns/]]` (patterns)

---

*Projects — Where goals become reality* 🎯