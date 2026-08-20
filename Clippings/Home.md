---
aliases: [Clippings Inbox, External Knowledge, Knowledge Capture]
tags: [clippings, inbox, moc, hub, external]
---

# 📎 MOC — Clippings (External Knowledge Inbox)

> **Drawer**: 2 of 3 | **Purpose**: Captured external knowledge awaiting synthesis
> **Links**: `[[Home]]` • `[[Raw/Home]]` • `[[Wiki/Home]]`

---

## 🗂️ Sub-Clusters

| MOC | Description | Status | Capture Method |
|-----|-------------|--------|----------------|
| `[[Clippings/YouTube/MOC-YouTube]]` | Video transcripts + summaries | 🟢 Active | `youtube-content` skill |
| `[[Clippings/Articles/MOC-Articles]]` | Blog posts, docs, tutorials | 🟡 Manual | Obsidian Clipper |
| `[[Clippings/Papers/MOC-Papers]]` | arXiv, research papers | 🟡 Manual | `arxiv` skill |
| `[[Clippings/Discord-Telegram/MOC-Gateway-Messages]]` | Notable gateway messages | 🟡 Manual | Copy from logs |
| `[[Clippings/Tools/MOC-Tools]]` | Tool docs, CLI references | 🟡 Manual | As discovered |
| `[[Clippings/Unprocessed/MOC-Unprocessed]]` | Raw dumps awaiting ingestion | 🟢 Active | Auto-dump |

---

## ⚡ Quick Status

```dataview
TABLE
  file.name as Clipping,
  file.ctime as Captured,
  file.tags as Tags
FROM "Clippings"
WHERE file.name != "Home" AND file.name != "MOC-Clippings"
SORT file.ctime DESC
```

---

## 🔄 Clippings → Wiki Synthesis

| Clipping Type | Wiki Target | Synthesis Trigger |
|---------------|-------------|-------------------|
| YouTube (architectures) | `[[Wiki/Patterns/]]` | 3+ videos on same topic |
| Articles (best practices) | `[[Wiki/Operations/]]` | Verified in practice |
| Papers (research) | `[[Wiki/Patterns/]]` | Reproducible results |
| Gateway messages | `[[Wiki/Troubleshooting/]]` | Recurring issue |
| Tool docs | `[[Wiki/Integrations/]]` | Used in production |

---

## 🔗 Cross-Links

- **Upstream**: `[[Home]]` (dashboard)
- **Parallel**: `[[Raw/Home]]`, `[[Wiki/Home]]`
- **Downstream**: `[[Wiki/Patterns/]]`, `[[Wiki/Decisions/]]`

---

*Clippings — The knowledge inbox that feeds your intelligence* 📎