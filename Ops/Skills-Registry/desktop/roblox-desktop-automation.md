# 📝 Skill: roblox-desktop-automation

> **Category**: desktop • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
Join Roblox private servers via background automation.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

# Roblox Desktop Automation Skill

Automate Roblox private server joins and in-game actions via background desktop automation on Windows.

## Usage

```bash
# Join private server
hermes skill run roblox-desktop-automation --place-id 126509999165004 --server-code f915e10f9781f441b324524ce4afea14

# Just launch Roblox (background)
hermes skill run roblox-desktop-automation --launch-only

# Verify game loaded
hermes skill run roblox-desktop-automation --verify
```

## Workflow (Internal)

### Reliable Private Server Join Pattern

1. **Launch Roblox Player directly** (background)
   ```python
   terminal(command='start "" "C:\\Users\\[username]\\AppData\\Local\\Roblox\\Versions\\version-<hash>\\RobloxPlayerBeta.exe"', background=True)
   computer_use(action="wait", seconds=20)
   ```

2. **Verify Roblox window exists**
   ```python
   computer_use(action="list_windows")
   # Get PID and window_id for "RobloxPlayerBeta.exe" with title "Roblox"
   ```

3. **Join private server via protocol handler**
   ```python
   terminal(command='start "" "roblox://placeId=<PLACE_ID>&privateServerLinkCode=<SERVER_CODE>"')
   computer_use(action="wait", seconds=20)
   ```

4. **Verify in game** (vision capture)
   ```python
   computer_use(action="capture", mode="vision", pid=<PID>, window_id=<WINDOW_ID>)
   ```

## Key Learnings

| Aspect | Detail |
|---|---|
| **Launch method** | Direct exe launch + protocol handler is more reliable than opening Roblox website |
| **Protocol format** | `roblox://placeId=<ID>&privateServerLinkCode=<CODE>` |
| **Timing** | 15-20s after exe launch, 15-30s after protocol handler |
| **Window detection** | Use `list_windows` to get PID + window_id, then capture with both |
| **Background** | Works fully in background; no focus stealing |

## Common Pitfalls

1. **Opening Roblox website first** - Opens in browser, then launches Roblox, which is slower and less reliable
2. **Not waiting long enough** - Roblox takes 15-20s to fully initialize after exe launch
3. **Missing window_id** - Must capture with BOTH pid AND window_id on Windows
4. **Wrong protocol format** - Must include both `placeId` AND `privateServerLinkCode`

## Vision Verification

The local Ollama + moondream model can verify game load:
- "Garden" sign visible (for Grow a Garden)
- Characters with orange hats in garden area
- Game UI elements present

## Example: Grow a Garden Private Server

```bash
# Place ID: 126509999165004
# Share code: f915e10f9781f441b324524ce4afea14

start "" "[APPDATA]Roblox\Versions\version-145f189a6a974303\RobloxPlayerBeta.exe"
# wait 20s
start "" "roblox://placeId=126509999165004&privateServerLinkCode=f915e10f9781f441b324524ce4afea14"
# wait 20s
```

## Related Skills

- `computer-use` - Core desktop automation
- `netflix-manager` - Netflix profile switching (different app pattern)

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`[HERMES_HOME]\skills\desktop\roblox-desktop-automation\`

---

*Source: `skill_view('roblox-desktop-automation')`*
*Updated: 2026-08-10*
tags: [skill, desktop, #skill/desktop]
parent: "[[Ops/Skills-Registry/desktop/MOC-Desktop]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
