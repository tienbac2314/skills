---
name: enterprise-review-docs
description: Review, audit, and improve code and BA documentation (BRD, URD, SRS, HLD, LLD, API specs, CSDL data dictionaries, Operations Runbooks, and Test Matrices) against enterprise standards for technical accuracy, business logic completeness, clarity, and voice. Iterates with tracking and verification.
---

# Review Documentation

Universal evaluation and iterative refinement workflow for code documentation, architectural specs, and Business Analysis (BA) artifacts across any project, enforcing empirical enterprise banking and telecom standards (MobiFone, LPBank).

**Target**: Target document path (`$ARGUMENTS` or specified file)

**Companion skill**: `write-docs`

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
│  │ - Zero AI fluff/tells   │   │ - API tables & Error JSON│  │
│  │                         │   │ - CSDL audit columns     │  │
│  │                         │   │ - Runbook sequences      │  │
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
| ISS-003 | Completeness | Section 4.1 | Use case `UC-TASK-01` lacks Exception Flow (EF) and explicit Trigger | 12-Field UC Standard | pending |
| ISS-004 | Enterprise DB| Section 5.0 | Table `ew_order` missing mandatory audit column `tenant_code` or `version` | CSDL Standard | pending |
| ISS-005 | Voice | Section 1 | Contains AI fluff: "In today's fast-paced environment, it is crucial..." | Writing Guidelines | pending |
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
   - Does it have a Document Control block (Project Name, Doc Code, Version, Issue Date)?
   - Does it contain a Revision History table (Date, Old Version, New Version, Description, Author)?
   - Does it contain a Sign-off table (Trang ký: Author, Reviewer, Approver)?
2. **Completeness & Coverage (0-10)**:
   - **URD / SRS**:
     - Are functional requirements specified using the **12-Field Use Case Card** (Actor, Priority, Trigger, Pre-conditions, Post-conditions, Basic Flow, Alternative Flow, Exception Flow, Business Rules, Acceptance Criteria, Related Design)?
     - Are In-Scope and Out-of-Scope boundaries explicitly defined?
     - Are NFRs quantified (Latency in ms, Availability %, Concurrency CCU, SLA/SLO)?
   - **HLD (High-Level Design)**:
     - Are system tiers cleanly delineated (Ingress/Gateway, Business Services, Cache/Broker, DB)?
     - Is the clustering topology specified (Active-Active Gateway/App, Active-Standby DB)?
     - Are Server KPIs and Service KPIs quantified in tables?
   - **LLD & API Specifications**:
     - Do API contracts follow tabular schemas (Request Headers, Query/Body Params, Response Headers, Response Body fields)?
     - Are parameters marked with Mandatory/Optional (`M`/`O`) and validation rules?
     - Are concrete cURL snippets and complete JSON payloads (Success 200/201 and Error 400/401/403/500 with error codes) provided?
   - **CSDL (Database Design)**:
     - Does each table specify: Column Name, Data Type, Size, Nullable, PK/FK, Default, Business Meaning?
     - Are mandatory enterprise audit columns present (`tenant_code`, `is_deleted`, `created_by`, `created_date`, `last_modified_by`, `last_modified_date`, `version`)?
     - Is indexing and partitioning documented for high-volume entities?
   - **Operations Runbook (HDVH)**:
     - Is startup/shutdown sequenced according to strict dependency order?
     - Does the troubleshooting matrix map: `Symptom -> Root Cause -> Workaround -> Permanent Fix`?
   - **Test Matrix (RTM)**:
     - Does RTM link requirements to test cases?
     - Do test cases specify Pre-conditions, Steps to reproduce, Test data, Expected results, and Priority?
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
| **Content Completeness** | X/10 | [12-field UC cards, NFRs, API tables, audit columns] |
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
3. For enterprise completeness: Inject missing tables (governance header, 12-field use case cards, API parameter tables, CSDL audit columns).
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

### 1. Business Analysis Docs (URD / SRS)
- [ ] Document control, version history, and sign-off table present.
- [ ] Requirements uniquely identified (`FR-001`, `NFR-001`).
- [ ] Each functional requirement detailed via 12-field Use Case Card (with Trigger, Pre/Post-conditions, Basic/Alt/Exception flows, Business Rules, Acceptance Criteria).
- [ ] Acceptance criteria written with testable precision (Gherkin `Given/When/Then`).
- [ ] Quantified NFRs (latency, CCU, availability %, security compliance).
- [ ] Scope boundary clearly states what will *not* be built.

### 2. Technical Architecture Docs (HLD / LLD)
- [ ] System topology delineated into Client, Ingress/Gateway, Microservices, Broker/Cache, and Persistence tiers.
- [ ] High-availability clustering defined (Active-Active Gateway/App, Active-Standby DB).
- [ ] Quantified Server KPIs (CPU, RAM, Disk %) and Service KPIs (latency, query success rate).
- [ ] API endpoints specified in tabular format with parameter types, mandatory flags, and validation rules.
- [ ] Concrete cURL commands and complete JSON payloads (Success 200/201 and Error 400/401/403/500).
- [ ] Component names and routes match actual repository code.

### 3. Database Design (PTTK CSDL)
- [ ] Mermaid ER diagram represents entities and relations.
- [ ] Table summary maps physical names to business descriptions.
- [ ] Every table specifies: Data Type, Size, Nullable, PK/FK, Default Value, Description.
- [ ] Mandatory enterprise audit columns present (`tenant_code`, `is_deleted`, `created_by`, `created_date`, `last_modified_by`, `last_modified_date`, `version`).
- [ ] Indexing strategy defined for high-frequency queries.

### 4. Operations Runbook & Test Matrix (HDVH & Testcase)
- [ ] Startup/shutdown procedures adhere to strict dependency order.
- [ ] Troubleshooting matrix maps `Symptom -> Root Cause -> Workaround -> Permanent Fix`.
- [ ] Requirements Traceability Matrix (RTM) maps requirements to test cases.
- [ ] Test cases specify Pre-conditions, Steps, Test Data, Expected Results, and Priority.
