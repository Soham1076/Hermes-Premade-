# 📝 Skill: netflix-cleanup

> **Category**: desktop • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
Close Netflix tab and minimize all windows in background.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

# Netflix Cleanup Skill

Close the Netflix tab in Microsoft Edge and minimize all open windows using background desktop automation.

## Usage

Run the skill:

```bash
# From terminal
hermes skill run netflix-cleanup
```

Or invoke from an agent — just say "Close Netflix and minimize all windows"

## Steps (internal)

1. **Focus Edge** - Find the Microsoft Edge window with Netflix
2. **Close Netflix tab** - Click the close button on the Netflix tab
3. **Show Desktop** - Click "Show Desktop" button in taskbar to minimize all windows

## Requirements

- Microsoft Edge with Netflix tab open
- `computer-use` skill loaded (dependency)

## Notes

- Runs in **background** — doesn't steal your cursor or focus
- Works even if you're working in another window
- Leaves Edge running (just closes the Netflix tab)
- Instant cleanup for when you're done watching

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`C:\Users\soham\AppData\Local\hermes\profiles\cosmos\skills\desktop\netflix-cleanup\`

---

*Source: `skill_view('netflix-cleanup')`*
*Updated: 2026-08-10*
tags: [skill, desktop, #skill/desktop]
parent: "[[Ops/Skills-Registry/desktop/MOC-Desktop]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
