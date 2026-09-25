---
name: enterprise-write-docs
description: Author enterprise-grade documentation across projects, including Business Analysis (BRD, URD, SRS, User Stories/PRD) and Technical Architecture (HLD, LLD, API specs, CSDL data dictionaries, Operations Runbooks, Test Matrices, and ADRs). Use when creating new documentation, structuring project specifications, or updating technical and business requirements.
---

# Write Documentation

Universal guide for authoring enterprise-grade technical engineering and Business Analysis (BA) documentation across any project, modeled on empirical banking and telecommunications industry standards (MobiFone, LPBank).

## Document Classification & Template Index

Select the appropriate document archetype and use the corresponding blueprint template in `templates/`:

| Category | Document Type | Primary Audience | Core Purpose | Blueprint Template |
| :--- | :--- | :--- | :--- | :--- |
| **BA** | **URD / BRD** | Executives, Business Stakeholders, POs | Define business problem, project objectives, actors, and high-level scope | [`templates/urd-srs-template.md`](templates/urd-srs-template.md) |
| **BA** | **SRS / Detailed Specs**| Business Analysts, Tech Leads, QA | 12-field Use Case Cards, functional rules, and quantified NFRs | [`templates/urd-srs-template.md`](templates/urd-srs-template.md) |
| **Tech** | **HLD (High-Level Design)**| Architects, Tech Leads, DevOps | Topology, clustering, component breakdown, hardware sizing, system KPIs | [`templates/hld-template.md`](templates/hld-template.md) |
| **Tech** | **LLD & API Specs** | Software Engineers, Integrators | Sequence diagrams, state machines, LPBank tabular API specs, error catalogs | [`templates/lld-api-template.md`](templates/lld-api-template.md) |
| **Tech** | **CSDL (Database Design)**| Database Admins, Backend Devs | ER diagrams, table schemas, enterprise audit columns, indexing | [`templates/csdl-db-template.md`](templates/csdl-db-template.md) |
| **Tech** | **Operations Runbook** | DevOps, SRE, SysAdmins | Dependency startup/shutdown order, daily checklist, troubleshooting matrix | [`templates/runbook-ops-template.md`](templates/runbook-ops-template.md) |
| **QA** | **Test Traceability Matrix**| QA Engineers, Test Leads, UAT | RTM linking requirements to test cases, test steps, expected results | [`templates/testcase-matrix-template.md`](templates/testcase-matrix-template.md) |
| **Tech** | **ADR (Architecture Decision)**| Engineering Team, Maintainers | Document non-trivial technical choices, context, trade-offs, and alternatives | Inline section below |

---

## Enterprise Governance Header (Standard for All Documents)

Every enterprise document must start with governance tables before the main content:

```markdown
## 0. Quản lý tài liệu (Document Control)

| Thông tin | Giá trị |
| :--- | :--- |
| **Tên dự án** | [Tên dự án phần mềm] |
| **Mã hiệu dự án** | [MÃ-DỰ-ÁN] |
| **Mã hiệu tài liệu** | [MÃ-DỰ-ÁN-LOẠI-TÀI-LIỆU] |
| **Phiên bản** | 1.0 |
| **Ngày ban hành** | YYYY-MM-DD |

### Lịch sử thay đổi (Revision History)
| Ngày thay đổi | Phiên bản cũ | Phiên bản mới | Vị trí thay đổi | Mô tả thay đổi | Tác giả |
| :--- | :--- | :--- | :--- | :--- | :--- |
| YYYY-MM-DD | - | 1.0 | Toàn bộ | Tạo mới tài liệu | [Tên tác giả] |

### Trang ký duyệt (Sign-off)
| Họ và tên | Chức vụ | Đơn vị / Bộ phận | Chữ ký | Ngày |
| :--- | :--- | :--- | :--- | :--- |
| [Họ tên 1] | Business Analyst / Author | BA / Dev Team | | |
| [Họ tên 2] | Solution Architect / Reviewer | Architecture Team | | |
| [Họ tên 3] | Product Owner / Approver | Đơn vị Nghiệp vụ | | |
```

---

## Authoring Blueprints by Document Type

### 1. Business Analysis Specifications (URD / SRS)
*Reference: [`templates/urd-srs-template.md`](templates/urd-srs-template.md)*

Each functional requirement must be documented using the **Enterprise 12-Field Use Case Card**:

