---
name: enterprise-write-docs
description: Author enterprise-grade documentation across projects, including Business Analysis (BRD, URD, SRS, User Stories/PRD) and Technical Architecture (HLD, LLD, API specs, CSDL data dictionaries, Operations Runbooks, Test Matrices, and ADRs). Use when creating new documentation, structuring project specifications, or updating technical and business requirements.
---

# Enterprise Write Documentation

Universal guide for authoring enterprise-grade technical engineering and Business Analysis (BA) documentation across any project, modeled on empirical tier-1 banking, telecommunications, and mission-critical enterprise standards.

## Document Classification & Template Index

Select the appropriate document archetype and use the corresponding blueprint template in `templates/`:

| Category | Document Type | Primary Audience | Core Purpose | Blueprint Template |
| :--- | :--- | :--- | :--- | :--- |
| **All-in-One** | **PTTKHT (Unified 4-in-1 Spec)** | Entire Stakeholder & Eng Matrix | Single source of truth unifying Requirements, Architecture, Detailed Design & DB | Section below & Modular Templates |
| **BA** | **URD / BRD** | Executives, Business Stakeholders, POs | Define business problem, project objectives, actors, and high-level scope | [`templates/urd-srs-template.md`](templates/urd-srs-template.md) |
| **BA** | **SRS / Detailed Specs**| Business Analysts, Tech Leads, QA | 12-field Use Case Cards, 5-dimension Business Rules (BR-xxx), and quantified NFRs | [`templates/urd-srs-template.md`](templates/urd-srs-template.md) |
| **Tech** | **HLD (High-Level Design)**| Architects, Tech Leads, DevOps | Topology, clustering, Data Scope RBAC, Request Journey, KPIs | [`templates/hld-template.md`](templates/hld-template.md) |
| **Tech** | **LLD & API Specs** | Software Engineers, Integrators | Sequence diagrams, state machines, enterprise tabular API specs, error catalogs | [`templates/lld-api-template.md`](templates/lld-api-template.md) |
| **Tech** | **CSDL (Database Design)**| Database Admins, Backend Devs | ER diagrams, 4-group table classification, field-level encryption, audit columns | [`templates/csdl-db-template.md`](templates/csdl-db-template.md) |
| **Tech** | **Operations Runbook** | DevOps, SRE, SysAdmins | Dependency startup/shutdown order, daily checklist, troubleshooting matrix | [`templates/runbook-ops-template.md`](templates/runbook-ops-template.md) |
| **QA** | **Test Traceability Matrix**| QA Engineers, Test Leads, UAT | RTM, multi-browser matrix (EDG/CHR/FF), 3 execution rounds (L1/L2/L3), 7 testing levels | [`templates/testcase-matrix-template.md`](templates/testcase-matrix-template.md) |
| **QA / Legal** | **Acceptance Minutes (BBKT & VHT)**| Sign-off Committee, PM, QA Lead | Legally-binding acceptance minutes, mandatory invariants, pilot operations evaluation | [`templates/bien-ban-nghiem-thu-template.md`](templates/bien-ban-nghiem-thu-template.md) |
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
*A: Tạo mới (Add), M: Sửa đổi (Modify), D: Xóa bỏ (Delete)*
| Ngày thay đổi | Vị trí thay đổi | A/M/D | Phiên bản cũ | Phiên bản mới | Mô tả thay đổi | Tác giả |
| :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| YYYY-MM-DD | Toàn bộ | A | - | 1.0 | Tạo mới tài liệu | [Tên tác giả] |

