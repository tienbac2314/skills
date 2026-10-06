---
name: mermaid-cli
description: Use when generating, compiling, or exporting Mermaid diagrams to SVG, PNG, or PDF images, or running the mmdc command-line tool.
---

# mermaid-cli (mmdc)

## Overview
CLI tool (`mmdc`) for `@mermaid-js/mermaid-cli`. Converts Mermaid text definitions and markdown-embedded charts into SVG, PNG, and PDF images.

## When to Use
- Convert Mermaid files (`.mmd`, `.mermaid`) to static assets (`.svg`, `.png`, `.pdf`).
- Extract and render ` ```mermaid ` blocks from Markdown files (`.md`).
- Generate diagram images programmatically or in build pipelines.
- Verify Mermaid diagram syntax via CLI render.

### When NOT to Use
- Antigravity native UI diagram rendering (use inline Mermaid blocks in artifacts or chat instead).
- Plain Mermaid editing without asset export.

## Quick Reference

| Action | Command |
|---|---|
| Single file to SVG | `mmdc -i input.mmd -o output.svg` |
| Single file to PNG | `mmdc -i input.mmd -o output.png` |
| Single file to PDF | `mmdc -i input.mmd -o output.pdf` |
| Stdin input | `echo "graph TD; A-->B;" \| mmdc -i - -o output.svg` |
| Process Markdown | `mmdc -i docs.md -o docs-rendered.md` |
| Apply theme | `mmdc -i input.mmd -o output.svg -t dark` |
| Transparent background | `mmdc -i input.mmd -o output.png -b transparent` |
| High resolution (scale) | `mmdc -i input.mmd -o output.png -s 2` |
| Custom CSS | `mmdc -i input.mmd -o output.svg -C custom.css` |
| Mermaid JSON config | `mmdc -i input.mmd -o output.svg -c mermaid.json` |
| Puppeteer config | `mmdc -i input.mmd -o output.svg -p puppeteer-config.json` |

## Usage Patterns

### 1. Basic File Conversion
```bash
# Default output is <input>.svg
mmdc -i diagram.mmd

# Explicit format and destination
mmdc -i diagram.mmd -o dist/diagram.png
mmdc -i diagram.mmd -o dist/diagram.pdf
```

### 2. Stdin / Pipeline Rendering
```bash
# Pipe Mermaid string directly
cat diagram.mmd | mmdc -i - -o output.svg

# In PowerShell
Get-Content diagram.mmd | mmdc -i - -o output.svg
```

### 3. Markdown Document Processing
Processes markdown files, renders all ` ```mermaid ` or `:::mermaid` blocks, replaces code blocks with image links, and saves output images:
```bash
# Output modified markdown; images saved to output directory
mmdc -i README.md -o README.rendered.md

# Specify custom artifacts folder for generated images
mmdc -i README.md -o README.rendered.md -a ./assets/diagrams
```

### 4. Themes and Appearance
Available themes (`-t`):
`default`, `neutral`, `dark`, `forest`, `base`, `neo`, `neo-dark`, `redux`, `redux-dark`, `redux-color`, `redux-dark-color`, `null`.

```bash
# Dark theme with transparent background
mmdc -i diagram.mmd -o diagram.png -t dark -b transparent

# High-DPI raster export
mmdc -i diagram.mmd -o diagram.png -s 3 --size 2048
```

### 5. Custom Styling and Configuration
Mermaid JSON config (`mermaid-config.json`):
```json
{
  "theme": "forest",
  "flowchart": {
    "curve": "basis",
    "htmlLabels": true
  }
}
```
Run with config:
```bash
mmdc -i diagram.mmd -o output.svg -c mermaid-config.json -C custom.css
```

### 6. Puppeteer Configuration (CI / Container / Sandbox)
If Chromium fails to launch in sandboxed or container environments, create `puppeteer-config.json`:
```json
{
  "args": ["--no-sandbox", "--disable-setuid-sandbox"]
}
```
Run with:
```bash
mmdc -i diagram.mmd -o output.svg -p puppeteer-config.json
```

## Common Mistakes

| Problem | Cause | Solution |
|---|---|---|
| `No usable sandbox` error | Puppeteer running in Linux CI/Docker without sandbox permissions | Pass `-p puppeteer-config.json` with `--no-sandbox`. |
| Huge SVG file size | Font embedding enabled by default | Add `--no-font-embed` flag. |
| Blurry PNG export | Default scale factor 1 | Add `-s 2` or `-s 3` for Retina/high-res rendering. |
| Cut-off PDF output | Diagram size mismatch with page format | Use default diagram fitting or pass `--pdf-paper-format A4`. |
| Missing diagram in Markdown | Syntax error in ` ```mermaid ` block | Verify diagram definition independently before batch rendering. |
