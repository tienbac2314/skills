---
name: review-docs
description: Review, audit, and improve developer and codebase documentation (Quickstarts, API references, architecture guides, code recipes, engineering deep dives, and ADRs) for code accuracy, clarity, and voice. Iterates with tracking and verification.
---

# Review Documentation (Developer & Codebase Docs)

Universal evaluation and iterative refinement workflow for developer-facing technical documentation, SDK guides, API contracts, codebase architecture, and engineering deep-dives.

> [!NOTE]
> **Enterprise BA vs. Developer Doc Reviews**:
> - Use **`review-docs`** (this skill) to audit developer documentation, SDK articles, API references, codebase architecture guides, and technical deep-dives against source code and developer usability standards.
> - Use **`enterprise-review-docs`** when auditing formal enterprise Business Analysis (BRD, URD, SRS, 12-field Use Case Cards, 5D Business Rules, Data Scope RBAC, CSDL data dictionaries, Operations Runbooks, or Acceptance Minutes).

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
│  │ Lens A: Style & Voice   │   │ Lens B: Code & Rigor     │  │
│  │ - 1-3 sentence paras    │   │ - Source code alignment  │  │
│  │ - Progressive flow      │   │ - Runnable snippets      │  │
│  │ - Sentence case         │   │ - Type & signature check │  │
│  │ - Zero AI fluff/tells   │   │ - No duplicate sections  │  │
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
| ISS-001 | Accuracy | Section 2.1 | API parameter `timeout` in doc is actually `timeoutMs` in exported interface | `src/client.ts:42` | pending |
| ISS-002 | Code | Section 3 | Snippet imports non-existent `@myorg/utils/format`; should be `@myorg/core` | Codebase check | pending |
| ISS-003 | Redundancy | Section 4.2 | "Common Use Cases" repeats identical code already shown in Section 2 | tldraw Docs Guide | pending |
| ISS-004 | Voice | Section 1 | Contains AI fluff: "In today's fast-paced environment, it is crucial..." | Writing Guidelines | pending |
```

---

## Step 2: Dual-Lens Evaluation

Evaluate the document across two parallel lenses:

### Lens A: Style, Readability & Voice

Audit presentation, structure, and tone:

1. **Readability & Scannability (0-10)**:
   - **Paragraph length**: 1-3 sentences per paragraph maximum. No dense unbroken prose walls.
   - **Progressive disclosure**: Starts with the simplest working example before introducing complex configuration and internals.
   - **Structured tables**: API parameters, properties, flags, error codes, and options are in tables, not paragraphs.
   - **Sentence case headings**: Headings follow sentence case (`Getting started with plugins`, not `Getting Started With Plugins`).
   - **No redundant sections**: Code examples are not copy-pasted or slightly trimmed across multiple sections.

2. **Voice & Tone (0-10)**:
   - **Active voice & present tense**: Direct, assertive explanations.
   - **Zero AI tells**: Flag corporate filler words ("delve", "crucial", "harness", "testament", "seamlessly", formulaic transitions like "In conclusion", "Moreover", "It is important to remember").
   - **Framing in deep-dives**: Technical nuggets frame the problem and tension first, explain what was built, and avoid prescriptive lecturing.

---

### Lens B: Code Accuracy & Technical Rigor

Audit technical substance against the codebase:

1. **Source Code Alignment (0-10)**:
   - Check every code snippet, import statement, and API call against actual repository source code (`file:line`).
   - Verify that documented function and method signatures match exported types.
   - Verify that configuration flags, default values, and environment variables exist in the codebase.
   - Flag drift, deprecated APIs, and phantom parameters.

2. **Runnable Examples & Completeness (0-10)**:
   - Ensure the primary code example has all required imports and can run standalone.
   - Verify that payloads and variable names are realistic (no generic `foo`, `bar`, `test1`).
   - Check that error handling and failure modes are explicitly documented with realistic recovery steps.

---

## Step 3: Summarize Findings & Scorecard

Compile the evaluation findings into a structured scorecard:

```markdown
## Evaluation Report: [Document Path]

| Dimension | Score | Primary Finding |
| :--- | :---: | :--- |
| **Readability & Scannability** | X/10 | [1-3 sentence paras, tables, progressive disclosure] |
| **Voice & Tone** | X/10 | [Active voice, zero AI fluff, confident technical tone] |
| **Code Alignment** | X/10 | [Exact match with exported types and routes in source code] |
| **Snippet Completeness** | X/10 | [Runnable examples, complete imports, realistic payloads] |
| **Total** | **X/40** | Passing threshold: 32/40 (Zero P0 accuracy defects) |

### Priority Fixes

1. `[ISS-001]` **[Category]**: [Concrete issue summary with file:line reference]
2. `[ISS-002]` **[Category]**: [Concrete issue summary]
3. `[ISS-003]` **[Category]**: [Concrete issue summary]
```

### User Triage Decision

Ask the user how to proceed:
- **Improve**: Apply surgical fixes for pending issues, then re-evaluate.
- **Complete and finish**: Apply fixes directly to all pending issues and finalize (skip re-evaluation).
- **Done**: Exit without modifying the document.

---

## Step 4: Improve (Surgical Edits)

When applying improvements:
1. Target **only** issues with `pending` status in the state tracker.
2. For code accuracy: Inspect referenced source code files and update signatures, types, and imports to match actual reality.
3. For style and scannability: Reformat paragraphs > 3 sentences into structured tables or concise bullet points; remove duplicate code blocks.
4. For voice: Strip corporate AI filler words and convert headings to sentence case.
5. Update the state file, marking addressed issues as `fixed`.

---

## Step 5: Verification & Iteration

In subsequent rounds:
1. **Verify fixes**: Check the updated document against each tracker item:
   - If resolved correctly -> mark `verified-fixed`.
   - If unresolved or incomplete -> mark `not-fixed`.
2. **Scan for regressions**: Ensure fixes did not break syntax highlighting or introduce unverified code snippets.
3. **Re-calculate scores**:
   - If Total >= 32/40 and all critical issues are `verified-fixed` or `wont-fix`, mark review passed.
   - Otherwise, prompt user for next iteration.
