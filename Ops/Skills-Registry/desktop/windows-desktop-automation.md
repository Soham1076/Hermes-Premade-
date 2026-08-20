# 📝 Skill: windows-desktop-automation

> **Category**: desktop • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
Windows patterns for computer-use automation and vision.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

# Windows Desktop Automation Patterns

This skill captures Windows-specific patterns, pitfalls, and workflows for background desktop automation using `computer_use` and vision providers.

### Vision Provider Setup (Windows)

### Cloud Providers - Quota Issues
**Google AI Studio (Gemini)** free tier is easily exhausted:
- `gemini-2.0-flash` - 429 RESOURCE_EXHAUSTED (quota: 0)
- `gemini-2.5-flash` - 404 no longer available to new users
- `gemini-2.0-flash-lite` - 500 multimodal not enabled
- `gemini-1.5-flash/pro` - 404 NOT_FOUND

**NVIDIA NIM** (nvidia/nemotron-3-ultra):
- `mode=\"vision\"` returns `ValueError: Received multimodal data but multimodal processing is not enabled. Use --enable-multimodal flag to enable multimodal processing.`

**Recommendation**: Default to local vision to avoid quota/issues.

### Local Vision (Recommended)
```yaml
# ~/.config/hermes/config.yaml
auxiliary:
  vision:
    provider: ollama
    model: moondream:latest  # 1.7 GB, fast, free
    # model: llava:latest    # 4.1 GB, better accuracy
```

**Setup**:
```bash
ollama pull moondream      # Fast, lightweight
ollama pull llava          # Better accuracy, larger
```

**Via Hermes CLI (also works)**:
```bash
hermes config set auxiliary.vision.provider ollama
hermes config set auxiliary.vision.model moondream:latest
```

---

## Windows App Name Resolution

### Problem: Display Name vs Process Name
`capture(app="Microsoft Edge")` fails with "no on-screen window matched"

### Solution: Use Process Name
```python
# ✅ Correct
computer_use(action="capture", app="msedge.exe", mode="som")

# ❌ Incorrect
computer_use(action="capture", app="Microsoft Edge", mode="som")
```

### Discovery Pattern
```python
# Always verify with list_apps first
apps = computer_use(action="list_apps")
# Find app with bundle_id containing "msedge.exe"
edge_app = next(a for a in apps["apps"] if "msedge.exe" in a.get("bundle_id", ""))
```

---

## Edge Browser Automation Patterns

### Launch Edge Reliably
```python
# Method 1: Full path (most reliable)
terminal(command=r'"C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" https://www.netflix.com')

# Method 2: start command
terminal(command='start msedge https://www.netflix.com')
```

### Wait for Page Load
```python
# Netflix: 8-10 seconds
# Simple pages: 3-5 seconds
computer_use(action="wait", seconds=8)
```

### Re-capture After Navigation (Critical!)
```python
# SOM indices are SINGLE-USE - always re-capture after:
# - Page navigation
# - Tab switches
# - Dropdown opens
# - Modal dialogs

computer_use(action="click", element=43, capture_after=True)
# Then re-capture for fresh indices
result = computer_use(action="capture", app="msedge.exe", mode="som")
```

### Profile Dropdown Pattern (Netflix)
```python
# 1. Capture Edge window
result = computer_use(action="capture", app="msedge.exe", mode="som")

# 2. Find "Personal Profile" button (top-right toolbar)
profile_btn = find_element(result, label_contains="Personal Profile")

# 3. Click to open dropdown
computer_use(action="click", app="msedge.exe", element=profile_btn.index, capture_after=True)

# 4. Re-capture to see dropdown menu
result = computer_use(action="capture", app="msedge.exe", mode="som")

# 5. Find target profile
target = find_element(result, label_contains="[PROFILE_NAME]")

# 6. Click profile
computer_use(action="click", app="msedge.exe", element=target.index)
```

---

## Window Management

### Show Desktop (Minimize All)
```python
# Capture screen to find "Show Desktop" button
result = computer_use(action="capture", app="screen", mode="som")
show_desktop = find_element(result, label_contains="Show Desktop")
computer_use(action="click", app="screen", element=show_desktop.index)
```

### Minimize Specific Window (Edge)
```python
# Method 1: Click minimize button via SOM (most reliable)
result = computer_use(action="capture", app="msedge.exe", mode="som", pid=PID, window_id=WID)
minimize_btn = find_element(result, label="Minimize")  # element 0
computer_use(action="click", app="msedge.exe", element=minimize_btn.index, pid=PID, window_id=WID)

# Method 2: Win+Down key combo (focus window first)
computer_use(action="focus_app", app="msedge.exe")  # targets pid/window_id
computer_use(action="key", keys="win+down", delivery_mode="foreground", pid=PID, window_id=WID)
```

### Close Specific Tab
```python
# Capture browser, find tab close button
result = computer_use(action="capture", app="msedge.exe", mode="som")
for el in result["elements"]:
    label = el.get("label", "").lower()
    if "netflix" in label and ("close tab" in label or label == "close tab"):
        computer_use(action="click", app="msedge.exe", element=el["index"])
        break
```

