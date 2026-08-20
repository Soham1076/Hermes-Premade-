---
aliases: [Templates Hub, Template Library]
tags: [templates, moc, hub]
---

# 📄 MOC — Templates Library

> **Cluster**: Templates • **Purpose**: Reusable templates for consistent note creation
> **Links**: `[[Home]]` • `[[Chats/Templates/MOC-Templates]]`

---

## 📋 Template Categories

| Template | Purpose | Location |
|----------|---------|----------|
| **Chat Templates** | Session summaries, sprint notes | `[[Chats/Templates/MOC-Templates]]` |
| **Project Templates** | Sprint plans, weekly goals, blockers | `Projects/Active/*/` |
| **Daily Note** | Daily tracking template | `Tracking/Daily/MOC-Daily.md` |
| **Weekly Review** | Sprint retrospective template | `Tracking/Weekly/MOC-Weekly.md` |
| **Monthly Review** | Monthly planning template | `Tracking/Monthly/MOC-Monthly.md` |
| **ADR Template** | Architecture Decision Records | `Wiki/Decisions/MOC-Decisions.md` |
| **Pattern Template** | Discovered patterns | `Wiki/Patterns/MOC-Patterns.md` |
| **Fix Template** | Troubleshooting entries | `Wiki/Troubleshooting/MOC-Troubleshooting.md` |
| **Chapter Overview** | Domain chapter summaries | `Templates/Chapter-Overview.md` |

---

## 📝 Chapter Overview Template

**File:** `Templates/Chapter-Overview.md`

```markdown
---
domain: ""
subject: ""
branch: ""
chapter: ""
unit: ""
topic: ""
type: "chapter-overview"
status: "draft"
difficulty: "medium"
weight: "medium"
last-reviewed: ""
next-review: ""
source: ""
tags: [...]
---

# {{chapter}} - {{subject}} ({{domain}})

## 📚 Overview
...

## 🔑 Key Concepts
...

## 📝 Formulas & Theorems
...

## 🎯 Practice Strategy
...

## 🔗 Links
- [[../MOC-{{subject}}]]
- [[../../MOC-Domains]]
```

---

## 🔗 Cross-Links

- **Upstream**: `[[Home]]` (dashboard)
- **Used by**: All MOCs reference their template locations
- **Chats**: `[[Chats/Templates/MOC-Templates]]` for session templates

---

*Templates — Consistency at scale* 📄