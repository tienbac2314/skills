---
name: write-docs
description: Author enterprise-grade documentation across projects, including Business Analysis (BRD, SRS, User Stories/PRD) and Technical Architecture (HLD, LLD, API specs, ADRs, and code guides). Use when creating new documentation, structuring project specifications, or updating technical and business requirements.
---

# Write Documentation

Universal guide for authoring technical engineering and business analysis (BA) documentation across any project.

## Scope & Document Classification

Select the appropriate document archetype based on target audience and project stage:

| Category | Document Type | Primary Audience | Core Purpose | Key Artifacts |
| :--- | :--- | :--- | :--- | :--- |
| **BA** | **BRD** (Business Requirements) | Executives, Product Owners, Business Stakeholders | Define business problem, ROI, scope, and high-level process flows | Scope matrix (In/Out), Business Process Flows, KPIs |
| **BA** | **SRS** (Software Requirements) | Business Analysts, Tech Leads, QA Engineers | Detailed functional requirements, system behavior, and constraints | FR/NFR matrices, Use Cases, Gherkin scenarios, Data Dictionary |
| **BA** | **PRD / User Stories** | Product Managers, Scrum Teams | Feature-level functionality and acceptance criteria | Story format (`As a...`), Acceptance Criteria (`Given/When/Then`) |
| **Tech** | **HLD** (High-Level Design) | Architects, Engineering Leads, DevOps | System boundaries, architectural topology, component responsibilities | C4 context/container diagrams, integration flows, tech stack choices |
| **Tech** | **LLD** (Low-Level Design) | Software Engineers, Code Reviewers | Concrete implementation details, class/module structures, DB schemas | Sequence diagrams, class diagrams, ER diagrams, state machines |
| **Tech** | **API Contract / SDK Docs** | Internal/External Developers, Consumers | Integration specs, payload contracts, authentication, error states | OpenAPI/JSON schemas, endpoint tables, runnable code snippets |
| **Tech** | **ADR** (Architecture Decision Record) | Engineering Team, Future Maintainers | Record non-trivial architectural decisions, tradeoffs, and rationale | Context, Decision, Consequences, Alternatives |

---

## Authoring Blueprints

### 1. Business Requirements Document (BRD)

Structure for defining business context and organizational impact:

1. **Executive summary**: Concise problem statement, proposed solution, and strategic alignment.
2. **Business objectives & KPIs**: Quantifiable targets (e.g., "Reduce checkout drop-off by 15%", "Process 10,000 requests/sec").
3. **Project scope**:
   - **In-scope**: Explicit list of deliverables and business capabilities.
   - **Out-of-scope**: Explicit boundary lines to prevent scope creep.
4. **Stakeholder & persona analysis**: User personas, operational roles, and business value per persona.
5. **Business process flows**: Current state (As-Is) vs. Target state (To-Be) using Mermaid `flowchart LR`.
6. **Business rules**: Unambiguous policy constraints (e.g., "Discount cannot exceed 30% without manager sign-off").
7. **Risks, assumptions & dependencies**: Business risks, operational dependencies, and mitigation strategies.

### 2. Software Requirements Specification (SRS)

Structure for system-level functional and non-functional specifications:

1. **System overview**: Context diagram showing system interactions with users and third-party systems.
2. **Functional requirements (FR)**:
   - Use standard tabular schema:
     ```markdown
     | Req ID | Module | Feature | Requirement Description | Priority (P0-P3) | Acceptance Test ID |
     | :--- | :--- | :--- | :--- | :--- | :--- |
     | FR-001 | Auth | MFA Login | System must enforce TOTP verification for admin accounts. | P0 | TC-AUTH-01 |
     ```
3. **Non-functional requirements (NFR)**:
   - Quantified metrics across standard pillars:
     - **Performance**: Latency (p95, p99), throughput (RPS), concurrency limits.
     - **Scalability**: Horizontal/vertical scaling triggers, data growth projections.
     - **Security & Compliance**: RBAC, encryption at rest/in transit, regulatory requirements (GDPR, HIPAA, SOC2).
     - **Availability & Resilience**: SLA/SLO (e.g., 99.95%), RTO (Recovery Time Objective), RPO (Recovery Point Objective).
4. **External interface requirements**: User interfaces, hardware interfaces, software protocols, communication standards.
5. **Data models & dictionary**: Entity definitions, data types, validation rules, field constraints.
6. **Acceptance criteria (Gherkin syntax)**:
   ```gherkin
   Scenario: User enters invalid credentials
     Given an active user exists with email "dev@example.com"
     When the user submits password "wrongpassword"
     Then the system returns HTTP 401 Unauthorized
     And records a failed login attempt in the audit log
   ```

### 3. High-Level Design (HLD)

Structure for architectural topology and system decomposition:

1. **System context & architectural topology**:
   - Mermaid diagram illustrating core services, clients, databases, message brokers, and third-party APIs.
   ```mermaid
   graph TD
     Client[Client Application] --> Gateway[API Gateway / Ingress]
     Gateway --> AuthService[Auth Service]
     Gateway --> OrderService[Order Service]
     OrderService --> DB[(PostgreSQL Main)]
     OrderService --> EventBus[(Kafka Queue)]
     EventBus --> WorkerService[Notification Worker]
   ```
