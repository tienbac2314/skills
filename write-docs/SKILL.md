---
name: write-docs
description: Author clean, high-density developer and codebase documentation (Quickstarts, API references, architecture guides, code recipes, engineering deep dives, and ADRs). Use when documenting libraries, SDKs, APIs, developer tools, or application codebases.
---

# Write Documentation (Developer & Codebase Docs)

Universal guide for authoring developer-facing technical documentation, SDK guides, API references, codebase architecture, and engineering deep-dives. Modeled on high-clarity software engineering principles (tldraw docs guide).

> [!NOTE]
> **Enterprise BA vs. Developer Docs**:
> - Use **`write-docs`** (this skill) for software engineers, SDK/library users, open-source contributors, and codebase maintainers (APIs, quickstarts, architecture guides, code recipes, engineering deep-dives).
> - Use **`enterprise-write-docs`** when authoring formal Business Analysis (BRD, URD, SRS, 12-field Use Case Cards, 5D Business Rules, CSDL data dictionaries, Operations Runbooks, or legal Acceptance Minutes).

---

## Document Archetypes

Select the appropriate document archetype for developer-facing documentation:

| Document Type | Primary Audience | Core Purpose | Key Structure |
| :--- | :--- | :--- | :--- |
| **Quickstart / Getting Started** | New Developers, Evaluators | Fastest path to first successful run | Prerequisites -> Install -> Minimal working code -> Next steps |
| **Architecture / Codebase Guide** | Contributors, Maintainers | Explain system internals, patterns, and data flow | System overview -> Core abstractions -> Lifecycle/Pipeline -> Invariants |
| **API & Interface Reference** | Integrating Developers | Definitive contract of functions, types, and endpoints | Signature -> Parameter table -> Return value -> Errors -> Runnable example |
| **Code Recipe / Example Guide** | Developers implementing features | How to solve specific, real-world development tasks | Problem -> Complete runnable code -> Step-by-step breakdown -> Edge cases |
| **Engineering Deep-Dive ("Nugget")** | Engineers, Tech Blog Readers | Explain how a hard technical problem was solved | Problem frame -> The insight -> Implementation walkthrough -> Tradeoffs |
| **Architecture Decision Record (ADR)** | Engineering Team, Maintainers | Document non-trivial technical choices & tradeoffs | Context -> Decision -> Rationale & alternatives -> Consequences |
| **Migration & Release Guide** | Upgrading Developers | Safe upgrade path between versions | Breaking changes -> Deprecation schedule -> Step-by-step migration |

---

## Authoring Blueprints

### 1. Quickstart & Getting Started Guides
The goal is zero friction: get the developer from zero to a running snippet in under 3 minutes.

1. **Lead with what this is**: 1 sentence stating what the library/service does.
2. **Prerequisites**: Exact runtime versions (e.g., `Node >= 20.0`, `Go >= 1.22`, `Python >= 3.11`).
3. **Installation**: Single copyable CLI command:
   ```bash
   npm install @myorg/core
   ```
4. **Minimal Working Example**: Complete, runnable code snippet with all imports:
   ```typescript
   import { createClient } from '@myorg/core'

   const client = createClient({ apiKey: process.env.API_KEY })
   const result = await client.ping()
   console.log('Connected:', result.ok)
   ```
5. **Next Steps**: Table linking to related conceptual guides and API references.

---

### 2. Architecture & Codebase Internals Guides
Explains how the codebase works under the hood so engineers can modify or extend it safely.

1. **High-Level Overview**: Mermaid diagram of component boundaries and data flow:
   ```mermaid
   graph LR
     Input[User Action / Event] --> Dispatcher[Event Dispatcher]
     Dispatcher --> Reducer[State Store / Reducer]
     Reducer --> SideEffects[Async Workers]
     Reducer --> View[Reactive View Layer]
   ```
2. **Core Abstractions**: Short 2-3 sentence definition of each fundamental primitive, class, or domain entity.
3. **Execution Pipeline**: Numbered step-by-step trace of how a request or event travels through the system.
4. **Maintenance Invariants**: Explicit rules that developers must NEVER break when refactoring (e.g., "State updates are always synchronous; network requests never mutate state directly").

---

### 3. API & Interface Reference Tables
Definitive, dense reference for functions, classes, components, or REST/gRPC endpoints.

