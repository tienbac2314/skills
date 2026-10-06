# Troubleshooting & Headless CI Reference

Resolving execution errors, sandbox permissions, and font rendering anomalies with `@mermaid-js/mermaid-cli`.

---

## 1. Official CLI Tooling Only

Always use the official `@mermaid-js/mermaid-cli` package (`mmdc`):
```bash
# Verify installation
mmdc --version
```

### Prohibited Tools
* **`mermaidx`**: Unofficial wrapper with broken argument parsing and unmaintained dependencies.
* Generic online render APIs: Violate enterprise data privacy policies.

---

## 2. The Visual Inspection Loop Protocol

Never assume a diagram looks acceptable just because `mmdc` returned exit code 0. Syntactically valid Mermaid can still produce hideous, overlapping, or clipped graphics.

### The 4-Step Verification Loop
1. **Compile:** Run `mmdc -i input.mmd -o output.png -s 2 -b "#FFFFFF"`.
2. **Inspect:** Call `view_file` (Multimodal Vision) on `output.png`.
3. **Audit:** Check visually for:
   - Text overflowing outside node or note boundaries.
   - Arrow crossings or labels obscuring arrowheads.
   - Lifelines stretched too far apart.
   - Unintended color semantics (e.g. red for ops).
4. **Refine:** Edit `.mmd`, re-compile, and re-inspect until pristine.

---

## 3. Linux CI, Docker & Container Environments

In containerized or root-privileged environments (Docker, GitHub Actions, GitLab CI), Chromium will fail with:
`Error: Failed to launch the browser process! No usable sandbox!`

### Solution: `puppeteer-config.json`
Create a configuration file:
```json
{
  "args": ["--no-sandbox", "--disable-setuid-sandbox"]
}
```

Pass the config via the `-p` flag:
```bash
mmdc -i input.mmd -o output.png -p puppeteer-config.json -b "#FFFFFF"
```

---

## 4. Headless Linux Font Fallback Issues

If labels in the generated image appear with ugly serif fonts, clipped widths, or square boxes (`tofu`), the headless container is missing basic sans-serif and CJK fonts.

### Recommended System Packages
```bash
# Ubuntu / Debian
apt-get update && apt-get install -y \
  fonts-liberation \
  fonts-noto-cjk \
  fonts-noto-color-emoji \
  libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 libexpat1 \
  libfontconfig1 libgbm1 libgcc1 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 \
  libnspr4 libnss3 libpango-1.0-0 libpangocairo-1.0-0 libx11-6 libx11-xcb1 \
  libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 \
  libxrandr2 libxrender1 libxss1 libxtst6

# Alpine
apk add --no-cache \
  chromium \
  nss \
  freetype \
  harfbuzz \
  ca-certificates \
  ttf-freefont
```

---

## 5. Windows PowerShell Nuances

* When passing raw Mermaid via stdin, use `Get-Content`:
  ```powershell
  Get-Content input.mmd | mmdc -i - -o output.png -b "#FFFFFF"
  ```
* Ensure paths with spaces are enclosed in quotes:
  ```powershell
  mmdc -i "docs/canonical/diagram.mmd" -o "docs/canonical/diagram.png" -s 2 -b "#FFFFFF"
  ```
