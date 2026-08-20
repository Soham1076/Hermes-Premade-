---
aliases: [Tags Taxonomy, Tag Hierarchy, Tagging System]
tags: [tags, taxonomy, moc, system, template]
---

# 🏷️ MOC — Tags Taxonomy (Template)

> **Cluster**: Tags • **Purpose**: Hierarchical tag taxonomy for consistent classification
> **Links**: `[[Home]]` • `[[Tracking/MOC-Tracking]]`

---

## 🌳 Tag Hierarchy

```
#domain
  ├── #domain/ai-ml
  │   ├── #domain/ai-ml/llm
  │   ├── #domain/ai-ml/mlops
  │   └── #domain/ai-ml/research
  ├── #domain/software
  │   ├── #domain/software/architecture
  │   ├── #domain/software/languages
  │   └── #domain/software/tools
  └── #domain/creative
      ├── #domain/creative/visual
      ├── #domain/creative/audio
      ├── #domain/creative/writing
      └── #domain/creative/interactive

#type
  ├── #type/moc
  ├── #type/reference
  ├── #type/practice-log
  ├── #type/decision
  ├── #type/template
  ├── #type/sprint-plan
  ├── #type/retrospective
  ├── #type/blocker
  └── #type/meeting-notes

#status
  ├── #status/active
  ├── #status/incubating
  ├── #status/completed
  ├── #status/archived
  └── #status/weak

#priority
  ├── #priority/critical
  ├── #priority/high
  ├── #priority/medium
  └── #priority/low

#review
  ├── #review/daily
  ├── #review/weekly
  ├── #review/monthly
  └── #review/quarterly

#skill
  ├── #skill/hermes-agent
  ├── #skill/mlops
  ├── #skill/creative
  ├── #skill/github
  └── #skill/productivity

#source
  ├── #source/chat
  ├── #source/web
  └── #source/pdf
```

---

## 📝 Tagging Rules

1. **Always tag**: `#domain/*`, `#type/*`, `#status/*`
2. **Tag at creation**: Don't retroactively tag (use templates)
3. **One primary domain**: Each note belongs to ONE primary domain
4. **Multiple types OK**: A note can be `#type/reference` AND `#type/practice-log`
5. **Status is current**: Update status when work progresses

---

## 🔍 Tag Queries

```dataview
TABLE
  file.link as Note,
  file.tags as Tags
FROM ""
WHERE contains(file.tags, "skill/hermes-agent")
```

---

*Tags — The vocabulary that makes knowledge findable* 🏷️