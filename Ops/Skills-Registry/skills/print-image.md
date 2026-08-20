# 🖨️ Skill: print-image

> **Source**: `skills/print-image/SKILL.md` • **Category**: Skills

---

## Overview

This skill prints a local image file by:
1. Converting the image to PDF using Pillow (Python) – no external tools required.
2. Opening the PDF in Microsoft Edge.
3. Clicking the **Print** button in Edge's PDF viewer.
4. Selecting the target printer (default is `Brother DCP‑T535DW`).
5. Confirming the print dialog.

## Prerequisites

- Python 3 with the `Pillow` library installed (`pip install pillow`).
- Microsoft Edge installed at the default location (`C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe`).
- The desired printer must be installed and visible to the OS (e.g., `Brother DCP‑T535DW`).
- The user has permission to launch Edge and print.

## Steps

```bash
# 1. Convert image to PDF (in‑place)
python - <<'PY'
from PIL import Image, ImageFile
import sys, os
ImageFile.LOAD_TRUNCATED_IMAGES = True
img_path = sys.argv[1]
pdf_path = os.path.splitext(img_path)[0] + '.pdf'
im = Image.open(img_path)
im.save(pdf_path, 'PDF')
print('PDF created at', pdf_path)
PY "${IMAGE_PATH}"
```
```bash
# 2. Open the PDF in Edge (background process)
"C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" "file://${PDF_PATH}" &
```
```bash
# 3. Click the Print button in Edge's PDF viewer (element 53)
hermes computer-use click --element 53
```
```bash
# 4. Click the final Print button in the Windows print dialog (element 35)
hermes computer-use click --element 35
```

## Parameters

- `IMAGE_PATH` – absolute path to the image you want to print (e.g., `C:\Users\soham\screenshot.png`).
- `PRINTER_NAME` – optional; defaults to `Brother DCP‑T535DW`. Adjust the printer selection in the system dialog if needed.

## Usage Example

```bash
hermes skill run print-image -- IMAGE_PATH="C:\Users\soham\screenshot.png"
```
This will print the image on the configured printer.tags: [skill, skills, #skill/skills]
parent: "[[Ops/Skills-Registry/skills/MOC-Skills]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
