# 🛠️ Skill: automated-screen-ocr

> **Source**: `skills/automated-screen-ocr/SKILL.md` • **Category**: Skills

---

## Overview

**User Preference:** Concise and fast interactions; avoid verbose explanations.
This skill provides a fully automated workflow to capture the current Windows desktop screen, run OCR with Tesseract, and return the extracted text without needing the user to manually upload an image.

## Prerequisites

- **ffmpeg** with `gdigrab` support (install via Chocolatey: `choco install ffmpeg`).
- **Tesseract OCR** (install via Chocolatey: `choco install tesseract`).
- The `$HOME` environment variable should resolve to the user's home directory (e.g., `C:\Users\<username>`).

## Steps

1. **Confirm explicit on-demand scope**
   Capture only after the user specifically asks to inspect their screen or asks for an action that requires inspecting it. Do not monitor the desktop continuously.
2. **Capture Screenshot**
   In the command's current working directory, use a simple filename and verify the file was created:
   ```bash
   ffmpeg -y -f gdigrab -framerate 1 -t 1 -i desktop -frames:v 1 screenshot.png
   ```
   FFmpeg can warn about image-sequence naming for a one-frame PNG; that is acceptable when the command succeeds and `screenshot.png` exists.
3. **Create Output Directory**
   ```bash
   mkdir -p ocr_output
   ```
4. **Run Tesseract OCR**
   Use Tesseract on the captured PNG. On Windows Git Bash/MSYS, prefer native Windows paths when invoking `tesseract.exe` directly:
   ```bash
   "C:\\Program Files\\Tesseract-OCR\\tesseract.exe" "C:\\Users\\<username>\\screenshot.png" "C:\\Users\\<username>\\ocr_output\\screenshot_ocr" -l eng
   ```
   This produces `ocr_output/screenshot_ocr.txt`.
5. **Read and assess the result**
   Use the file-reading tool to return the OCR text. For blurred mathematical notation or an unclear question number, do not guess—ask the user to zoom the relevant area and capture a fresh snapshot.

For the Windows/Git Bash command-path details, see [references/windows-git-bash-paths.md](references/windows-git-bash-paths.md).

## Pitfalls & Tips

- Ensure both `ffmpeg` and `tesseract` are on the system `PATH`. After installation, restart the shell or run `refreshenv`.
- High‑resolution screenshots produce better OCR results. If the image is very large (>5 MB), consider resizing before OCR.
- Language data: use `-l <lang>` to specify other languages (e.g., `-l deu` for German).

## Fallback

If any command fails (missing binary, permission issue), fall back to the manual `screen-analysis` flow that asks the user to upload a screenshot and uses the `ocr-and-documents` skill.

---

*Reference scripts and documentation are stored under the `references/` directory of this skill.*tags: [skill, skills, #skill/skills]
parent: "[[Ops/Skills-Registry/skills/MOC-Skills]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
