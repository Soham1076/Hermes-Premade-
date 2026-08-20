# 📸 Skill: screen-analysis

> **Source**: `skills/screen-analysis/SKILL.md` • **Category**: Skills

---

## Overview

This skill enables Hermes to analyze a screenshot only when the user explicitly requests it. It guides the user to provide an image, then runs OCR to extract visible text and returns the result.

## Steps

1. **Prompt for image**
   Ask the user:
   ```
   Please upload a screenshot (PNG, JPG, or JPEG) of the screen you want analyzed.
   ```

2. **When image is received**
   - Save the image to a temporary location, e.g., `$HOME/screenshot.png`.
   - Run OCR using the existing `ocr-and-documents` tool (which uses `pymupdf`):
     ```bash
     hermes tools ocr-and-documents --input $HOME/screenshot.png --output $HOME/screenshot_ocr.txt
     ```
   - Read the OCR result file and send the extracted text back.

3. **Fallback**
   - If OCR returns an empty result, inform the user and optionally offer a simple image caption using any available captioning script.

## Pitfalls & Tips

- OCR works best with clear, high‑resolution screenshots; low‑resolution images may produce poor results.
- Large images (>5 MB) may need to be resized before OCR; this skill does not handle resizing automatically.
- Ensure the image file is saved under the user's home directory (`/c/Users/[username]/...`) so subsequent shell commands can access it.

## Verification

After OCR, read the generated `$HOME/screenshot_ocr.txt` and verify it contains at least one non‑whitespace character before responding.

## Example Interaction

```
User: /screen-analyze
Hermes: Please upload a screenshot.
(User uploads image)
Hermes: Running OCR…
Hermes: Extracted text:
"Username: admin
Password: ********"
```

## Integration

Trigger this skill via a custom command such as `/screen-analyze` or any prompt you define. The skill does **not** run automatically; it only activates after explicit user instruction.tags: [skill, skills, #skill/skills]
parent: "[[Ops/Skills-Registry/skills/MOC-Skills]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
