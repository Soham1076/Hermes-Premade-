---
aliases: [Chat Templates, Session Templates]
tags: [chats, templates, moc]
---

# 📋 MOC — Chat Templates

> **Cluster**: Chats • **Purpose**: Reusable templates for session summaries
> **Links**: `[[Chats/MOC-Chats]]` • `[[Chats/Chat-Sessions-Index]]`

---

## 📋 Templates

### Daily Session Template
```markdown
---
date: YYYY-MM-DD
type: session
participants: [User, Assistant]
tags: [chat, session, YYYY-MM-DD]
---

# Session - YYYY-MM-DD

## 🎯 Goals
- [ ] Goal 1
- [ ] Goal 2

## 💬 Key Discussions
### Topic 1
- Discussion points
- Decisions made

### Topic 2
- Discussion points
- Decisions made

## ✅ Outcomes
- Completed items
- Deliverables

## 🔄 Open Loops
- Items to continue next session
- Blockers
```

### Project Sprint Template
```markdown
---
date: YYYY-MM-DD
type: sprint
project: Project-Name
tags: [chat, sprint, project]
---

# Sprint Session - Project Name

## 🎯 Sprint Goal
- Focus for this session

## 📋 Tasks
- [ ] Task 1
- [ ] Task 2

## 🚧 Blockers
- Blocker 1
- Blocker 2

## 📝 Decisions (ADRs)
- Decision 1 → `[[Wiki/Decisions/...]]`
- Decision 2 → `[[Wiki/Decisions/...]]`

## 🔄 Retro
- What worked
- What didn't
- Improvements
```