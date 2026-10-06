---
name: enterprise-review-docs
description: Review, audit, and improve code and BA documentation (BRD, URD, SRS, HLD, LLD, API specs, CSDL data dictionaries, Operations Runbooks, and Test Matrices) against enterprise standards for technical accuracy, business logic completeness, clarity, and voice. Iterates with tracking and verification.
---

# Enterprise Review Documentation

Universal evaluation and iterative refinement workflow for code documentation, architectural specs, and Business Analysis (BA) artifacts across any project, enforcing empirical tier-1 enterprise banking, telecom, and mission-critical standards.

**Target**: Target document path (`$ARGUMENTS` or specified file)

**Companion skill**: `enterprise-write-docs`

---

## Workflow Overview

```
┌──────────────────────────────────────────────────────────────┐
│  1. INITIALIZE: Create review state file to track issues      │
└──────────────────────────────────────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│  2. EVALUATE (Dual-Lens Audit)                               │
│  ┌─────────────────────────┐   ┌──────────────────────────┐  │
│  │ Lens A: Style & Voice   │   │ Lens B: Content & Rigor  │  │
│  │ - 1-3 sentence paras    │   │ - Governance & Sign-off  │  │
│  │ - Sentence case         │   │ - 12-field Use Case Card │  │
│  │ - Zero AI fluff/tells   │   │ - 5D Business Rules (BR) │  │
│  │ - Active voice          │   │ - Data Scope RBAC (2-tier)│ │
│  │                         │   │ - Field Crypto & Blind Idx│ │
│  │                         │   │ - DB Table Classification│  │
│  │                         │   │ - Multi-browser & Rounds │  │
│  └─────────────────────────┘   └──────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│  3. UPDATE STATE: Log issues with unique IDs and locations    │
└──────────────────────────────────────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│  4. SUMMARIZE & PROMPT USER                                  │
│  Scorecard (X/40) + Priority fixes (Passing: >= 32/40)       │
└──────────────────────────────────────────────────────────────┘
                               ↓
            ┌──────────────────┼──────────────────┐
            ↓                  ↓                  ↓
     [User: Improve]    [User: Complete]   [User: Done]
            ↓                  ↓                  ↓
┌──────────────────────┐ ┌──────────────────────┐ EXIT
│  5. IMPROVE          │ │  5b. COMPLETE & FINISH│
│  Surgical fixes      │ │  Fix all & exit      │
└──────────────────────┘ └──────────────────────┘
            ↓                  ↓
┌──────────────────────┐      EXIT
│  6. VERIFY & LOOP    │
│  Verify fixed items  │
└──────────────────────┘
```

---

## Step 1: Initialize Review State File

Create a markdown state file in `.reviews/` (or `<scratchpad>/review-<filename>.md`) to record findings across rounds.

**Path**: `.reviews/review-<document-basename>.md`

**Format**:

```markdown
# Review Tracker: [Document Path]

## Status Summary
- **Current Score**: [Total]/40
- **Round**: 1
- **Pending Issues**: [Count]

## Issue Tracker

Status values: `pending` | `fixed` | `verified-fixed` | `not-fixed` | `wont-fix`

| ID | Category | Location | Issue Description | Standard Reference | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| ISS-001 | Governance | Header | Missing Document Control table, revision history, or sign-off page | Enterprise Standards | pending |
| ISS-002 | Accuracy | Section 3.2 | API specifies endpoint `POST /orders`, but route in code is `POST /api/v1/orders` | `src/routes/order.ts:24` | pending |
| ISS-003 | Completeness | Section 4.1 | Use case lacks Exception Flow (EF) or Business Rules reference | 12-Field UC Standard | pending |
| ISS-004 | Security | Section 5.0 | PII stored in plaintext without AES-256 encryption and Blind Index | Security Standards | pending |
| ISS-005 | Enterprise DB| Section 6.2 | Tables not classified into Custom / Extended / Baseline / Eliminated | CSDL Standard | pending |
| ISS-006 | Voice | Section 1 | Contains AI fluff: "In today's fast-paced environment, it is crucial..." | Writing Guidelines | pending |
```

---

## Step 2: Dual-Lens Evaluation

### Lens A: Style, Readability & Voice

1. **Readability (0-10)**:
   - Paragraph density: Maximum 1 to 3 sentences per paragraph.
   - Progressive disclosure: High-level overview -> Happy path -> Edge cases and internals.
   - Scannability: Extensive use of structured tables, checklists, and code snippets.
   - Heading casing: All headings must use sentence case (`Data flow and ingestion`, not `Data Flow And Ingestion`).
2. **Voice & Tone (0-10)**:
   - Active voice and present tense throughout.
   - Confident assertions without hedging ("might", "it appears that").
   - **Zero AI tells**: Flag corporate filler words ("delve", "crucial", "harness", "testament", "seamlessly", formulaic transitions like "In conclusion", "Moreover").