| Field | Content & Standards |
| :--- | :--- |
| **Mã & Tên Use Case** | `[MÃ-UC]` - `[Tên chức năng]` (e.g. `UC-ORDER-01 - Tạo đơn hàng mới`) |
| **Actor(s)** | Specific user roles interacting with the feature (e.g. Admin, QLĐV, Nhân viên) |
| **Priority** | `P0` (Critical/Must), `P1` (High), `P2` (Medium), `P3` (Low) |
| **Description** | Concise 1-2 sentence statement of user objective and system value |
| **Trigger** | Specific user action or scheduled event triggering the flow |
| **Pre-Conditions** | Numbered prerequisite states (auth, role permissions, valid referenced entities) |
| **Post-Conditions** | State changes, persisted records, notifications triggered, audit logs written |
| **Basic Flow** | Sequential numbered interactions between Actor and System (happy path) |
| **Alternative Flow** | Valid alternative execution paths (e.g. bulk excel upload vs single manual entry) |
| **Exception Flow** | Explicit failure handlings labeled `EF-01`, `EF-02` (validation failure, network timeout, cancel) |
| **Business Rules** | Enforced business invariants (character limits, duplicate checks, transaction thresholds) |
| **Acceptance Criteria**| Concrete Gherkin scenarios (`Given / When / Then`) or testable outcome verification |
| **Related Design** | Link or reference to screen wireframe, mockup, or Figma canvas |

---

### 2. High-Level Design (HLD)
*Reference: [`templates/hld-template.md`](templates/hld-template.md)*

Mandatory architectural sections:
1. **System Topology & Context**: Mermaid `graph TD` showing Client tier -> Ingress/Gateway tier -> Business Microservices tier -> Cache/Broker tier -> Persistence DB tier.
2. **Clustering & High Availability**:
   - Gateway/Ingress: Active - Active cluster with auto load-balancing.
   - Application Services: Multi-instance stateless containers.
   - Database: Active - Standby streaming replication.
3. **Hardware & Sizing Minimums**: Sizing tables per server role (CPU cores, RAM GB, Disk type/size).
4. **Quantified KPI Metrics**:
   - **Server KPIs**: CPU < 70% (Alert at > 85%), RAM < 75%, Disk space < 80%.
   - **Service KPIs**: Latency p95 < 1000ms, DB query success rate >= 99.95%, log retention >= 180 days.
5. **Cross-Cutting Concerns**: Authentication (JWT/OAuth2), centralized logging (ELK/OpenSearch), distributed tracing, rate limiting, and circuit breaking.

---

### 3. Low-Level Design & API Specifications (LLD)
*Reference: [`templates/lld-api-template.md`](templates/lld-api-template.md)*

Mandatory design sections:
1. **Execution Flows**: Mermaid `sequenceDiagram` with `autonumber`, happy path and alternative/error branches.
2. **State Machine Lifecycle**: Mermaid `stateDiagram-v2` specifying all entity status transitions.
3. **LPBank-Style Tabular API Contracts**:
   - General info: Group, HTTP Method, Endpoint, Base URL, Purpose note.
   - Request Headers table: `Type | Param/Key | Kiểu DL | Bắt buộc (M/O) | Diễn giải | Giá trị ví dụ`.
   - Request Body/Query table: `Type | Param/Key | Kiểu DL | Bắt buộc (M/O) | Diễn giải | Giá trị ví dụ | Validation`.
   - Executable cURL snippet with realistic headers and payloads.
   - Response Headers and Response Body fields tables.
   - Concrete JSON payloads: Complete HTTP 200/201 Success payload and HTTP 400/401/403/500 Error payloads (with `status`, `code`, `message`, `errors`).

---

### 4. Database Design & Data Dictionary (PTTK CSDL)
*Reference: [`templates/csdl-db-template.md`](templates/csdl-db-template.md)*

Mandatory CSDL sections:
1. **ER Diagram**: Mermaid `erDiagram` showing cardinality (`||--o{`) and foreign keys.
2. **Table Summary**: Summary table mapping Physical Name -> Module -> Business Description -> Projected rows/year.
3. **Mandatory Enterprise Audit Columns**:
   Every business table must implement:
   - `tenant_code` (`varchar(100)`, Not Null, Indexed): Multi-tenant segregation.
   - `is_deleted` (`int8 / bool`, Not Null, Default `0`, Indexed): Soft-delete flag.
   - `created_by` (`varchar(100)`, Not Null): Username of creator.
   - `created_date` (`timestamp`, Not Null, Default `NOW()`): Record creation time.
   - `last_modified_by` (`varchar(100)`, Nullable): Username of last modifier.
   - `last_modified_date` (`timestamp`, Nullable): Last update timestamp.
   - `version` (`int8`, Not Null, Default `0`): Optimistic locking counter.
4. **Field Specifications**: Full column schema tables (`Tên trường | Kiểu dữ liệu | Size | Nullable | Khóa PK/FK | Mặc định | Ý nghĩa`).
5. **Indexing Strategy**: Explicit composite indexes for high-frequency queries and unique constraints.

---

### 5. Operations Runbook & Troubleshooting (HDVH)
*Reference: [`templates/runbook-ops-template.md`](templates/runbook-ops-template.md)*

Mandatory operational sections:
1. **Dependency-Ordered Startup Sequence**:
   `CSDL/Storage -> Cache/Broker -> Registry -> Core Services -> Gateway -> Web Frontend`.
