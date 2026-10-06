---
name: mermaid-cli
description: Use when installing or running mermaid-cli (mmdc) to render, compile, or visually optimize Mermaid diagrams to PNG, SVG, or PDF across Linux, macOS, or Windows with zero bloat.
---

# mermaid-cli (mmdc)

Standardized skill for compiling, exporting, and visually optimizing Mermaid diagrams using the official `@mermaid-js/mermaid-cli` (`mmdc`). Cross-platform, zero bloat, headless-ready.

---

## When to Use
- Compile Mermaid source files (`.mmd`, `.mermaid`) to static assets (`.svg`, `.png`, `.pdf`).
- Extract and render ` ```mermaid ` code blocks from Markdown files (`.md`).
- Generate enterprise-grade architecture, sequence, and state machine diagrams.
- Optimize diagram visual geometry, font scaling, text wrapping, and color semantics.

### When NOT to Use
- Native chat or artifact UI diagram rendering (use inline Mermaid blocks instead).
- Plain Mermaid syntax editing without image export or verification needs.

---

## 1. Golden Rules

1. **Official CLI Only:** Always use `@mermaid-js/mermaid-cli` (`mmdc`). Never install or use unofficial wrappers like `mermaidx`.
2. **Zero Bloat:** Do not install heavy desktop apps or extra plugins. Use bundled Puppeteer Chromium or system Chromium.
3. **Mandatory White Background:** Always pass `-b "#FFFFFF"` unless explicitly asked for transparent assets (`-b transparent`). Default without `-b` causes transparent PNGs with unreadable dark text in Word/dark mode.
4. **High Resolution Raster:** Always pass `-s 2` (minimum scale 2x) for PNG to prevent blurry renders. For dense diagrams (sequence > 15 steps, wide ERDs), use `-s 5` (crisp 4K assets).
5. **Vector for Documents:** Export `.svg` alongside `.png` when embedding into Word (DOCX) or print layouts for infinite zoom without pixelation.
6. **Zero Emojis:** Never use emojis in technical enterprise diagrams.
7. **Inspect Output:** Never assume render succeeded visually. Always use `view_file` (Multimodal Vision) to inspect output PNG for text overflow, line collisions, and geometry defects.

---

## 2. Quick Reference CLI Commands

| Action | Command |
|---|---|
| Single file to SVG (Vector) | `mmdc -i input.mmd -o output.svg` |
| Single file to PNG (Standard) | `mmdc -i input.mmd -o output.png -s 2 -b "#FFFFFF"` |
| High-Density 4K PNG (Dense) | `mmdc -i input.mmd -o output.png -s 5 -b "#FFFFFF"` |
| Single file to PDF | `mmdc -i input.mmd -o output.pdf` |
| Stdin input (PowerShell) | `Get-Content input.mmd \| mmdc -i - -o output.png -b "#FFFFFF"` |
| Process Markdown file | `mmdc -i docs.md -o docs-rendered.md -a ./assets/diagrams` |
| Puppeteer Sandbox config | `mmdc -i input.mmd -o output.svg -p puppeteer-config.json` |

---

## 3. Visual Verification Loop

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Write / Edit│ ──> │ Compile mmdc │ ──> │  view_file   │ ──> │ Pass / Refine│
│     .mmd     │     │  PNG (-s 2)  │     │ Vision Audit │     │   Geometry   │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
```

1. **Render PNG:** `mmdc -i input.mmd -o output.png -s 2 -b "#FFFFFF"`.
2. **Inspect via Vision:** Call `view_file` on `output.png`.
3. **Check Quality:**
   - Are participant or note box texts clipped or overflowing boundaries?
   - Are lifelines or nodes spaced thousands of pixels apart due to long label text?
   - Are arrows crossing awkwardly or obscuring arrowhead labels?
   - Do colors follow enterprise semantics (green=success, amber=pending, red=error only, slate/cyan=ops)?
4. **Iterate:** Apply targeted `<br/>` wraps or layout adjustments and re-verify until pristine.

---

## 4. Deep-Dive References (Modular Guides)

For specialized patterns, edge cases, and architectural best practices, consult the dedicated guides in `references/`:

| Topic | Reference Document | Key Techniques |
|---|---|---|
| **Flowchart Anti-Spaghetti** | [flowchart-optimization.md](references/flowchart-optimization.md) | Bus routing (1-to-N trunk bar), strict column locking (`~~~`), anti-ballooning diamonds, footer legend. |
| **Sequence Diagrams** | [sequence-optimization.md](references/sequence-optimization.md) | Actor `<br/>` wrapping, narrowing huge lifeline gaps (-60% width), multi-actor note spanning. |
| **Syntax & Escaping** | [syntax-and-escaping.md](references/syntax-and-escaping.md) | Escaping `#` with `#35;` (ticket/URL hashes), quotes `&quot;`, comparisons `&lt;=`, node shapes. |
| **Enterprise Styling** | [enterprise-styling.md](references/enterprise-styling.md) | Semantic palette hygiene, avoiding red for Ops lanes, component vs sequence boundaries. |
| **Word & DOCX Export** | [document-export-docx.md](references/document-export-docx.md) | `-b "#FFFFFF"`, high-DPI scaling (`-s 5`), SVG vector pairing, page aspect ratio fitting. |
| **Troubleshooting & CI** | [troubleshooting-and-ci.md](references/troubleshooting-and-ci.md) | Puppeteer sandbox fixes (`--no-sandbox`), headless Linux fonts, PowerShell paths. |

---

## 5. Common Pitfalls & Quick Solutions

| Issue | Root Cause | Solution |
|---|---|---|
| Messy 1-to-N curving splines | Dagre routes each edge independently | Insert a thin dummy `BUS` bar (`height:2px`) between source and targets. |
| Flowchart 3x taller than needed | Default 50px spacing & giant diamonds | Add `%%{init: {"flowchart": {"rankSpacing": 25, "nodeSpacing": 20}}}%%` & wrap text. |
| Return loop scrambles column order | Dagre recomputes ranks on feedback cycles | Lock column ranks with `ERR1 ~~~ ERR2 ~~~ ERR3`, or use dotted/label returns. |
| Missing diagram legend | Subgraph loose nodes stack vertically | Embed HTML table with inline mini SVGs into a single transparent footer node. |
| Black / unreadable PNG in Word | Transparent background by default | Add `-b "#FFFFFF"` flag. |
| Fuzzy / blurry text in Word | Scale factor default is 1 (96 DPI) | Use `-s 2` for standard, `-s 5` for dense diagrams. |
| Parse error on `#ticket` or `#1` | `#` parsed as hex color / reserved token | Replace `#` with `#35;` numeric entity. |
| Lifelines 1200px+ apart | Long horizontal string on single message line | Insert `<br/>` into message label to split across 2–3 lines. |
| Note box overflows edge | Note placed over single narrow actor | Add `<br/>` or span note across multiple actors (`Note over A,B:`). |
| Docker / CI browser launch failure | Linux sandbox restrictions | Pass `-p puppeteer-config.json` with `--no-sandbox`. |