### Lens B: Content, Technical & Business Rigor

1. **Document Governance (Prerequisite)**:
   - Document Control block (Project Name, Doc Code, Version, Issue Date).
   - Revision History table (Date, Old Version, New Version, Description, Author).
   - Sign-off table (Trang ký: Author, Reviewer, Approver).
2. **Completeness & Coverage (0-10)**:
   - **URD / SRS & Unified Spec (PTTKHT)**:
     - Functional requirements specified using **12-Field Use Case Card** (Actor, Priority, Trigger, Pre/Post-conditions, Basic/Alt/Exception flows, Business Rules, Acceptance Criteria, Related Design).
     - **Business Rules Catalog (BR-xxx)** categorized into 5 dimensions: Scope & Permissions (`BR-SCP`), Workflow & Roles (`BR-WF`), Calculations & Rollups (`BR-CALC`), Data Protection & Audit (`BR-SEC`), Data Integrity & Constraints (`BR-INT`).
     - In-Scope and Out-of-Scope boundaries explicitly defined.
     - Quantified NFRs (`NFR-SEC`, `NFR-PERF`, `NFR-REL`, `NFR-MNT`, `NFR-UX`, `NFR-CMP`).
   - **HLD (High-Level Design)**:
     - Clear tier delineation (Client, Ingress/Gateway, Microservices, Cache/Broker, DB).
     - **Data Scope RBAC Model**: 4 account scopes (Global, Managing Unit, Subordinate Unit, Base User) $\times$ 2 layers of control (UI Route Guard + API/SQL Query Scope Filter).
     - **Request Journey & Maintenance Invariants**: 7-Step E2E journey + 4 Maintenance Invariants (no scope bypass, no plaintext PII logging, state validation before transition, transactional audit).
     - Clustering & High Availability (Active-Active Gateway/App, Active-Standby DB).
     - Quantified Server KPIs and Service KPIs.
   - **LLD & API Specifications**:
     - Tabular schema: Request Headers, Query/Body Params, Response Headers, Response Body fields.
     - Mandatory/Optional (`M`/`O`) flags and explicit validation rules.
     - Concrete cURL snippets and complete JSON payloads (Success 200/201 and Error 400/401/403/500 with error codes).
   - **CSDL (Database Design)**:
     - **Table Classification (4 Groups)**: Custom system tables, Extended platform tables, Unmodified baseline tables, Eliminated tables.
     - **Field-Level Sensitive Data Encryption**: PII plaintext columns null/removed, AES-256-GCM cipher columns, HMAC-SHA256 blind index hash columns, key versioning, operational trade-offs documented.
     - Mandatory audit columns (`tenant_code`, `is_deleted`, `created_by`, `created_date`, `last_modified_by`, `last_modified_date`, `version`).
     - Capacity growth estimation and re-architecture warning thresholds.
   - **Operations Runbook (HDVH)**:
     - Startup/shutdown procedures adhere to strict dependency order.
     - Troubleshooting matrix maps: `Symptom -> Root Cause -> Workaround -> Permanent Fix`.
   - **Acceptance Testing & Sign-off (KBKT & BBKT)**:
     - Multi-browser matrix (EDG, CHR, FF) and 3 execution rounds (L1/L2/L3).
     - 7 testing levels with negative testcases for scope boundaries.
     - Non-negotiable mandatory passing invariants specified.
     - Formal legal acceptance minutes template with committee roles and pilot operational evaluation.
3. **Accuracy & Real-World Alignment (0-10)**:
   - **Code alignment**: When repository code is present, verify function names, API endpoints, schema types, and file paths against actual source lines (`file:line`). Flag discrepancies.
   - **Business logic soundness**: Check that calculation formulas, status transitions, and validation rules have no logical deadlocks.
   - **Diagram correctness**: Validate Mermaid syntax and verify that flows match described logic.

---

## Step 3: Summarize Findings & Scorecard

Compile the evaluation findings into a structured scorecard:

```markdown
## Evaluation Report: [Document Path]

| Dimension | Score | Primary Finding |
| :--- | :---: | :--- |
| **Readability & Voice** | X/10 | [Scannability, sentence case, zero AI tells] |
| **Governance & Structure** | X/10 | [Document control, sign-off, mandatory sections] |
| **Content Completeness** | X/10 | [12-field UC, BR-xxx 5D, Data Scope RBAC, Crypto, DB classification] |
| **Technical Accuracy** | X/10 | [Code/schema consistency, diagram correctness] |
| **Total** | **X/40** | Passing threshold: 32/40 (Zero P0 accuracy defects) |

### Priority Fixes

1. `[ISS-001]` **[Category]**: [Concrete issue summary with file:line or standard reference]
2. `[ISS-002]` **[Category]**: [Concrete issue summary]
3. `[ISS-003]` **[Category]**: [Concrete issue summary]
```

