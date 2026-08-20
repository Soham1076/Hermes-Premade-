# 📝 Skill: netflix-manager

> **Category**: desktop • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
Netflix workflow - open, switch profile, cleanup.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

# Netflix Manager Skill

Complete Netflix workflow: open Edge → go to Netflix → switch profile → (watch) → close tab → minimize all windows.

## Usage

```bash
# Full workflow (open + switch profile)
hermes skill run netflix-manager --profile [PROFILE_NAME]

# Just cleanup
hermes skill run netflix-manager --cleanup

# Open and switch to different profile
hermes skill run netflix-manager --profile kids
```

## Commands

| Flag | Action |
|------|--------|
| `--profile NAME` | Open Netflix and switch to profile |
| `--cleanup` | Close Netflix tab and minimize all windows |
| `--help` | Show usage |

## Sub-skills used

- `netflix-profile-switcher` - Opens Edge, navigates to Netflix, switches profile
- `netflix-cleanup` - Closes Netflix tab, minimizes all windows

## Requirements

- Microsoft Edge installed
- `computer-use` skill loaded
- Netflix account with profiles set up

## Notes

- Runs entirely in **background** — no focus stealing
- First run ~10-15 sec (Edge launch + Netflix load)
- Subsequent runs faster if Edge stays open
- Cleanup is instant (~2-3 seconds)

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`[HERMES_HOME]\skills\desktop\netflix-manager\`

---

*Source: `skill_view('netflix-manager')`*
*Updated: 2026-08-10*
tags: [skill, desktop, #skill/desktop]
parent: "[[Ops/Skills-Registry/desktop/MOC-Desktop]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
