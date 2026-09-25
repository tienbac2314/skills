# Agent Skills (`skills`)

Agent skills for authoring, auditing, and iteratively refining technical architecture and Business Analysis (BA) documentation across software projects.

Compatible with **Antigravity (AGY)**, **Codex**, **Claude Code**, and agentic coding workflows supporting `SKILL.md` specifications.

---

## Skills Catalog

| Skill | Category | Description | Key Artifacts |
| :--- | :--- | :--- | :--- |
| [`write-docs`](write-docs/SKILL.md) | Standard Documentation | Universal authoring guide for software projects: Business Analysis (BRD, SRS, User Stories) and Technical Architecture (HLD, LLD, API specs, ADRs). | Progressive disclosure, sentence case, 1-3 sentence paragraphs, zero AI fluff. |
| [`review-docs`](review-docs/SKILL.md) | Standard Review Loop | Dual-lens evaluation loop auditing style/voice and technical accuracy against source code. Uses state tracking (`.reviews/review-<doc>.md`). | Readability, voice, code alignment (`file:line`), triage decision gate (`Improve / Complete / Done`). |
| [`enterprise-write-docs`](enterprise-write-docs/SKILL.md) | Enterprise & Banking | High-governance documentation guide modeled on empirical banking and telecommunications industry standards (MobiFone, LPBank). | Document control, 12-field Use Case Cards, system KPIs, tabular API contracts, audit columns. |
| [`enterprise-review-docs`](enterprise-review-docs/SKILL.md) | Enterprise Review Loop | Strict audit and verification enforcing enterprise governance, quantified NFRs, CSDL audit columns, and operational runbook dependency ordering. | Scorecard out of 40, audit checklists for URD, SRS, HLD, LLD, CSDL, HDVH, and Test Matrices. |

---

## Enterprise Templates (`enterprise-write-docs/templates/`)

Modular markdown blueprints ready for project execution:

- [`urd-srs-template.md`](enterprise-write-docs/templates/urd-srs-template.md): URD / SRS specification featuring the 12-field Use Case Card (`Actor`, `Priority`, `Trigger`, `Pre/Post Conditions`, `Basic Flow`, `Alternative Flow`, `Exception Flow`, `Business Rules`, `Acceptance Criteria`, `Related Design`).
- [`hld-template.md`](enterprise-write-docs/templates/hld-template.md): High-Level Design specifying multi-tier topology (Ingress, Microservices, Broker, Persistence), Active-Active / Active-Standby clustering, and quantified Server/Service KPI tables.
- [`lld-api-template.md`](enterprise-write-docs/templates/lld-api-template.md): Low-Level Design and LPBank-style tabular API contract (Request/Response headers, query/body params with M/O flags, validation, curl, success 200/201 and error 400/401/403/500 JSON schemas).
- [`csdl-db-template.md`](enterprise-write-docs/templates/csdl-db-template.md): Database design and data dictionary with ER diagrams, field specifications, and mandatory enterprise audit columns (`tenant_code`, `is_deleted`, `created_by`, `created_date`, `last_modified_by`, `last_modified_date`, `version`).
- [`runbook-ops-template.md`](enterprise-write-docs/templates/runbook-ops-template.md): Operations runbook with dependency-ordered startup/shutdown sequence, daily inspection checklist, and standardized troubleshooting matrix (`Symptom -> Root Cause -> Workaround -> Permanent Fix`).
- [`testcase-matrix-template.md`](enterprise-write-docs/templates/testcase-matrix-template.md): Requirements Traceability Matrix (RTM) linking requirements to test cases, test execution table, and UAT readiness summary.

---

## Installation & Usage

### 1. Antigravity (AGY)
Install globally to `~/.gemini/config/skills/`:
```bash
# Clone or copy individual skills
cp -r write-docs ~/.gemini/config/skills/
cp -r review-docs ~/.gemini/config/skills/
```
Or add to a specific project repository at `.agents/skills/`.

### 2. Codex
Install globally to `~/.codex/skills/`:
```bash
cp -r write-docs ~/.codex/skills/
cp -r review-docs ~/.codex/skills/
```

### 3. Activating in Chat
Reference the skill by name or trigger directly:
- `/write-docs` - Generate specifications or architectural design.
- `/review-docs <path-to-document>` - Audit and improve existing documentation.
- `/enterprise-write-docs` - Generate enterprise-grade banking/telecom documentation with formal templates.
- `/enterprise-review-docs <path-to-document>` - Audit against enterprise governance and technical rigor.