### User Triage Decision

Ask the user how to proceed:
- **Improve**: Apply surgical fixes for pending issues, then re-evaluate.
- **Complete and finish**: Apply fixes directly to all pending issues and finalize (skip re-evaluation).
- **Done**: Exit without modifying the document.

**Triage Rule**: Mark requests for entirely new feature documentation out-of-scope as `wont-fix`. Review polishes and verifies existing documents; document expansion belongs in a separate `write-docs` task.

---

## Step 4: Improve (Surgical Edits)

When applying improvements:
1. Target **only** issues with `pending` status in the state tracker.
2. For accuracy issues: Inspect referenced source code or specs and match exact implementation realities.
3. For enterprise completeness: Inject missing tables (governance header, 12-field use case cards, BR-xxx rules, Data Scope matrix, API parameter tables, CSDL classification, field encryption).
4. For style issues: Strip AI filler words, enforce sentence case headings, and break down paragraphs > 3 sentences into tables or bullets.
5. Update state tracker, marking addressed issues as `fixed`.

---

## Step 5: Verification & Iteration

In subsequent rounds:
1. **Verify fixes**: Check the updated document against each tracker item:
   - If resolved correctly -> mark `verified-fixed`.
   - If unresolved or incomplete -> mark `not-fixed`.
2. **Scan for regressions**: Ensure fixes did not break Mermaid diagrams or invalidate schemas.
3. **Re-calculate scores**:
   - If Total >= 32/40 and all critical issues are `verified-fixed` or `wont-fix`, mark review passed.
   - Otherwise, prompt user for next iteration.

---

## Document-Specific Audit Checklists

### 1. Business Analysis Docs (URD / SRS / PTTKHT)
- [ ] Document control, version history, and sign-off table present.
- [ ] Requirements uniquely identified (`FR-001`, `NFR-001`).
- [ ] Each functional requirement detailed via 12-field Use Case Card (with Trigger, Pre/Post-conditions, Basic/Alt/Exception flows, Business Rules, Acceptance Criteria).
- [ ] Business Rules Catalog (`BR-xxx`) cataloged across 5 dimensions: Scope, Workflow, Calculations, Security, Integrity.
- [ ] Acceptance criteria written with testable precision (Gherkin `Given/When/Then`).
- [ ] Quantified NFRs (latency, CCU, availability %, security compliance).
- [ ] Scope boundary clearly states what will *not* be built.

### 2. Technical Architecture Docs (HLD / LLD)
- [ ] System topology delineated into Client, Ingress/Gateway, Microservices, Broker/Cache, and Persistence tiers.
- [ ] Data Scope RBAC Model defines 4 account scopes $\times$ 2 enforcement layers (UI + API data scope filter).
- [ ] 7-Step Request Journey & 4 Maintenance Invariants explicitly documented.
- [ ] High-availability clustering defined (Active-Active Gateway/App, Active-Standby DB).
- [ ] Quantified Server KPIs (CPU, RAM, Disk %) and Service KPIs (latency, query success rate).
- [ ] API endpoints specified in tabular format with parameter types, mandatory flags, and validation rules.
- [ ] Concrete cURL commands and complete JSON payloads (Success 200/201 and Error 400/401/403/500).
- [ ] Component names and routes match actual repository code.

### 3. Database Design (PTTK CSDL)
- [ ] Mermaid ER diagram represents entities and relations.
- [ ] Table classification partitioned into: Custom system tables, Extended platform tables, Unmodified baseline tables, and Eliminated tables.
- [ ] Field-level encryption documented for sensitive PII (AES-256-GCM cipher, HMAC-SHA256 blind index, key version, operational search trade-offs).
- [ ] Every table specifies: Data Type, Size, Nullable, PK/FK, Default Value, Description.
- [ ] Mandatory enterprise audit columns present (`tenant_code`, `is_deleted`, `created_by`, `created_date`, `last_modified_by`, `last_modified_date`, `version`).
- [ ] Indexing strategy defined for high-frequency queries.
- [ ] Capacity growth sizing formula and re-architecture thresholds specified.

### 4. Operations Runbook & Acceptance Testing (HDVH & KBKT / BBKT)
- [ ] Startup/shutdown procedures adhere to strict dependency order.
- [ ] Troubleshooting matrix maps `Symptom -> Root Cause -> Workaround -> Permanent Fix`.
- [ ] Requirements Traceability Matrix (RTM) maps requirements to test cases.
- [ ] Multi-browser matrix (EDG, CHR, FF) and 3 execution rounds (L1, L2, L3) tracked.
- [ ] 7 testing levels covered, including negative authorization/scope bypass testcases.
- [ ] Non-negotiable mandatory passing invariants specified.
- [ ] Formal Acceptance & Pilot Operations Minutes template included for sign-off.
