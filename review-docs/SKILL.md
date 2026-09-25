---
name: review-docs
description: Review, audit, and improve code and BA documentation (BRD, SRS, HLD, LLD, API docs, architecture specs) against standards for technical accuracy, business logic completeness, clarity, and voice. Iterates with tracking and verification.
---

# Review Documentation

Universal evaluation and iterative refinement workflow for code documentation, architectural specs, and Business Analysis (BA) artifacts across any project.

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
│  │ Style & Readability     │   │ Content & Accuracy       │  │
│  │ - Sentence structure    │   │ - Source code alignment  │  │
│  │ - Voice & AI fluff check│   │ - Business logic & NFRs  │  │
│  └─────────────────────────┘   └──────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│  3. UPDATE STATE: Log issues with unique IDs and locations    │
└──────────────────────────────────────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│  4. SUMMARIZE & PROMPT USER                                  │
│  Present scorecard (X/40) + Priority fixes                   │
└──────────────────────────────────────────────────────────────┘
                               ↓
            ┌──────────────────┼──────────────────┐
            ↓                  ↓                  ↓
     [User: Improve]    [User: Complete]   [User: Done]
            ↓                  ↓                  ↓
┌──────────────────────┐ ┌──────────────────────┐ EXIT
│  5. IMPROVE          │ │  5b. COMPLETE & FINISH│
│  Apply fixes         │ │  Fix all & exit      │
└──────────────────────┘ └──────────────────────┘
            ↓                  ↓
┌──────────────────────┐      EXIT
│  6. VERIFY & LOOP    │
│  Check fixed items   │
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

| ID | Category | Location | Issue Description | Source / Reference | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| ISS-001 | Accuracy | Section 3.2 | Endpoint specifies `POST /orders`, but route in `src/routes/order.ts` is `POST /api/v1/orders` | `src/routes/order.ts:24` | pending |
| ISS-002 | Voice | Section 1 | Contains AI fluff: "In today's fast-paced environment, it is crucial..." | Writing Guidelines | pending |
| ISS-003 | Completeness | Section 4 | SRS lacks quantifiable NFR for p99 latency | NFR Standards | pending |
```

---

## Step 2: Dual-Lens Evaluation

Evaluate the document across two parallel lenses:

### Lens A: Style, Readability & Voice

Audit presentation, structure, and tone:

1. **Readability (0-10)**:
   - Are sentences concise and direct (1-3 sentences per paragraph)?
   - Are complex concepts introduced progressively (simple to complex)?
   - Are tables, bullet points, and callouts used for scannability?
   - Do headings follow sentence case?
2. **Voice & Tone (0-10)**:
   - Is it written in active voice and present tense?
   - Are assertions confident without unnecessary hedging ("might", "it appears that")?
   - **Zero AI tells**: Flag hollow filler ("crucial", "harness", "testament", "delve", "seamlessly", formulaic transitions).

### Lens B: Content, Accuracy & Completeness

Audit factual correctness and structural rigor:

1. **Completeness & Coverage (0-10)**:
   - **BRD / SRS**: Does it define explicit in-scope and out-of-scope boundaries? Are NFRs quantified? Are acceptance criteria testable (Given/When/Then)?
   - **HLD / LLD**: Does it specify architecture topology, database schemas, API contracts, error codes, and failure modes?
   - **API / Dev Docs**: Are all required headers, error codes, and payloads documented?
2. **Accuracy & Real-World Alignment (0-10)**:
   - **Code verification**: Cross-check documented APIs, function signatures, database models, and file paths against actual source code in the repository. Flag exact `file:line` mismatches.
   - **Business logic verification**: Check that business rules and calculation formulas are logically consistent and unambiguous.
   - **Diagram verification**: Ensure Mermaid syntax is valid and flows match described logic.

---

## Step 3: Summarize Findings

Compile the evaluation findings into a structured scorecard:

```markdown
## Evaluation Report: [Document Path]

| Dimension | Score | Primary Finding |
| :--- | :--- | :--- |
| **Readability** | X/10 | [Key readability observation] |
| **Voice & Style** | X/10 | [Tone or AI-fluff finding] |
| **Completeness** | X/10 | [Coverage / missing sections] |
| **Accuracy** | X/10 | [Code/logic consistency finding] |
| **Total** | **X/40** | Passing threshold: 32/40 |

### Priority Fixes

1. `[ISS-001]` **[Category]**: [Concrete issue summary with file:line reference]
2. `[ISS-002]` **[Category]**: [Concrete issue summary]
3. `[ISS-003]` **[Category]**: [Concrete issue summary]
```

### User Triage Decision

Ask the user how to proceed:
- **Improve**: Apply surgical fixes for pending issues, then run a verification pass.
- **Complete and finish**: Apply fixes directly to all pending issues and finalize (skip re-evaluation).
- **Done**: Exit without modifying the document.

**Triage Rule**: Mark items that request entirely new documentation out of scope as `wont-fix`. The review skill refines and verifies existing documents; document expansion belongs in a separate authoring task (`write-docs`).

---

## Step 4: Improve (Surgical Edits)

When applying improvements:
1. Target **only** issues with `pending` status in the state tracker.
2. For accuracy issues:
   - Inspect the referenced source code file or specification.
   - Apply fixes that reflect exact implementation realities.
3. For style issues:
   - Strip AI filler words and convert headings to sentence case.
   - Reformat dense paragraphs into concise tables or bullet lists.
4. Update the state file, marking addressed issues as `fixed`.

---

## Step 5: Verification & Iteration

In subsequent rounds:
1. **Verify fixes**: Inspect the updated document against the tracker:
   - If resolved correctly -> mark `verified-fixed`.
   - If unresolved or incomplete -> mark `not-fixed`.
2. **Scan for regressions**: Ensure fixes did not introduce syntax errors or broken links.
3. **Re-calculate scores**:
   - If Total >= 32/40 and all critical accuracy issues are `verified-fixed` or `wont-fix`, mark review passed.
   - Otherwise, prompt user for next iteration.

---

## Document-Specific Audit Checklists

### Business Analysis Docs (BRD / SRS)
- [ ] Requirements are uniquely identified (`FR-001`, `NFR-001`).
- [ ] Requirements use RFC 2119 precision (`MUST`, `SHOULD`, `MAY`).
- [ ] NFRs include measurable metrics (latency in ms, availability percentage), not vague adjectives like "fast" or "secure".
- [ ] Scenarios include concrete Given/When/Then acceptance criteria.
- [ ] Scope boundary clearly states what will *not* be built.

### Technical Architecture Docs (HLD / LLD)
- [ ] Component names and service boundaries match repository packages or microservices.
- [ ] Data models and column names match database migration files or ORM schemas.
- [ ] API endpoints, HTTP verbs, and status codes match actual route controllers.
- [ ] Error handling specifies concrete error codes and fallback mechanisms.
- [ ] Mermaid diagrams render without syntax errors.