### Mute/Unmute YouTube Tab (Edge)
```python
# 1. Capture Edge window (need pid/window_id from list_windows)
result = computer_use(action="capture", app="msedge.exe", mode="som", pid=PID, window_id=WID)

# 2. Find "Mute tab" or "Unmute tab" button in tab bar (usually element 39)
mute_btn = find_element(result, label_contains="Mute tab")  # or "Unmute tab"
computer_use(action="click", app="msedge.exe", element=mute_btn.index, pid=PID, window_id=WID)

# Tab title updates to show "Audio muted" when muted, button label flips to "Unmute tab"

### Pause/Play YouTube Video
# 'k' key has foreground lock issues on Chromium (rejected by Windows)
# **Workaround**: Click center of video player area (coordinate ~800, 500 in captured window)
computer_use(action="click", app="msedge.exe", coordinate=[800, 500], pid=PID, window_id=WID)
# Requires fresh capture after each state change

### Minimize Edge Window
# Click "Minimize" button (element 0) via SOM - most reliable
result = computer_use(action="capture", app="msedge.exe", mode="som", pid=PID, window_id=WID)
minimize_btn = find_element(result, label="Minimize")  # element 0
computer_use(action="click", app="msedge.exe", element=minimize_btn.index, pid=PID, window_id=WID)

# Win+Down key combo has foreground lock issues - avoid
# Win+D works globally but minimizes ALL windows

### Tab Close Pattern
# Each tab has "Close tab" button (usually element 39 in current capture)
# Tab title shows "Audio playing" or "Audio muted" status
# Must re-capture after tab close - indices shift
```

### Print via Edge System Dialog (Most Reliable)
```python
# 1. Open image/PDF in Edge
terminal(command=r'"C:\\Program Files (x86)\\Microsoft\\Edge\\Application\\msedge.exe" "file:///C:/Users/<user>/screenshot.png"')

# 2. Wait for load
computer_use(action="wait", seconds=3)

# 3. Open system print dialog (Ctrl+Shift+P)
computer_use(action="key", keys="ctrl+shift+p", delivery_mode="foreground", pid=PID, window_id=WID)

# 4. Capture system print dialog (explorer.exe window)
result = computer_use(action="list_windows")
print_dialog = next(w for w in result["windows"] if "Print" in w["title"])
dialog_result = computer_use(action="capture", app="explorer.exe", mode="som", pid=print_dialog["pid"], window_id=print_dialog["window_id"])

# 5. Click "Print" button (element 27)
print_btn = find_element(dialog_result, label="Print")  # element 27
computer_use(action="click", app="explorer.exe", element=print_btn.index, pid=print_dialog["pid"], window_id=print_dialog["window_id"])
```

### Minimize Edge Window
```python
# Method 1: Click minimize button via SOM (most reliable)
result = computer_use(action="capture", app="msedge.exe", mode="som", pid=PID, window_id=WID)
minimize_btn = find_element(result, label="Minimize")  # element 0
computer_use(action="click", app="msedge.exe", element=minimize_btn.index, pid=PID, window_id=WID)

# Method 2: Win+Down key combo has foreground issues - avoid
```

### Pause YouTube Video
```python
# 'k' key has foreground lock issues - click video player area instead
computer_use(action="capture", app="msedge.exe", mode="som", pid=PID, window_id=WID)
# Click center of video player (approx 800, 500 in captured coords)
computer_use(action="click", app="msedge.exe", coordinate=[800, 500], pid=PID, window_id=WID)
```

### Access Already-Logged-In Sites via User's Browser Session (Session: 2026-08-09)
```python
# Powerful pattern: User's Edge browser already has authenticated sessions
# 1. Capture Edge to find "New Tab" button
result = computer_use(action="capture", app="msedge.exe", mode="som")
new_tab = find_element(result, label="New Tab")  # element 44
computer_use(action="click", app="msedge.exe", element=new_tab.index, capture_after=True)

# 2. Wait for new tab page, then type URL in address bar
result = computer_use(action="capture", app="msedge.exe", mode="som")
address_bar = find_element(result, label="Address and search bar")  # element 5/6
computer_use(action="type", app="msedge.exe", element=address_bar.index, text="https://web.getmarks.app", delivery_mode="foreground")

# 3. Press Enter (foreground required for text input on Chromium)
computer_use(action="key", keys="Enter", delivery_mode="foreground", app="msedge.exe")

# 4. Wait for page load (8-10s for complex apps)
computer_use(action="wait", seconds=8)

# 5. Re-capture - user is now logged in if session exists
result = computer_use(action="capture", app="msedge.exe", mode="som")
# Look for user profile indicators (e.g., "Hey, [USER_NAME]!", profile image, sidebar with user name)

# 6. Navigate within app by clicking sidebar/menu elements
tests_btn = find_element(result, label="Tests")  # element 39/40
computer_use(action="click", app="msedge.exe", element=tests_btn.index, capture_after=True)
```

