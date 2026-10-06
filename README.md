# Agent Skills (`skills`)

Agent skills for authoring, auditing, and iteratively refining technical architecture and Business Analysis (BA) documentation across software projects.

Compatible with **Antigravity (AGY)**, **Codex**, **Claude Code**, and agentic coding workflows supporting `SKILL.md` specifications.

---

## Skills Catalog

| Skill | Category | Description | Key Artifacts |
| :--- | :--- | :--- | :--- |
| [`write-docs`](write-docs/SKILL.md) | Standard Documentation | Universal authoring guide for software projects: Business Analysis (BRD, SRS, User Stories) and Technical Architecture (HLD, LLD, API specs, ADRs). | Progressive disclosure, sentence case, 1-3 sentence paragraphs, zero AI fluff. |
| [`review-docs`](review-docs/SKILL.md) | Standard Review Loop | Dual-lens evaluation loop auditing style/voice and technical accuracy against source code. Uses state tracking (`.reviews/review-<doc>.md`). | Readability, voice, code alignment (`file:line`), triage decision gate (`Improve / Complete / Done`). |
| [`enterprise-write-docs`](enterprise-write-docs/SKILL.md) | Enterprise Architecture | High-governance documentation guide modeled on empirical tier-1 banking, telecommunications, and mission-critical enterprise systems. | 4-in-1 Unified Spec (PTTKHT), 12-field Use Case Cards, 5D Business Rules (BR-xxx), Data Scope RBAC, Request Journey, Field-level crypto, DB table classification. |
| [`enterprise-review-docs`](enterprise-review-docs/SKILL.md) | Enterprise Review Loop | Strict audit and verification enforcing enterprise governance, quantified NFRs, scope boundaries, CSDL audit columns, runbook sequences, and acceptance rigor. | Scorecard out of 40, audit checklists for URD, SRS, HLD, LLD, CSDL, HDVH, and Test Matrices. |

---

## Enterprise Templates (`enterprise-write-docs/templates/`)

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
# Copy enterprise skills
cp -r enterprise-write-docs ~/.gemini/config/skills/
cp -r enterprise-review-docs ~/.gemini/config/skills/
```
Or add to a specific project repository at `.agents/skills/`.

### 2. Codex / Claude Code
Install globally to user skills directory (`~/.codex/skills/` or equivalent):
```bash
cp -r enterprise-write-docs ~/.codex/skills/
cp -r enterprise-review-docs ~/.codex/skills/
```

### 3. Activating in Chat
Reference the skill by name or trigger directly:
- `/write-docs` - Generate specifications or architectural design.
- `/review-docs <path-to-document>` - Audit and improve existing documentation.
- `/enterprise-write-docs` - Author enterprise-grade documentation with formal blueprints.
- `/enterprise-review-docs <path-to-document>` - Audit against enterprise governance and technical rigor.