2. **Graceful Shutdown Sequence**:
   `Traffic Ingress Drain -> Gateway -> Core Workers -> Core Services -> Registry/Cache -> CSDL`.
3. **Daily Routine Checklist**: Morning and afternoon inspection checks (healthchecks, disk thresholds, error logs, replication lag, backup validation).
4. **Standard Troubleshooting Matrix**:
   - `Mã lỗi / Triệu chứng` | `Triệu chứng nhận biết` | `Nguyên nhân gốc rễ (Root Cause)` | `Cách xử lý tức thời (Workaround)` | `Giải pháp triệt để (Permanent Fix)`.

---

### 6. Test Traceability Matrix & Test Cases
*Reference: [`templates/testcase-matrix-template.md`](templates/testcase-matrix-template.md)*

Mandatory QA sections:
1. **Requirements Traceability Matrix (RTM)**: Mapping URD/SRS Requirement ID -> Test Feature -> Test Type -> Priority -> Test Case Count.
2. **Standard Test Execution Table**:
   `Mã Test Case (TC-xxx) | Mã Yêu cầu | Tên kịch bản | Tiền điều kiện | Các bước thực hiện | Dữ liệu test | Kết quả mong muốn | Mức ưu tiên (P0-P3) | Kết quả lần 1 | Kết quả lần 2 | Mã Bug/Defect | Ghi chú`.
3. **Test Run Summary**: Total cases, Passed %, Failed %, Blocked %, UAT readiness verdict.

---

### 7. Architecture Decision Record (ADR)

Use this format when recording non-trivial architecture choices:

```markdown
# ADR-001: [Title in Imperative Form, e.g., Adopt Event-Driven Ingestion via Kafka]

- **Status**: Proposed | Accepted | Deprecated | Superseded by ADR-xxx
- **Date**: YYYY-MM-DD
- **Deciders**: [Names / Roles]

## Context & Problem Statement
Technical context, requirements, business drivers, and architectural constraints.

## Decision
Assertive statement of the chosen solution and scope.

## Rationale & Alternatives Considered
1. **Option A (Chosen)**: Pros, cons, reason for selection.
2. **Option B**: Pros, cons, reason for rejection.

## Consequences
- **Positive**: Capabilities unlocked, performance gains, scalability.
- **Negative / Trade-offs**: Added operational complexity, cost, technical debt.
```

---

## Writing Principles & Quality Standards

### 1. Progressive Disclosure
- Begin with concise 1-2 sentence definition of purpose.
- Present standard happy-path flows and default configurations.
- Progress to deep internals, edge cases, exception flows, and tuning.

### 2. High-Density Scannability
- Maximum 1 to 3 sentences per paragraph.
- Use structured tables for parameters, schemas, error codes, and comparisons.
- Avoid large uninterrupted prose blocks.

### 3. Active Voice & Professional Technical Voice
- Use active voice and present tense:
  - *Yes*: "The gateway inspects incoming JWT tokens and rejects expired sessions."
  - *No*: "The incoming JWT tokens will be inspected by the gateway and expired sessions will be rejected."
- Headings must use sentence case (`Data flow and ingestion`, not `Data Flow And Ingestion`).
- Zero AI tells: eliminate filler words, hollow importance claims, and corporate padding:
  - Drop: "It is crucial to remember that...", "In today's fast-paced environment...", "Delve into...", "Harness the power of...", "A testament to...".

### 4. Concrete Examples & Runnable Code
- Provide realistic domain entities and realistic payloads (UUIDs, timestamps, realistic emails, actual column types), never placeholder variables like `foo`, `bar`, `test1`.
- Snippets must be syntactically valid and runnable against the target framework or language.

---

## Pre-Publication Verification Checklist

Before publishing or finalizing documentation, verify:

- [ ] **Document governance present**: Document control header, version history, and sign-off table included.
- [ ] **Structural completeness**: Contains all mandatory sections for the document archetype.
- [ ] **Use cases fully specified**: URD/SRS use cases include Actor, Trigger, Pre/Post-conditions, Basic, Alternative, and Exception flows.
- [ ] **Traceability intact**: Business requirements trace to functional specs, which trace to design artifacts and test cases.
- [ ] **Diagram correctness**: Mermaid diagrams render cleanly without syntax errors or unescaped characters.
- [ ] **Enterprise database standards**: Table specs include data types, column sizes, nullability, PK/FK, and standard audit columns (`tenant_code`, `is_deleted`, `created_date`, `version`).
- [ ] **API contract rigor**: API specs include Request/Response header tables, body parameter tables, validation rules, curl commands, and concrete error payloads.
- [ ] **Operational sequences defined**: Runbooks define dependency-ordered startup/shutdown and troubleshooting matrix (`Symptom -> Root Cause -> Workaround -> Permanent Fix`).
- [ ] **Sentence case & voice**: Headings use sentence case; text is free of AI filler and passive voice.
