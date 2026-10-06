# Agent Skills (`skills`)

Agent skills for authoring, auditing, and iteratively refining technical documentation across software projects.

Compatible with **Antigravity (AGY)**, **Codex**, **Claude Code**, and agentic coding workflows supporting `SKILL.md` specifications.

---

## Two Distinct Documentation Tracks

The repository provides two specialized, cleanly differentiated documentation tracks:

```
┌────────────────────────────────────────┐   ┌────────────────────────────────────────┐
│     Track 1: Developer & Codebase      │   │    Track 2: Enterprise BA & Gov        │
│    `write-docs`  &  `review-docs`      │   │`enterprise-write-docs` & `*-review`    │
├────────────────────────────────────────┤   ├────────────────────────────────────────┤
│ • Audience: Devs, SDK users, engineers │   │ • Audience: Stakeholders, BA, PO, PM   │
│ • Focus: Code, APIs, Quickstarts,      │   │ • Focus: URD, SRS, 4-in-1 PTTKHT,      │
│   Architecture guides, Tech Nuggets    │   │   5D Business Rules, Data Scope RBAC,  │
│ • Inspiration: tldraw docs-guide       │   │   Crypto, Runbooks, UAT Minutes        │
│ • Principles: Runnable code, 1-3 paras,│   │ • Principles: Formal sign-off, RTM,    │
│   progressive disclosure, zero AI fluff│   │   audit columns, legal acceptance      │
└────────────────────────────────────────┘   └────────────────────────────────────────┘
```

---

## Skills Catalog

| Skill | Track | Primary Purpose | Key Artifacts |
| :--- | :--- | :--- | :--- |
| [`write-docs`](write-docs/SKILL.md) | **Developer & Codebase** | Author clean, high-density documentation for software engineers, SDK users, and codebase maintainers. | Quickstarts, API reference tables, codebase architecture guides, code recipes, engineering deep-dives ("nuggets"), ADRs. |
| [`review-docs`](review-docs/SKILL.md) | **Developer & Codebase** | Dual-lens evaluation loop auditing technical accuracy against repository source code and developer usability. | Code alignment (`file:line`), runnable snippets, 1-3 sentence paragraphs, progressive disclosure, zero AI fluff. |
| [`enterprise-write-docs`](enterprise-write-docs/SKILL.md) | **Enterprise BA & Gov** | Author high-governance specifications for enterprise banking, telecommunications, and mission-critical systems. | 4-in-1 Unified Spec (PTTKHT), 12-field Use Case Cards, 5D Business Rules (BR-xxx), Data Scope RBAC, Field encryption, CSDL tables. |
| [`enterprise-review-docs`](enterprise-review-docs/SKILL.md) | **Enterprise BA & Gov** | Strict compliance audit enforcing enterprise governance, quantified NFRs, CSDL audit columns, and operational runbook dependency order. | Scorecard out of 40, audit checklists for URD, SRS, HLD, LLD, CSDL, HDVH, and Test Matrices. |

---

## Track 1: Developer Documentation (`write-docs` / `review-docs`)

Modeled on the engineering principles of the **tldraw docs guide**:
- **Progressive Disclosure**: Simplest working example first, then configuration, then deep internals and edge cases.
- **High-Density Scannability**: 1 to 3 sentences per paragraph. Structured tables for parameters, flags, and options.
- **Runnable Code**: Every snippet must have valid syntax, correct imports, and realistic domain payloads (no `foo`/`bar`).
- **Technical "Nuggets"**: Engineering deep-dives that frame the hard problem, explain the key insight, walk through the solution as "what we did", and discuss tradeoffs.
- **Zero Redundancy**: Show complete implementations once; avoid repeating trimmed-down snippets across multiple sections.

---

## Track 2: Enterprise Templates (`enterprise-write-docs/templates/`)

Modular markdown blueprints ready for enterprise project execution:

- [`urd-srs-template.md`](enterprise-write-docs/templates/urd-srs-template.md): URD / SRS specification featuring the 12-field Use Case Card, 5-dimension Business Rules Catalog (`BR-SCP`, `BR-WF`, `BR-CALC`, `BR-SEC`, `BR-INT`), Data Scope RBAC model, and quantified NFRs.
- [`hld-template.md`](enterprise-write-docs/templates/hld-template.md): High-Level Design specifying multi-tier topology (Ingress, Microservices, Broker, Persistence), Active-Active / Active-Standby clustering, Data Scope RBAC Model, 7-Step Request Journey & 4 Maintenance Invariants, and Server/Service KPI tables.
- [`lld-api-template.md`](enterprise-write-docs/templates/lld-api-template.md): Low-Level Design and tabular API contracts (Request/Response headers, query/body params with M/O flags, validation, curl, success 200/201 and error 400/401/403/500 JSON schemas).
- [`csdl-db-template.md`](enterprise-write-docs/templates/csdl-db-template.md): Database design and data dictionary featuring 4-group Table Classification (Custom, Extended, Baseline, Eliminated), Field-Level Sensitive Data Encryption (AES-256-GCM + HMAC-SHA256 Blind Index), mandatory audit columns, and capacity scaling thresholds.
- [`runbook-ops-template.md`](enterprise-write-docs/templates/runbook-ops-template.md): Operations runbook with dependency-ordered startup/shutdown sequence, daily inspection checklist, and standardized troubleshooting matrix (`Symptom -> Root Cause -> Workaround -> Permanent Fix`).
- [`testcase-matrix-template.md`](enterprise-write-docs/templates/testcase-matrix-template.md): Test Traceability Matrix (RTM), multi-browser compatibility matrix (Edge, Chrome, Firefox), 3 execution rounds (L1/L2/L3), 7 enterprise testing levels, and non-negotiable passing invariants.
- [`bien-ban-nghiem-thu-template.md`](enterprise-write-docs/templates/bien-ban-nghiem-thu-template.md): Formal legally-binding System Acceptance & Pilot Operations Minutes (`BBKT & VHT`) with committee sign-off, quantitative execution statistics, defect tracking, and user survey evaluation.

---

## Installation & Usage

### 1. Antigravity (AGY)
Install globally to `~/.gemini/config/skills/`:
```bash
# Developer documentation skills
cp -r write-docs ~/.gemini/config/skills/
cp -r review-docs ~/.gemini/config/skills/

# Enterprise BA & Architecture skills
cp -r enterprise-write-docs ~/.gemini/config/skills/
cp -r enterprise-review-docs ~/.gemini/config/skills/
```

### 2. Activating in Chat
- `/write-docs` - Generate developer guides, API references, architecture docs, or engineering deep-dives.
- `/review-docs <path>` - Audit developer documentation against repository source code.
- `/enterprise-write-docs` - Generate enterprise BA specs (BRD/URD/SRS/HLD/CSDL/Runbook/Acceptance).
- `/enterprise-review-docs <path>` - Audit enterprise specs against governance checklists and quantified NFRs.
