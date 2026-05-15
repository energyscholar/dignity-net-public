# Dignity Net Public Export Workflow

This repo contains the public-facing copies of Dignity Net materials. The canonical full-stack markdown source currently lives outside this repo at:

- `/Users/genevieveprentice/Documents/Dignity Core/01_DignityNet/DN-FullStack_v1.1_2026-04-07.md`

The public PDF in this repo is an exported artifact, not the authoring source.

## Prerequisites

- `pandoc` installed locally
- Google Chrome installed at:
  - `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`
- `pdftotext` available for verification

## Rebuild The Public DN PDF

1. Render the canonical markdown source to standalone HTML:

```bash
pandoc '/Users/genevieveprentice/Documents/Dignity Core/01_DignityNet/DN-FullStack_v1.1_2026-04-07.md' \
  -s \
  -o /private/tmp/DN-FullStack_v1.1_2026-04-07.html \
  --metadata title='DN-FullStack_v1.1_2026-04-07'
```

2. Print the HTML to PDF with Chrome headless, suppressing print headers and footers:

```bash
'/Applications/Google Chrome.app/Contents/MacOS/Google Chrome' \
  --headless=new \
  --disable-gpu \
  --no-pdf-header-footer \
  --print-to-pdf='/private/tmp/DN-FullStack_v1.1_2026-04-07.clean.pdf' \
  'file:///private/tmp/DN-FullStack_v1.1_2026-04-07.html'
```

3. Verify that the rights line is present in the rebuilt PDF:

```bash
'/opt/homebrew/bin/pdftotext' '/private/tmp/DN-FullStack_v1.1_2026-04-07.clean.pdf' - | rg 'Copyright|All rights reserved|Public reading'
```

4. Copy the verified PDF into:

- this public repo:
  - `/Users/genevieveprentice/Documents/dignity-net-public/DN-FullStack_v1.1_2026-04-07.pdf`
- provenance storage:
  - `/Users/genevieveprentice/Documents/Dignity Core/01_DignityNet/Full_Stack_Provenance/pdfs/DN-FullStack_v1.1_2026-04-07.pdf`

## Notes

- The rights line should live in the source markdown itself so every downstream export inherits it.
- If the public markdown paper changes rights language, keep it aligned with the full-stack source and `LICENSE.md`.
- If Chrome or Pandoc locations change, update this file rather than relying on memory.
