---
aliases: [Workflow Preferences, Productivity Config]
tags: [core, preferences, template, workflow]
---

# ⚡ Workflow Preferences (Template)

> **Your preferred ways of working**
> **Links**: `[[Core/Preferences/MOC-Preferences]]` • `[[Core/Identity/User-Profile]]`

---

## 🛠️ Tool Preferences

| Category | Preferred Tool | Config Notes |
|----------|----------------|--------------|
| **Terminal** | [zsh/fish/bash] | [aliases, plugins] |
| **Editor** | [VS Code/Vim/Neovim] | [keybindings, extensions] |
| **Git** | [CLI / GUI / Both] | [workflow: rebase/merge] |
| **Note-taking** | [Obsidian / Notion / Logseq] | [vault structure] |
| **Task Management** | [Todoist / Things / Linear / Plain text] | [methodology] |

---

## ⚡ Automation Preferences

| Trigger | Preferred Action |
|---------|------------------|
| **New project** | [Template / Scaffold / Manual] |
| **Daily start** | [Review notes / Check calendar / Run script] |
| **Code changes** | [Auto-format / Lint / Test / Manual] |
| **Note capture** | [Quick capture / Structured / Voice] |
| **Sync/backup** | [Auto / Manual / Scheduled] |

---

## 🔧 CLI Aliases (Add to shell config)

```bash
# Navigation
alias vault="cd ~/path/to/vault"
alias kb="cd ~/path/to/vault"

# Git
alias gs="git status"
alias ga="git add -A"
alias gc="git commit -m"
alias gp="git push"

# Hermes
alias h="hermes"
alias hc="hermes chat -q"
alias hd="hermes desktop"

# Vault sync
alias kbsync="cd ~/path/to/vault && git add -A && git commit -m 'Sync $(date +%F)' && git push"
```

---

## 📅 Schedule Preferences

| Setting | Value |
|---------|-------|
| **Work hours** | [e.g., 9-17, 10-18] |
| **Break pattern** | [Pomodoro / 50/10 / Custom] |
| **Deep work blocks** | [Morning / Afternoon / Evening] |
| **Sync times** | [Morning / Evening / Manual] |
| **Review cadence** | [Daily / Weekly / Monthly] |

---

*Workflow Prefs — Your OS-level settings* ⚡