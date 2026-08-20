# 📝 Skill: windows-printing

> **Category**: desktop • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
Print files on Windows reliably via command line.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

# Overview
This skill encapsulates a class‑level workflow for printing files (images, PDFs) on a Windows 10 machine using built‑in tools and common utilities. It is tuned for the user **Takopi**, who prefers short, direct instructions and fast, repeatable automation.

## Prerequisites
- A working printer installed in **Settings → Printers & scanners**.
- The printer driver must be functional (test via *Print a test page*).
- `ffmpeg` (for image → PDF conversion) – install with `choco install ffmpeg` if not present.
- `mspaint` is available by default (used for the `/pt` verb).

## Core Steps (image or PDF)
1. **Identify the exact printer name** (often includes "Printer" suffix):
   ```powershell
   Get-Printer | Select-Object Name, PrinterStatus, Default | Format-Table -AutoSize
   ```
   On this system: **`Brother DCP-T535DW Printer`** (not `Brother DCP-T535DW`).

2. **Set the printer as default** (helps verbs that omit `-d`):
   ```bash
   rundll32.exe printui.dll,PrintUIEntry /y /n "Brother DCP-T535DW Printer"
   ```

3. **If printing a PNG/JPEG, convert to PDF** (more reliable with the Windows print subsystem):
   ```bash
   ffmpeg -y -i "C:\\\\Users\\\\<user>\\\\screenshot.png" "C:\\\\Users\\\\<user>\\\\screenshot.pdf"
   ```

4. **Print the file** – reliable alternatives:
   - **Edge + system dialog** (Ctrl+Shift+P in Edge PDF viewer) — **most reliable for images**:
     1. Open image in Edge: `"C:\\Program Files (x86)\\Microsoft\\Edge\\Application\\msedge.exe" "file:///C:/Users/<user>/screenshot.png"`
     2. Wait 3s for load
     3. Press `Ctrl+Shift+P` → system print dialog → select printer → Print
   - **PowerShell Verb Print** (works for PDFs and images):
     ```powershell
     Start-Process -FilePath "C:\\\\Users\\\\<user>\\\\screenshot.pdf" -Verb Print
     ```
   - **mspaint /pt** (explicit printer name — include "Printer" suffix):
     ```bash
     mspaint /pt "C:\\\\Users\\\\<user>\\\\screenshot.png" "Brother DCP-T535DW Printer"
     ```
     ⚠ This command may hang; run with `background=true, notify_on_complete=true` and a generous timeout.

5. **Test the printer** (verify it's responsive):
   ```bash
   rundll32.exe printui.dll,PrintUIEntry /k /n "Brother DCP-T535DW Printer"
   ```

6. **Verify the job** (optional):
   ```powershell
   Get-PrintJob -PrinterName "Brother DCP-T535DW Printer" | Format-Table Id, Name, JobStatus, SubmittedTime
   ```

## Edge + System Dialog: Computer-Use Automation Pattern
```python
# 1. Open image in Edge
terminal(command=r'"C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" "file:///C:/Users/<user>/screenshot.png"')

# 2. Wait for load
computer_use(action="wait", seconds=3)

# 3. Capture Edge to get pid/window_id
result = computer_use(action="list_windows")
edge = next(w for w in result["windows"] if "msedge.exe" in w["app_name"])

# 4. Open system print dialog (Ctrl+Shift+P) - MUST use foreground delivery
computer_use(action="key", keys="ctrl+shift+p", delivery_mode="foreground", pid=edge["pid"], window_id=edge["window_id"])

# 5. Wait for dialog, then capture system print dialog (explorer.exe)
import time; time.sleep(2)
result = computer_use(action="list_windows")
print_dialog = next(w for w in result["windows"] if "Print" in w["title"])
dialog = computer_use(action="capture", app="explorer.exe", mode="som", pid=print_dialog["pid"], window_id=print_dialog["window_id"])

# 6. Click "Print" button (element 27)
print_btn = find_element(dialog, label="Print")
computer_use(action="click", app="explorer.exe", element=print_btn.index, pid=print_dialog["pid"], window_id=print_dialog["window_id"])
```

## Pitfalls & Tips
- **Default printer**: If the printer isn’t default, many `Print` verbs silently drop the job. Use the `/y /n` command or set it manually in Settings.
- **File type**: PNGs often fail with the plain `Print` verb; converting to PDF resolves this.
- **Spooler issues**: Restart with `net stop spooler && net start spooler` if jobs disappear.
- **Background processes**: When using `terminal` with `background=true`, remember to `wait` for completion or check `Get-PrintJob`.
- **Timeouts**: Printing commands can hang; increase the timeout (max 600 s) for long jobs.

## References
- `references/printing-session-2026-08-07.md` – detailed transcript of the August 2026 troubleshooting session, including failed attempts and the final successful Edge + system dialog pattern.
- `references/printing-session-20260805.md` – earlier troubleshooting session transcript.

---
*This skill follows Takopi’s preference for concise, direct instructions.*

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`C:\Users\soham\AppData\Local\hermes\profiles\cosmos\skills\desktop\windows-printing\`

---

*Source: `skill_view('windows-printing')`*
*Updated: 2026-08-10*
tags: [skill, desktop, #skill/desktop]
parent: "[[Ops/Skills-Registry/desktop/MOC-Desktop]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
