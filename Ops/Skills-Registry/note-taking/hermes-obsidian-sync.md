# 📝 Skill: hermes-obsidian-sync

> **Category**: note-taking • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
Sync Hermes state to Obsidian vault via cron job.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

## When to Use

- User wants persistent, offline-accessible knowledge base of Hermes capabilities
- User wants cross-device sync of skills, memories, config via OneDrive/iCloud/Git
- User wants visual knowledge graph of Hermes skills and relationships
- Automating periodic backup of Hermes state to filesystem

# Hermes Obsidian Sync Skill

Maintain a living Obsidian vault mirror of Hermes Agent's state: skills catalog, user/system memories, configuration snapshots, and chat session links. Runs on a schedule via cron.

## Purpose

- **Offline access** to Hermes capabilities without the agent running
- **Cross-device sync** via OneDrive/iCloud/Git
- **Visual knowledge graph** using Obsidian's wikilinks and graph view
- **Audit trail** of configuration changes over time

## Vault Structure (Graph-Friendly)

```\n/path/to/vault/Cosmos-KB/\n├── Home.md                              # 🏠 Graph entry point (navigation table)\n├── Memories/\n│   └── Takopi-Profile.md                # 👤 User profile, preferences, boundaries\n├── Skills/\n│   ├── Catalog.md                       # 📋 All skills by category (wikilinks)\n│   └── <Category>/<skill-name>.md       # 📄 Individual skill pages (94 total)\n├── Config/\n│   └── Active-Configuration.md          # ⚙️ Model, TTS, Vision, Gateway, Cron\n├── Chats/\n│   ├── Chat-Sessions-Index.md           # 📋 Session index + template\n│   └── YYYY-MM-DD/\n│       └── Session-Topic.md             # 💬 Per-chat: summary, skills, decisions, artifacts\n└── Daily/\n    ├── Index.md                         # 📅 Daily notes index\n    └── YYYY-MM-DD.md                    # 📝 Daily summary (sessions, config, files, memories)\n```

**Naming Convention:** Descriptive filenames (not `Index.md`) for clean Obsidian graph view.
- Each chat = own file in `Chats/YYYY-MM-DD/Topic.md`
- Each skill = own file in `Skills/<Category>/<skill-name>.md`
- Daily notes = `Daily/YYYY-MM-DD.md`

## Setup

1. **Ensure Obsidian vault exists** at known path (default: `~/Documents/Obsidian Vault` or `$OBSIDIAN_VAULT_PATH`)
2. **Create Hermes folder** in vault (auto-created on first run)
3. **Configure cron job** for periodic sync:

```bash
hermes cronjob create \
  --name hermes-obsidian-sync \
  --schedule "every 1h" \
  --skills "obsidian,hermes-agent" \
  --prompt "Update the Obsidian vault at <VAULT_PATH>/Hermes with current Hermes state:
  1. Skills/Index.md - Refresh from skills_list() (all skills with categories)
  2. Memories/Index.md - Refresh from memory(target='user') and memory(target='memory')
  3. Config/Index.md - Refresh current model, provider, TTS, vision, gateway status, cron jobs
  4. Chats/Index.md - Add any new session links from recent conversations
  Use write_file to update each file. Keep the same structure and wikilinks.
  Only update if content actually changed (compare with existing)."
```

## Sync Logic (per file)

| File | Source | Update Trigger |
|------|--------|----------------|
| `Skills/Index.md` | `skills_list()` | New skill installed, category changed |
| `Memories/Index.md` | `memory(target="user")`, `memory(target="memory")` | User preference added, system fact learned |
| `Config/Index.md` | `hermes config`, `cronjob list`, gateway status | Model/provider changed, cron added/removed |
| `Chats/Index.md` | `session_search()` | New conversation session |

## Key Patterns

### Resolving Vault Path

```python
# Priority order
1. $OBSIDIAN_VAULT_PATH env var
2. ~/Documents/Obsidian Vault (Windows: /c/Users/<user>/OneDrive/Documents/Obsidian Vault)
3. ~/Obsidian
```

### Writing with Wikilinks

```markdown
- [[Skills/Category/skill-name|skill-name]] - Description
```

Use `write_file` (not `patch`) for full rewrites — simpler and atomic.

### Idempotent Updates

Compare new content with existing before writing. Only write if changed to avoid unnecessary file timestamps.

## Cron Job Management

| Action | Command |
|--------|---------|
| Create | `cronjob create --name hermes-obsidian-sync --schedule "every 1h" --skills "obsidian,hermes-agent" --prompt "..."` |
| Run now | `cronjob run --job_id <id>` |
| Pause | `cronjob pause --job_id <id>` |
| Resume | `cronjob resume --job_id <id>` |
| Remove | `cronjob remove --job_id <id>` |
| List | `cronjob list` |

## Current Cron Prompt (Updated)

```text
Update the Obsidian vault at /c/Users/soham/OneDrive/Documents/Obsidian Vault/Cosmos-KB with current Hermes state:

1. **Skills/Catalog.md** - Refresh from `skills_list()` (all 94 skills with categories)
2. **Skills/<Category>/<skill-name>.md** - Create/update individual skill pages for all 94 skills (use `skill_view()` for each)
3. **Memories/Takopi-Profile.md** - Refresh from `memory(target="user")` and `memory(target="memory")`
4. **Config/Active-Configuration.md** - Refresh current model, provider, TTS, vision, gateway status, cron jobs
5. **Chats/Chat-Sessions-Index.md** - Add any new session links from recent conversations
6. **Daily/YYYY-MM-DD.md** - Create/update today's daily note with summary
7. **Daily/Index.md** - Update daily notes index

Use `write_file` to update each file. Keep the same structure and wikilinks.
Only update if content actually changed (compare with existing).
```

**Job ID**: `8d759f9040fb` | **Schedule**: Every 60 min | **Delivery**: Local (silent)

## Reference Files

- `references/netflix-edge-automation.md` — Working `computer_use` patterns for Netflix on Edge (foreground delivery for address bar, timing, cleanup)
- `references/graph-friendly-vault.md` — Graph-friendly vault design principles, templates, sync logic, file counts

## Safety

- Never write secrets (API keys, passwords) to vault
- Vault is user-owned — respect privacy boundaries
- Cron runs with `deliver="local"` by default (no gateway spam)

## Related Skills

- `obsidian` — Core vault operations (read, write, search)
- `hermes-agent` — Hermes config, models, providers, tools
- `cron-job-management` — Schedule, monitor, debug cron jobs
- `computer-use` — Desktop automation patterns (see reference file)

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`C:\Users\soham\AppData\Local\hermes\profiles\cosmos\skills\note-taking\hermes-obsidian-sync\`

---

*Source: `skill_view('hermes-obsidian-sync')`*
*Updated: 2026-08-10*
tags: [skill, note-taking, #skill/note-taking]
parent: "[[Ops/Skills-Registry/note-taking/MOC-Notetaking]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
