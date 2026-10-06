# Document Export & Word (DOCX) Optimization

Best practices for compiling Mermaid diagrams into high-resolution assets suitable for Word (DOCX), PDF reports, and executive presentations.

---

## 1. Mandatory White Background (`-b "#FFFFFF"`)

### The Problem
By default, `mmdc` exports PNG images with a **transparent background**. When embedded into:
* Microsoft Word with Dark Mode enabled
* PDF viewers with dark themes
* Markdown previewers with custom styling

All dark text labels, dark arrows, and black borders become invisible against a dark or variable background.

### The Rule
**Always pass `-b "#FFFFFF"`** unless the user explicitly requests transparent assets (`-b transparent`):
```bash
mmdc -i diagram.mmd -o diagram.png -s 2 -b "#FFFFFF"
```

---

## 2. High-DPI Scaling (`-s 2` to `-s 5`)

In Microsoft Word, a standard portrait page with 1-inch margins has a printable width of **6.5 inches (165 mm)**:
* Standard 96 DPI export (`-s 1`): An 800px image printed at 6.5 inches renders at ~123 DPI, resulting in blurry, fuzzy text.
* **Baseline standard:** Always use `-s 2` (minimum 2x scale, ~200–300 DPI equivalent).
* **Dense diagrams (e.g., Sequence > 15 steps, large ERD, complex flowcharts):** Use **`-s 5`**.

### Case Study: Diagram 12 Customer Login Sequence
* Scaled with `-s 5`: Generated a razor-sharp **3920 × 5575 px** PNG.
* When inserted into Word and zoomed to 500%, text and lines remain crystal clear without pixelation.

---

## 3. Vector SVG Pairing

Always compile `.svg` alongside `.png`:
```bash
# 1. High-res raster for immediate preview and legacy compatibility
mmdc -i diagram.mmd -o diagram.png -s 5 -b "#FFFFFF"

# 2. Vector SVG for modern Word (Office 2016+) and infinite vector zoom
mmdc -i diagram.mmd -o diagram.svg
```

Embedding `.svg` directly into Word allows readers to zoom infinitely with zero raster degradation.

---

## 4. Aspect Ratio & Page Geometry

| Diagram Aspect Ratio | Recommended Layout in Word | Direction in Mermaid |
|---|---|---|
| **1:1 to 1:2 (Portrait / Balanced)** | Standard Portrait Page | `flowchart TD`, `sequenceDiagram` |
| **2:1 to 3:1 (Wide Landscape)** | Full-width Portrait with `<br/>` wrapping | Use `flowchart TD` instead of `LR` |
| **> 3:1 (Ultra-wide)** | Dedicated **Landscape Section** in Word | Multi-zone architecture, 8+ actor sequence |

### Preventing the "Ultra-Wide Shrink" Bug
If a diagram is 2400px wide and only 400px tall (6:1 ratio), Word constrains the width to 6.5 inches. The resulting height is barely 1 inch, shrinking the font size from readable 12pt down to microscopic 2pt.

**Remedies:**
1. Switch horizontal `flowchart LR` to top-down `flowchart TD`.
2. Stack parallel subgraphs vertically instead of side-by-side.
3. Break long participant names with `<br/>` to narrow total canvas width.