1. **Method / Function Signature**: Full typed signature.
2. **Parameters Table**:
   | Parameter | Type | Required | Default | Description |
   | :--- | :--- | :---: | :---: | :--- |
   | `timeoutMs` | `number` | No | `5000` | Connection timeout in milliseconds. |
   | `retry` | `boolean` | No | `true` | Automatically retry idempotent read failures. |
3. **Return Value**: Type description and meaning.
4. **Throws / Errors**: Explicit list of error classes or HTTP error codes with remediation advice.
5. **Runnable Snippet**: Self-contained example demonstrating canonical usage.

---

### 4. Technical Deep-Dives ("Nuggets" / Engineering Blog Posts)
Short, engaging technical articles explaining how a tricky, non-obvious engineering problem was solved.

#### Structure of a Technical Nugget:
1. **Frame the problem**: Establish context, tension, and stakes. Why does standard tooling/APIs fall short?
   > *Example*: "When we added dashed lines, we wanted dashes to line up perfectly on rectangle corners and arrow tips. While this seems obvious, SVG's `stroke-dasharray` does not support this. Here is how we implemented custom dash alignment."
2. **Show the insight**: Explain the core mathematical, algorithmic, or architectural breakthrough in 1-2 paragraphs ("The insight is...").
3. **Walk through the implementation**: Clean code snippets building up the solution progressively. Frame as "what we did", not prescriptive tutorials.
4. **Tradeoffs & Wrap-up**: Performance characteristics, memory footprint, remaining edge cases, and links to source files.

---

### 5. Architecture Decision Record (ADR)
Document architectural choices in imperative form:

```markdown
# ADR-001: [Decision in Imperative Form, e.g., Use SQLite in WAL Mode for Local Cache]

- **Status**: Accepted | Proposed | Deprecated | Superseded by ADR-xxx
- **Date**: YYYY-MM-DD
- **Deciders**: [Names / Roles]

## Context & Problem Statement
Technical context, requirements, constraints, and forces driving the decision.

## Decision
Assertive statement of the chosen solution.

## Rationale & Alternatives Considered
1. **Option A (Chosen)**: Pros, cons, why selected.
2. **Option B**: Pros, cons, why rejected.

## Consequences
- **Positive**: Capabilities unlocked, performance improvements.
- **Negative / Tradeoffs**: Operational complexity, limitations, technical debt.
```

---

## Writing Principles & Quality Standards

### 1. Progressive Disclosure
Move from simple to complex:
- **First**: Simplest happy-path usage.
- **Then**: Configuration options and fine-grained control.
- **Finally**: Deep internals, edge cases, custom adapters, and performance tuning.

### 2. High-Density Scannability
- **1-3 sentences per paragraph**: Dense blocks of text are hard to scan. Cut ruthlessly.
- **Tables over prose**: Use tables for methods, properties, flags, error codes, and comparisons.
- **Avoid redundant duplicate sections**: Show complete implementations once. Do not repeat trimmed-down versions of the same code under "Common Use Cases".

### 3. Clear, Active Technical Voice
- **Active voice & present tense**:
  - *Yes*: "The serializer converts the state tree into a binary buffer."
  - *No*: "The state tree will be converted into a binary buffer by the serializer."
- **Headings in sentence case**: `Event handling and bubbling`, not `Event Handling And Bubbling`.
- **Zero AI tells**: Eliminate all corporate fluff and formulaic filler:
  - *Drop*: "It is crucial to note...", "In today's fast-paced environment...", "Delve into...", "Harness the power of...", "A testament to...", "Moreover,...".

### 4. Concrete Examples & Runnable Code
- Every code example must be syntactically valid and runnable against the target framework or language.
- Include all necessary imports in the primary example.
- Use realistic variable names and domain payloads (avoid `foo`, `bar`, `test1`).

---

## Pre-Publication Verification Checklist

Before publishing developer documentation, verify:

- [ ] **First sentence defines purpose**: Immediately answers "what is this and why do I care?"
- [ ] **Code examples are runnable**: Code snippets have valid syntax, correct imports, and no broken types.
- [ ] **Progressive disclosure followed**: Basic usage precedes advanced overrides.
- [ ] **No redundant sections**: Complete examples shown once without repetitive snippets.
- [ ] **Tables used for structured data**: Parameters, options, and error codes are in tables.
- [ ] **Sentence case headings**: All headings follow standard sentence case.
- [ ] **Zero AI fluff**: Free of corporate padding, rhetorical questions, and filler transitions.
