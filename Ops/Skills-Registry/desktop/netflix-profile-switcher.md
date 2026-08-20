# 📝 Skill: netflix-profile-switcher

> **Category**: desktop • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
Open Netflix on Edge and switch profiles in background.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

# Netflix Profile Switcher Skill

Quickly open Netflix on Microsoft Edge and switch to your desired profile using background desktop automation.

## Usage

Run the skill with a profile name parameter:

```bash
# From terminal
hermes skill run netflix-profile-switcher --profile sohammm
```

Or invoke from an agent — just say "Open Netflix on Edge and go to my profile sohammm"

## Steps (internal)

1. **Launch/Focus Edge** - Uses `start msedge https://www.netflix.com` or focuses existing window
2. **Wait for load** - Waits 5-8 seconds for Netflix to fully load
3. **Find profile button** - Locates the "Personal Profile" button in Edge toolbar (top-right)
4. **Click profile dropdown** - Opens the profile selection menu
4. **Select target profile** - Finds and clicks the specified profile name
5. **Verify** - Confirms profile is active

## Requirements

- Microsoft Edge installed at default location: `C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe`
- `computer-use` skill loaded (dependency)
- Netflix account with profiles already set up

## Notes

- Runs in **background** — doesn't steal your cursor or focus
- Works even if you're working in another window
- If profile not found, will list available profiles
- First run may take 10-15 seconds; subsequent runs faster if Edge stays open

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`C:\Users\soham\AppData\Local\hermes\profiles\cosmos\skills\desktop\netflix-profile-switcher\`

---

*Source: `skill_view('netflix-profile-switcher')`*
*Updated: 2026-08-10*
tags: [skill, desktop, #skill/desktop]
parent: "[[Ops/Skills-Registry/desktop/MOC-Desktop]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
