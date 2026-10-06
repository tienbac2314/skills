# mermaid-cli Setup & Installation Guide

This guide explains how to install and configure `@mermaid-js/mermaid-cli` (`mmdc`) for users, agents, and CI/CD pipelines.

The companion agent instructions are in [`SKILL.md`](SKILL.md) (the skill itself contains runtime CLI execution patterns, while this README documents environment setup).

---

## 1. Prerequisites

- **Node.js**: v18.0.0 or higher.
- **npm**, **pnpm**, or **yarn**.

---

## 2. Installation Options

### Option A: Global Installation (Recommended for Local Machine / Agent Host)

Installs the `mmdc` executable globally on your system `PATH`:

```bash
# npm
npm install -g @mermaid-js/mermaid-cli

# pnpm
pnpm add -g @mermaid-js/mermaid-cli

# yarn
yarn global add @mermaid-js/mermaid-cli
```

Verify installation:
```bash
mmdc --version
```

---

### Option B: Zero-Install via `npx` (Recommended for Ephemeral Agents & CI)

If running in an ephemeral environment or agent workspace where global packages cannot be installed:

```bash
# Direct execution without permanent install
npx -p @mermaid-js/mermaid-cli mmdc -i input.mmd -o output.svg
```

---

### Option C: Project Local Dependency

If you are using it within a specific project repository:

```bash
npm install --save-dev @mermaid-js/mermaid-cli
```
Run via npm scripts or `npx mmdc`.

---

## 3. Environment & Sandbox Configuration (Linux / Docker / CI)

`mermaid-cli` uses Puppeteer (headless Chromium) to render diagrams. In Linux, containerized, or sandboxed agent environments, Chromium may require sandbox flags.

### Puppeteer Config File (`puppeteer-config.json`)
Create a configuration file:
```json
{
  "args": ["--no-sandbox", "--disable-setuid-sandbox"]
}
```

Pass it to `mmdc`:
```bash
mmdc -i input.mmd -o output.svg -p puppeteer-config.json
```

### Required Linux System Libraries (Ubuntu/Debian)
If Chromium fails to launch due to missing shared libraries:
```bash
sudo apt-get update && sudo apt-get install -y \
  libasound2 \
  libatk-bridge2.0-0 \
  libatk1.0-0 \
  libcairo2 \
  libcups2 \
  libdbus-1-3 \
  libdrm2 \
  libgbm1 \
  libglib2.0-0 \
  libnspr4 \
  libnss3 \
  libpango-1.0-0 \
  libx11-6 \
  libxcb1 \
  libxcomposite1 \
  libxdamage1 \
  libxext6 \
  libxfixes3 \
  libxrandr2
```

---

## 4. Usage with Antigravity / AI Agents

Once `mmdc` is in your `PATH` (or accessible via `npx`), install the skill into your agent directory:

```bash
# Global install in Antigravity
cp -r mermaid-cli ~/.gemini/config/skills/
```

The agent will then automatically invoke `mmdc` whenever generating, compiling, or exporting diagrams according to the instructions in [`SKILL.md`](SKILL.md).

For advanced layout optimization, visual geometry, and troubleshooting, refer to the guides in [`references/`](references/):
- [`references/flowchart-optimization.md`](references/flowchart-optimization.md) (Bus routing, column locking `~~~`, anti-ballooning)
- [`references/sequence-optimization.md`](references/sequence-optimization.md) (Actor wrapping, lifeline gap reduction)
- [`references/syntax-and-escaping.md`](references/syntax-and-escaping.md) (Escaping `#35;`, quotes, comparison entities)
- [`references/enterprise-styling.md`](references/enterprise-styling.md) (Semantic colors, unidirectional flows)
- [`references/document-export-docx.md`](references/document-export-docx.md) (White backgrounds, high-DPI `-s 5`, Word sizing)
- [`references/troubleshooting-and-ci.md`](references/troubleshooting-and-ci.md) (Puppeteer `--no-sandbox`, Linux fonts)