2. **Tech stack rationale**: Selected languages, frameworks, databases, and message brokers with technical justifications.
3. **Component breakdown**: Responsibility boundaries for each service/module.
4. **Data flow & communication patterns**: Sync (REST, gRPC) vs. Async (event-driven, webhooks, pub/sub).
5. **Cross-cutting concerns**:
   - Authentication & authorization (JWT, OAuth2, mTLS).
   - Observability (distributed tracing, structured metrics, centralized logging).
   - Caching strategy (TTL, eviction policies, cache invalidation).
   - Resilience patterns (circuit breakers, exponential backoff, rate limiting).
6. **Deployment & infrastructure topology**: Cloud provider setup, container orchestration, multi-region/DR layout.

### 4. Low-Level Design (LLD)

Structure for code-level components, data schemas, and execution details:

1. **Component & class models**: Class structures, interfaces, contracts, design patterns used (e.g., Factory, Strategy).
2. **Sequence diagrams**: Explicit call flows showing actors, services, and failure branches:
   ```mermaid
   sequenceDiagram
     autonumber
     actor User
     participant Gateway
     participant OrderAPI
     participant PaymentService
     participant Database

     User->>Gateway: POST /orders
     Gateway->>OrderAPI: Forward request (validated)
     OrderAPI->>PaymentService: Process charge
     alt Payment Success
       PaymentService-->>OrderAPI: Charge ID: ch_123
       OrderAPI->>Database: Persist order (status: CONFIRMED)
       Database-->>OrderAPI: OK
       OrderAPI-->>User: 201 Created (orderId)
     else Payment Failure
       PaymentService-->>OrderAPI: Error: Insufficient funds
       OrderAPI-->>User: 402 Payment Required
     end
   ```
3. **API endpoint contracts**:
   - Method, URI, headers, request schema, response schema (success and error codes).
   - Real, parseable JSON examples (avoid generic `foo`/`bar`).
4. **Database schema**:
   - Table definitions, primary/foreign keys, indexes, partitioning strategy, and Mermaid `erDiagram`.
   - Migration impact and data volume considerations.
5. **State machine & lifecycle**:
   - Complete status transitions using Mermaid `stateDiagram-v2`.
6. **Error handling & edge cases**:
   - Error code enumeration, transaction isolation levels, concurrency conflicts, idempotency mechanisms.

### 5. Architecture Decision Record (ADR)

Structure for documenting key technical choices:

```markdown
# ADR-001: [Decision Title in Imperative Form, e.g., Use Kafka for Event Ingestion]

- **Status**: Accepted | Proposed | Deprecated | Superseded by ADR-xxx
- **Date**: YYYY-MM-DD
- **Deciders**: [Names / Roles]

## Context & Problem Statement
Describe the technical context, requirements, constraints, and forces driving the decision.

## Decision
State the chosen architectural solution clearly and assertively.

## Rationale & Alternatives Considered
1. **Option A (Chosen)**: Pros, cons, why selected.
2. **Option B**: Pros, cons, why rejected.

## Consequences
- **Positive**: Direct benefits and unlocked capabilities.
- **Negative / Tradeoffs**: Operational overhead, added complexity, or technical debt introduced.
```

---

## Writing Principles & Quality Standards

### 1. Progressive Disclosure
Structure content from high-level understanding to deep technical nuances:
- **Lead with the core**: 1-2 sentence definition of purpose and outcome.
- **Standard path**: Canonical, happy-path workflow or basic usage.
- **Advanced specifics**: Deep architectural details, edge cases, error states, and tuning.

### 2. High-Density Scannability
- Keep paragraphs short (1 to 3 sentences).
- Use tables for structured comparisons, API parameters, error codes, and configuration options.
- Use bullet points for feature lists and prerequisites.
- Avoid uninterrupted blocks of prose.

### 3. Clear, Active Technical Voice
- Use active voice and present tense:
  - *Yes*: "The gateway inspects incoming JWT tokens and rejects expired sessions."
  - *No*: "The incoming JWT tokens will be inspected by the gateway and expired sessions will be rejected."
- Headings must use sentence case (`Data flow and ingestion`, not `Data Flow And Ingestion`).
- Zero AI tells: eliminate filler words, hollow importance claims, and corporate padding:
  - Drop: "It is crucial to remember that...", "In today's fast-paced environment...", "Delve into...", "Harness the power of...", "A testament to...".

### 4. Concrete Examples & Runnable Code
- Snippets must be syntactically valid and runnable against the target framework or language.
- First example must be complete and self-contained; subsequent examples can be focused fragments.
- Use realistic domain entities and realistic payloads (UUIDs, timestamps, realistic emails, actual column types), never placeholder variables like `foo`, `bar`, `test1`.

---

## Pre-Publication Verification Checklist

Before publishing or finalizing documentation, verify:

- [ ] **Audience alignment**: Matches the technical depth of the target reader (business vs. dev).
- [ ] **Structural completeness**: Contains all mandatory sections for the document archetype.
- [ ] **Traceability**: Business requirements trace to functional specs, which trace to design artifacts and code symbols.
- [ ] **Diagram correctness**: Mermaid diagrams render cleanly without syntax errors or unescaped characters.
- [ ] **Code accuracy**: Code snippets, endpoints, and schemas match actual implementation code.
- [ ] **Edge cases documented**: Unhappy paths, error states, validation rules, and recovery actions are specified.
- [ ] **Sentence case & voice**: Headings use sentence case; text is free of AI filler and passive voice.