### Trang ký duyệt (Sign-off)
| Họ và tên | Chức vụ | Đơn vị / Bộ phận | Chữ ký | Ngày |
| :--- | :--- | :--- | :--- | :--- |
| [Họ tên 1] | Business Analyst / Author | BA / Dev Team | | |
| [Họ tên 2] | Solution Architect / Reviewer | Architecture Team | | |
| [Họ tên 3] | Product Owner / Approver | Đơn vị Nghiệp vụ | | |
```

---

## Authoring Blueprints by Document Type

### 1. Unified 4-in-1 Architecture Specification (PTTKHT)
Khi dự án yêu cầu một tài liệu duy nhất làm nguồn chân lý duy nhất (Single Source of Truth) thay vì phân mảnh thành 4 file rời rạc, áp dụng cấu trúc PTTKHT chuẩn hóa:
1. **Tổng quan hệ thống**: Phát biểu bài toán, mục tiêu, phạm vi trong/ngoài, ràng buộc, giả định và phụ thuộc.
2. **Phân tích yêu cầu**: Mô hình phân rã chức năng, danh mục nhóm người dùng, ma trận Use Case (`UC-01` ... `UC-n`), Đặc tả chi tiết 12 trường cho từng use case, Danh mục quy tắc nghiệp vụ (`BR-xxx` qua 5 chiều), Chỉ tiêu NFR lượng hóa (`NFR-SEC`, `NFR-PERF`, `NFR-REL`, `NFR-MNT`, `NFR-UX`, `NFR-CMP`).
3. **Thiết kế kiến trúc tổng thể (HLD)**: Kiến trúc phân tầng, cấu trúc module mã nguồn, Mô hình phân quyền theo phạm vi dữ liệu (4 phạm vi $\times$ 2 lớp), Mô hình triển khai HA/DR, Quyết định kiến trúc (ADRs), Đường đi của một yêu cầu (7 bước) & Bốn điểm bất biến cần giữ khi bảo trì.
4. **Thiết kế chi tiết (LLD)**: Quy ước dùng chung, Thiết kế thực thể và máy trạng thái từng phân hệ, Thuật toán tổng hợp số liệu, Thiết kế mã hóa dữ liệu nhạy cảm (AES-256-GCM + Blind Index), Nhật ký kiểm toán.
5. **Thiết kế cơ sở dữ liệu (CSDL)**: Sơ đồ ERD, Danh mục bảng phân loại 4 nhóm (Tùy biến, Mở rộng, Nguyên trạng, Loại bỏ), Đặc tả bảng chi tiết, Danh mục Enum, Ước lượng quy mô và ngưỡng tái cấu trúc kiến trúc.
6. **Phụ lục**: Ma trận truy vết Use Case $\leftrightarrow$ Thành phần hiện thực (Controller / Service / Table) và Danh mục quyền hệ thống.

---

### 2. Business Analysis Specifications (URD / SRS)
*Reference: [`templates/urd-srs-template.md`](templates/urd-srs-template.md)*

#### Use Case Card Chuẩn Doanh nghiệp 12 Trường
| Trường | Tiêu chuẩn nội dung |
| :--- | :--- |
| **Mã & Tên Use Case** | `[MÃ-UC]` - `[Tên chức năng]` (ví dụ: `UC-ORDER-01 - Tạo đơn hàng mới`) |
| **Actor(s)** | Vai trò người dùng tương tác (Admin, QLĐV, Nhân viên, Khách) |
| **Priority** | `P0` (Critical/Must), `P1` (High), `P2` (Medium), `P3` (Low) |
| **Description** | 1-2 câu súc tích nêu mục tiêu người dùng và giá trị hệ thống |
| **Trigger** | Sự kiện hoặc thao tác kích hoạt luồng |
| **Pre-Conditions** | Tiền điều kiện đánh số (trạng thái đăng nhập, quyền hạn, dữ liệu tham chiếu) |
| **Post-Conditions** | Trạng thái thay đổi sau khi thực hiện, bản ghi lưu trữ, sự kiện/thông báo bắn ra, log kiểm toán |
| **Basic Flow** | Các bước tương tác tuần tự giữa Actor và Hệ thống (Happy path) |
| **Alternative Flow** | Các luồng hợp lệ thay thế (nhập file Excel hàng loạt thay vì nhập từng dòng) |
| **Exception Flow** | Các nhánh xử lý lỗi cụ thể (`EF-01`, `EF-02`: dữ liệu sai, timeout, người dùng hủy) |
| **Business Rules** | Ràng buộc bất biến tham chiếu đến mã `BR-xxx` |
| **Acceptance Criteria**| Kịch bản Gherkin chuẩn xác (`Given / When / Then`) |
| **Related Design** | Liên kết màn hình wireframe / Figma canvas |

#### Danh mục Quy tắc Nghiệp vụ (Business Rules Catalog - BR-xxx)
Mọi quy tắc nghiệp vụ phải được tách bạch độc lập thành mã hiệu quy chuẩn theo 5 chiều:
1. **`BR-SCP-xxx` (Scope & Permissions)**: Phân quyền thừa kế cây tổ chức, cô lập dữ liệu giữa các đơn vị ngang hàng, kiểm soát 2 lớp (UI + API).
2. **`BR-WF-xxx` (Workflow & Roles)**: Vòng đời trạng thái (`DRAFT` ──► `SUBMITTED` ──► `APPROVED` / `REJECTED`), điều kiện thu hồi hồ sơ, thẩm quyền duyệt vượt cấp.
3. **`BR-CALC-xxx` (Calculations & Rollups)**: Công thức tính toán chỉ tiêu, quy tắc tổng hợp số liệu tự động từ cấp dưới, bảo toàn số liệu đơn vị tự nhập không bị ghi đè.
4. **`BR-SEC-xxx` (Data Protection & Audit)**: Mã hóa trường nhạy cảm, blind index phục vụ tra cứu, tính bất biến không thể sửa/xóa của log kiểm toán (`INSERT` only).
5. **`BR-INT-xxx` (Data Integrity & Validation)**: Khóa dữ liệu khi chốt kỳ, tính duy nhất của mã hồ sơ trong phạm vi tenant.

---

### 3. High-Level Design (HLD)
*Reference: [`templates/hld-template.md`](templates/hld-template.md)*

Mandatory architectural sections:
1. **System Topology & Context**: Mermaid `graph TD` thể hiện phân tầng rõ ràng: Client Tier ──► Ingress/Gateway Tier ──► Microservices Tier ──► Cache/Broker Tier ──► Persistence DB Tier.
2. **Data Scope RBAC Model**:
   - **4 Cấp phạm vi**: Toàn hệ thống (Global) $\leftrightarrow$ Cấp đơn vị quản lý (Managing Unit) $\leftrightarrow$ Cấp đơn vị trực thuộc (Subordinate Unit) $\leftrightarrow$ Người dùng cơ sở / Cá nhân (Base User / Self).
   - **2 Lớp kiểm soát**: Lớp 1 (Route & Action Guard trên UI) và Lớp 2 (Data Scope Filter tại Service/SQL query - Bắt buộc).
3. **Đường đi của một yêu cầu (7-Step Journey)**:
   Client Dispatch ──► Ingress Proxy/Gateway ──► Routing Controller ──► Scope & Permission Guard ──► Domain Service Execution ──► Data Persistence & ORM ──► Audit Log & Event Publishing.
4. **Bốn điểm bất biến cần giữ khi bảo trì (4 Maintenance Invariants)**:
   - *Invar 1*: Không bao giờ bypass Data Scope tại Repository/DAO.
   - *Invar 2*: Tuyệt đối không log Plaintext dữ liệu định danh nhạy cảm.
   - *Invar 3*: Mọi lệnh chuyển trạng thái phải kiểm tra trạng thái nguồn hợp lệ.
   - *Invar 4*: Ghi vết kiểm toán phải gắn liền giao dịch (Transactional).
5. **Clustering & High Availability Topology**: Active - Active Gateway/App instances, Active - Standby Database Replication.
6. **Quantified KPI Metrics**:
   - **Server KPIs**: CPU < 70% (Alert > 85%), RAM < 75%, Disk < 80%.
   - **Service KPIs**: Latency p95 < 1000ms, DB query success >= 99.95%, log retention >= 180 ngày.

---

### 4. Low-Level Design & API Specifications (LLD)
*Reference: [`templates/lld-api-template.md`](templates/lld-api-template.md)*

Mandatory design sections:
1. **Execution Flows**: Mermaid `sequenceDiagram` có `autonumber`, thể hiện đầy đủ happy path và exception branches.
2. **State Machine Lifecycle**: Mermaid `stateDiagram-v2` chỉ rõ toàn bộ trạng thái thực thể và điều kiện chuyển dịch.
3. **Enterprise Tabular API Contracts**:
   - Header bảng: Nhóm, Method, Endpoint, Base URL, Mục đích.
   - Request Headers: `Type | Param/Key | Kiểu DL | Bắt buộc (M/O) | Diễn giải | Giá trị ví dụ`.
   - Request Body/Query: `Type | Param/Key | Kiểu DL | Bắt buộc (M/O) | Diễn giải | Giá trị ví dụ | Validation`.
   - Đoạn mã cURL mẫu đầy đủ header xác thực và payload thực tế.
   - Response Headers và Response Body fields table.
   - Payload JSON cụ thể: HTTP 200/201 Success và HTTP 400/401/403/500 Error payloads (với `status`, `code`, `message`, `errors`).

---

### 5. Database Design & Data Dictionary (PTTK CSDL)
*Reference: [`templates/csdl-db-template.md`](templates/csdl-db-template.md)*

Mandatory CSDL sections:
1. **Phân loại Bảng Dữ liệu (4 Nhóm chuẩn)**:
   - *Bảng tùy biến của hệ thống*: Bảng mới 100% phục vụ nghiệp vụ dự án.
   - *Bảng nền tảng được mở rộng*: Bảng baseline được bổ sung thêm cột hoặc khóa ngoại.
   - *Bảng nền tảng dùng nguyên trạng*: Bảng dùng trực tiếp từ core platform.
   - *Bảng đã loại bỏ khỏi thiết kế*: Bảng bị bãi bỏ kèm lý do kỹ thuật.
2. **Thiết kế Mã hóa Dữ liệu Nhạy cảm (Field-Level Encryption & Blind Index)**:
   - Dữ liệu định danh cá nhân / tài chính bắt buộc rỗng ở cột plaintext, mã hóa AES-256-GCM tại cột `_cipher`.
   - Cột `_hash` lưu Blind Index (HMAC-SHA256 với secret salt) phục vụ tìm kiếm chính xác tuyệt đối.
   - Cột `key_version` hỗ trợ xoay vòng khóa (Key Rotation).
   - Nêu rõ hệ quả vận hành: Không hỗ trợ `LIKE %val%`, quy trình re-indexing khi đổi khóa.
3. **Bảy Trường Quản trị Chuẩn Doanh nghiệp (Enterprise Audit Columns)**:
   Mọi bảng tùy biến bắt buộc có: `tenant_code`, `is_deleted`, `created_by`, `created_date`, `last_modified_by`, `last_modified_date`, `version`.
4. **Ước lượng Quy mô & Ngưỡng Tái Cấu trúc (Capacity Sizing & Scaling Thresholds)**:
   - Dung lượng nền khi bàn giao.
   - Công thức tính tăng trưởng nhóm bảng tăng nhanh nhất.
   - Ngưỡng tới hạn cần phân vùng bảng (Partitioning) hoặc tách CSDL (Data Archiving).

---

### 6. Operations Runbook & Troubleshooting (HDVH)
*Reference: [`templates/runbook-ops-template.md`](templates/runbook-ops-template.md)*

Mandatory operational sections:
1. **Quy trình Bật Hệ thống theo Thứ tự Phụ thuộc**:
   `Storage/CSDL ──► Cache/Broker ──► Service Registry ──► Core Microservices ──► API Gateway ──► Web Frontend`.
2. **Quy trình Tắt Hệ thống An toàn (Graceful Shutdown Sequence)**:
   `Ingress Drain ──► API Gateway ──► Graceful Drain (30s) ──► Workers & Core Services ──► Registry & Cache ──► CSDL Checkpoint & Stop`.
3. **Quy trình Giám sát Hàng ngày**: Checklist kiểm tra đầu giờ sáng (08:30) và cuối chiều (16:30): tài nguyên máy chủ, log lỗi, replication lag, backup verification.
4. **Ma trận Xử lý Sự cố Chuẩn**: `Mã lỗi / Triệu chứng -> Nguyên nhân gốc rễ (Root Cause) -> Cách xử lý tức thời (Workaround) -> Giải pháp triệt để (Permanent Fix)`.

---

### 7. Acceptance Testing & Pilot Operations (KBKT & BBKT)
*References: [`templates/testcase-matrix-template.md`](templates/testcase-matrix-template.md), [`templates/bien-ban-nghiem-thu-template.md`](templates/bien-ban-nghiem-thu-template.md)*

Mandatory testing standards:
1. **Ma trận Trình duyệt Hỗ trợ**: Microsoft Edge (EDG), Google Chrome (CHR), Mozilla Firefox (FF).
2. **Theo dõi 3 Lần Kiểm thử (Execution Rounds)**: Lần 1 (L1), Lần 2 (L2), Lần 3 (L3).
3. **Bảy Cấp độ Kiểm thử Doanh nghiệp**:
   - `TC-AUTH`: Xác thực và quản lý phiên.
   - `TC-RBAC`: Phân quyền & phạm vi dữ liệu (Bắt buộc kiểm thử tiêu cực: giả mạo tham số đơn vị).
   - `TC-CORE`: Tính năng nghiệp vụ cốt lõi và kiểm thử phiên khách.
   - `TC-WF`: Luồng duyệt xuyên suốt E2E đa trạng thái.
   - `TC-SEC`: An toàn thông tin, kiểm chứng mã hóa dữ liệu trong DB, blind index, dọn sạch dữ liệu test.
   - `TC-PERF`: Hiệu năng, khả năng chịu tải, tương thích giao diện.
   - `TC-CD`: Nghiệm thu cài đặt sau triển khai (kiểm chứng biến môi trường khóa, giải mã dữ liệu).
   - `VHT`: Vận hành thử nghiệm chu kỳ thật với nhân sự nghiệp vụ thật (2-4 tuần).
4. **Các Trường hợp Bắt buộc Đạt (Non-Negotiable Invariants)**:
   - Dữ liệu nhạy cảm được mã hóa trong CSDL.
   - 100% ca kiểm thử tiêu cực chặn vượt quyền thành công tại tầng API.
   - Số liệu tổng hợp tự động khớp tuyệt đối với đối chiếu độc lập.
   - Môi trường thật sạch 100% dữ liệu thử nghiệm.
5. **Biên bản Nghiệm thu Pháp lý (Biên bản Nghiệm thu & Vận hành thử)**:
   Ký nhận đầy đủ giữa Hội đồng Nghiệm thu: Chủ trì (Nghiệp vụ), Giám sát (CNTT), Thư ký (Dự án), Cán bộ an toàn thông tin, Trưởng nhóm kiểm thử.

---

### 8. Architecture Decision Record (ADR)

Ghi nhận các quyết định kiến trúc quan trọng:

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
- Bắt đầu bằng định nghĩa mục đích cốt lõi ngắn gọn 1-2 câu.
- Trình bày luồng tiêu chuẩn (happy path) và cấu hình mặc định.
- Đi sâu vào chi tiết kỹ thuật chuyên sâu, ngoại lệ, tối ưu và xử lý lỗi.

### 2. High-Density Scannability
- Mỗi đoạn văn tối đa từ 1 đến 3 câu.
- Tận dụng tối đa bảng biểu có cấu trúc cho tham số, schema, mã lỗi và so sánh.
- Tuyệt đối tránh các khối văn xuôi dài dòng không ngắt đoạn.

### 3. Active Voice & Professional Technical Voice
- Dùng thể chủ động và thì hiện tại:
  - *Đúng*: "API Gateway giải mã JWT token và từ chối các phiên đăng nhập hết hạn."
  - *Sai*: "JWT token sẽ được kiểm tra bởi API gateway và các phiên hết hạn sẽ bị hủy bỏ."
- Tiêu đề dùng Sentence case (`Data flow and ingestion`, không dùng `Data Flow And Ingestion`).
- Loại bỏ 100% từ ngữ sáo rỗng hoặc văn phong trợ lý AI:
  - Bỏ: "Cần lưu ý rằng...", "Trong bối cảnh phát triển nhanh chóng...", "Đi sâu vào...", "Tận dụng sức mạnh của...", "Minh chứng cho...".

### 4. Concrete Examples & Runnable Code
- Dữ liệu ví dụ phải mang tính thực tế (UUID chuẩn, timestamp ISO-8601, email hợp lệ, kiểu cột thực tế), không dùng các biến vô nghĩa như `foo`, `bar`, `test1`.
- Các đoạn mã cURL, SQL, cấu hình phải đúng cú pháp và chạy được trực tiếp.

---

## Pre-Publication Verification Checklist

Trước khi hoàn tất hoặc ban hành tài liệu, đối chiếu kiểm tra:

- [ ] **Document Governance**: Bảng thông tin tài liệu, lịch sử thay đổi và trang ký duyệt đầy đủ.
- [ ] **Structural Completeness**: Đầy đủ các phần bắt buộc theo từng mẫu tài liệu.
- [ ] **Traceability**: Yêu cầu nghiệp vụ liên kết sang mã Use Case (`UC-xxx`), quy tắc nghiệp vụ (`BR-xxx`), thiết kế kỹ thuật và mã kiểm thử (`TC-xxx`).
- [ ] **Data Scope RBAC Defined**: Quy định rõ 4 cấp phạm vi và 2 lớp kiểm soát (UI + API).
- [ ] **Security & Cryptography Rigor**: Dữ liệu nhạy cảm có đặc tả mã hóa AES-256-GCM, blind index và cảnh báo hệ quả vận hành.
- [ ] **Database Standards**: Bảng được phân loại 4 nhóm; cột chuẩn doanh nghiệp (`tenant_code`, `is_deleted`, `created_date`, `version`) đầy đủ; có ước lượng quy mô và ngưỡng tải.
- [ ] **API Contract Completeness**: Có đủ bảng tham số Header, Body, cURL mẫu và đầy đủ payload JSON thành công lẫn lỗi.
- [ ] **Runbook Sequences**: Quy trình bật/tắt theo thứ tự phụ thuộc; ma trận sự cố đủ 4 cột (`Symptom -> Root Cause -> Workaround -> Permanent Fix`).
- [ ] **Testing & Sign-off Rigor**: Ma trận trình duyệt 3 loại (EDG/CHR/FF), 3 lượt chạy (L1/L2/L3), 7 cấp độ kiểm thử, ca kiểm thử bắt buộc đạt và mẫu biên bản nghiệm thu.
- [ ] **Voice & Style**: Câu văn ngắn gọn, thể chủ động, tiêu đề sentence case, sạch hoàn toàn từ ngữ sáo rỗng AI.
