---
aliases: [Wiki, Knowledge Base, Synthesized Intelligence, Intelligence Layer]
tags: [wiki, knowledge, synthesized, moc, hub]
---

# 📚 MOC — Wiki (Synthesized Intelligence)

> **Drawer**: 3 of 3 | **Purpose**: The living intelligence layer — reconciled, cross-referenced, actionable knowledge
> **Source**: `[[Raw/Home]]` (ops) + `[[Clippings/Home]]` (external) → synthesized by Hermes
> **Query Interface**: Ask *"Does my current X actually hold up?"* → Wiki answers
> **Links**: `[[Home]]` • `[[Raw/Home]]` • `[[Clippings/Home]]`

---

## 🗂️ Sub-Clusters

| MOC | Description | Entries | Confidence |
|-----|-------------|---------|------------|
| `[[Wiki/Patterns/MOC-Patterns]]` | Reusable patterns discovered | 0 | 🟡 Planned |
| `[[Wiki/Decisions/MOC-Decisions]]` | Architecture Decision Records (ADRs) | 0 | 🟡 Planned |
| `[[Wiki/Troubleshooting/MOC-Troubleshooting]]` | Verified solutions to recurring issues | 0 | 🟡 Planned |
| `[[Wiki/Operations/MOC-Operations]]` | Synthesized ops knowledge | 0 | 🟡 Planned |
| `[[Wiki/Integrations/MOC-Integrations]]` | Platform-specific knowledge | 0 | 🟡 Planned |
| `[[Wiki/Meta/MOC-Meta]]` | About the wiki itself | 0 | 🟡 Planned |

---

## ⚡ Quick Access

```dataview
TABLE
  file.name as Entry,
  file.tags as Tags,
  file.mtime as Updated
FROM "Wiki"
WHERE file.name != "Home" AND file.name != "MOC-Wiki"
SORT file.mtime DESC
```

---

## 🔄 Synthesis Protocols

### Raw → Wiki (Operational Patterns)
1. **Detect**: Recurring config/error in `Raw/Daily-Ops`
2. **Capture**: Clip to `Clippings/Unprocessed`
3. **Synthesize**: Write pattern in `Wiki/Patterns`
4. **Decide**: Create ADR in `Wiki/Decisions` if architectural
5. **Fix**: Document solution in `Wiki/Troubleshooting`

### Clippings → Wiki (External Knowledge)
1. **Collect**: 3+ sources on same topic in `Clippings/`
2. **Compare**: Cross-reference in `Wiki/Patterns`
3. **Validate**: Test in your environment
4. **Document**: Write verified pattern/decision

---

## 🔗 Cross-Links

- **Upstream**: `[[Home]]`, `[[Raw/Home]]`, `[[Clippings/Home]]`
- **Parallel**: `[[Ops/MOC-Ops]]`, `[[Tracking/MOC-Tracking]]`
- **Downstream**: `[[Projects/MOC-Projects]]` (applied patterns)

---

*Wiki — Where noise becomes signal, signal becomes wisdom* 📚