### Marks App → JEE Test Series Navigation (Session: 2026-08-09)
```python
# After accessing web.getmarks.app and confirming logged in as user:
# 1. Click "Tests" in sidebar (element 39/40)
# 2. Wait 3s for Tests page load
# 3. Re-capture - look for "Custom Test", "PYQ Mock Test" cards
# 4. Click "View Details →" for desired test series (element 73, 74, 75)
# 5. Wait 3s, re-capture - lands on Quizrr (MathonGo) test series page
# 6. Available: JEE Main 2027, JEE Advanced 2027, NEET 2027, Free Resources (PYQs, Notes, Formula Sheets)
```

---

## Common Pitfalls & Fixes

| Pitfall | Fix |
|---------|-----|
| Element index stale | Re-capture after EVERY state change |
| App not found in list_windows | Wait 8-10s after launch; use `screen` capture |
| Vision quota exhausted | Use local Ollama (moondream/llava) |
| Wrong app name | Use process name (`msedge.exe`) not display name |
| Click no effect | Check verdict: `unverifiable` → re-capture; `suspected_noop` → escalate |

---

## Verification Checklist

After any automation sequence:
- [ ] Re-capture to verify final state
- [ ] Check `effect` field: `confirmed` = done, `unverifiable` = inspect fresh state
- [ ] Don't silently retry same rung - read escalation hints
- [ ] Test vision early in session before complex tasks

---

### Session Learnings (2026-08-09)

**MEDIA Delivery for Screenshots:**
- `computer_use(action="capture", mode="vision")` with NVIDIA NIM provider returns `ValueError: Received multimodal data but multimodal processing is not enabled` - provider doesn't support vision
- PowerShell screenshot script works reliably for actual PNG files
- MEDIA delivery requires copying to accessible path (profile directory) and using MSYS-style forward-slash paths

**Vision Provider Status:**
- Google AI Studio (Gemini): Quota exhausted (429 RESOURCE_EXHAUSTED)
- NVIDIA NIM (nvidia/nemotron-3-ultra): Vision not enabled (500 multimodal error)
- **Local Ollama (moondream:latest) remains the reliable default**

---

### Session Learnings (2026-08-05)

### Vision Provider Configuration
- **Google AI Studio (Gemini) free tier is easily exhausted** - all models hit 429 RESOURCE_EXHAUSTED
- **Local Ollama is the reliable default**:
  ```yaml
  # ~/.config/hermes/config.yaml
  auxiliary:
    vision:
      provider: ollama
      model: moondream:latest  # 1.7 GB, fast, free
      # model: llava:latest    # 4.1 GB, better accuracy
  ```

### Roblox Private Server Automation
- **Must have the private server link/code** - cannot join without it
- Game URL pattern: `https://www.roblox.com/games/126509999165004/Grow-a-Garden-2?privateServerLinkCode=YOUR_CODE`
- Protocol handler: `roblox://placeId=126509999165004` (opens game page, not private server directly)
- Roblox player needs **15-20 seconds** to fully load after launch

### Wait Time Guidelines
| Task | Wait Time |
|------|-----------|
| Netflix load | 8-12 seconds |
| Roblox player launch | 15-20 seconds |
| Simple page navigation | 3-5 seconds |
| Dropdown/menu open | 1-2 seconds |
| Tab close | 1 second |

### Netflix Profile Selection (Corrected)
- **Profile selection happens on Netflix's "Who's watching?" page** (profile avatars)
- **NOT** on Edge's "Personal Profile" toolbar button
- Verified workflow: 1) Start msedge to netflix.com, 2) Wait 8-12s, 3) Click profile avatar by label, 4) Profile loads

### SOM Index Lifecycle
- **SOM indices are SINGLE-USE** - only valid until next `capture`
- **Must re-capture after EVERY state change**: page navigation, tab switch, dropdown open, modal, tab close
- Pattern: `click(element=N, capture_after=True)` → then fresh `capture(app=..., mode="som")`

### Verification Protocol
1. After `click`/`type`/`key`: check `effect` field
2. `confirmed` = done
3. `unverifiable` = re-capture fresh state before any retry
4. `suspected_noop` or structured refusal → climb escalation ladder
5. **Never silently retry the same rung**

---

## Related Skills

- `computer-use` - Core automation (bundled)
- `netflix-profile-switcher` - Open Netflix + switch profile
- `netflix-cleanup` - Close tab + minimize all
- `netflix-manager` - Combined workflow

## References

- `references/vision-setup-2026-08-07.md` — Local vision config via `hermes config set` (Ollama + moondream)
- `references/screenshot-capture-2026-08-07.md` — PowerShell script for actual screenshot files
- `references/netflix-automation-2026-08-05.md`
- `references/roblox-automation-2026-08-05.md`
- `references/session-learnings-2026-08-05.md`

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`[HERMES_HOME]\skills\desktop\windows-desktop-automation\`

---

*Source: `skill_view('windows-desktop-automation')`*
*Updated: 2026-08-10*
tags: [skill, desktop, #skill/desktop]
parent: "[[Ops/Skills-Registry/desktop/MOC-Desktop]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
