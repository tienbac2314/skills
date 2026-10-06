# [TÊN DỰ ÁN] - TÀI LIỆU YÊU CẦU NGƯỜI SỬ DỤNG & ĐẶC TẢ HỆ THỐNG (URD / SRS)

## 0. Quản lý tài liệu (Document Control)

| Thông tin | Giá trị |
| :--- | :--- |
| **Tên dự án** | [Tên dự án phần mềm] |
| **Mã hiệu dự án** | [MÃ-DỰ-ÁN, ví dụ: PRJ-CORE-SVC] |
| **Mã hiệu tài liệu** | [MÃ-DỰ-ÁN-URD / SRS] |
| **Phiên bản** | 1.0 |
| **Ngày ban hành** | YYYY-MM-DD |

### Lịch sử thay đổi (Revision History)

| Ngày thay đổi | Phiên bản cũ | Phiên bản mới | Vị trí thay đổi | Mô tả thay đổi | Tác giả |
| :--- | :--- | :--- | :--- | :--- | :--- |
| YYYY-MM-DD | - | 1.0 | Toàn bộ | Tạo mới tài liệu | [Tên tác giả] |

### Trang ký duyệt (Sign-off)

| Họ và tên | Chức vụ | Đơn vị / Bộ phận | Chữ ký | Ngày |
| :--- | :--- | :--- | :--- | :--- |
| [Họ tên 1] | Business Analyst | Trung tâm CNTT / BA Team | | |
| [Họ tên 2] | Tech Lead / Solution Architect | Architecture Team | | |
| [Họ tên 3] | Product Owner / Giám đốc Dự án | Đơn vị Nghiệp vụ | | |

---

## I. Giới thiệu (Introduction)

### 1.1 Mục đích tài liệu (Document Purpose)
Mô tả chi tiết các yêu cầu nghiệp vụ, luồng quy trình thực tế và các yêu cầu chức năng/phi chức năng của hệ thống. Tài liệu làm căn cứ để:
- Đội ngũ Kiến trúc và Kỹ thuật xây dựng HLD, LLD và mã nguồn.
- Đội ngũ QA/QC xây dựng ma trận kiểm thử (Test Matrix) và kịch bản nghiệm thu (UAT).
- Các bên liên quan (Stakeholders) ký duyệt nghiệm thu hệ thống.

### 1.2 Phạm vi tài liệu (Document Scope)
- **Áp dụng cho**: Phân hệ [Tên phân hệ / Ứng dụng].
- **Không bao gồm**: Các tính năng ngoài phạm vi được định nghĩa tại mục Out-of-Scope.

### 1.3 Định nghĩa thuật ngữ và từ viết tắt (Glossary & Acronyms)

| Thuật ngữ / Viết tắt | Định nghĩa đầy đủ | Ghi chú |
| :--- | :--- | :--- |
| URD | User Requirements Document | Tài liệu yêu cầu người sử dụng |
| SRS | Software Requirements Specification | Đặc tả yêu cầu phần mềm |
| FR | Functional Requirement | Yêu cầu chức năng |
| NFR | Non-Functional Requirement | Yêu cầu phi chức năng |
| NSD | Người sử dụng (User) | |
| QL | Quản lý | |

---

## II. Tổng quan về ứng dụng (System Overview)

### 2.1 Phát biểu bài toán (Problem Statement)
- Mô tả thực trạng hiện tại (As-Is): Quy trình thủ công, rủi ro sai sót, thiếu tập trung dữ liệu.
- Định hướng giải pháp (To-Be): Ứng dụng số hóa, đồng bộ thời gian thực, quản trị tập trung.

### 2.2 Mục tiêu kinh doanh & Dự án (Business & Project Objectives)
- **Mục tiêu kinh doanh**: Tối ưu hóa chi phí vận hành, giảm 30% thời gian xử lý thủ tục, hỗ trợ mở rộng quy mô.
- **Mục tiêu dự án**: Xây dựng phân hệ hoàn chỉnh đáp ứng [X] người dùng đồng thời, thời gian phản hồi dưới 1 giây.

### 2.3 Rủi ro, giả định và ràng buộc (Risks, Assumptions, Constraints)
- **Rủi ro**: Phụ thuộc vào tính sẵn sàng của các hệ thống tích hợp bên ngoài (IDP, Core Banking, SMS Gateway).
- **Giả định**: Hạ tầng mạng và máy chủ đáp ứng cấu hình tối thiểu được đề xuất.
- **Ràng buộc**: Phải tuân thủ quy định bảo mật dữ liệu khách hàng và tiêu chuẩn PCI-DSS/ISO 27001.

### 2.4 Phạm vi hệ thống (System Boundaries)
- **Trong phạm vi (In-Scope)**:
  - Đồng bộ danh mục và thông tin người dùng từ hệ thống quản trị trung tâm.
  - Quản lý quy trình xử lý nghiệp vụ theo phân quyền.
  - Báo cáo và dashboard thời gian thực.
- **Ngoài phạm vi (Out-of-Scope)**:
  - Thay thế hệ thống kế toán tài chính trung tâm.
  - Tích hợp cổng thanh toán quốc tế giai đoạn 1.

### 2.5 Danh mục người sử dụng (User Roles & Actors)

| STT | Nhóm người dùng (Actor) | Mô tả vai trò | Nền tảng truy cập |
| :--- | :--- | :--- | :--- |
| 1 | Super Admin | Quản trị toàn bộ ứng dụng và phân quyền hệ thống | Web Portal |
| 2 | Unit Manager (QLĐV) | Quản lý, phân công và phê duyệt trong đơn vị | Web Portal / Mobile App |
| 3 | Staff / Operator | Thực hiện tác vụ nghiệp vụ, cập nhật tiến độ | Web Portal / Mobile App |
| 4 | Guest / Auditor | Tra cứu, xem báo cáo kiểm toán theo quyền chỉ đọc | Web Portal |

### 2.6 Danh mục yêu cầu chức năng (Feature Summary Table)

| STT | Mã chức năng | Tên chức năng | Phân hệ | Người sử dụng | Nền tảng | Mức ưu tiên |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | FR-AUTH-01 | Đăng nhập tập trung qua SSO/IDP | Xác thực | Tất cả người dùng | Web / App | P0 (Bắt buộc) |
| 2 | FR-MGMT-01 | Đồng bộ cây danh mục đơn vị | Quản lý | Admin | Web | P0 (Bắt buộc) |
| 3 | FR-WORK-01 | Đăng ký và giao việc | Nghiệp vụ | QLĐV, Nhân viên | Web / App | P1 (Cao) |

---

## III. Yêu cầu chức năng chi tiết (Use Case Specifications)

### Use Case Card Chuẩn Doanh nghiệp

Mỗi chức năng phải được mô tả chi tiết bằng bảng đặc tả 12 trường dưới đây:

| Mục | Nội dung chi tiết |
| :--- | :--- |
| **Mã & Tên Use Case** | **[MÃ-UC] - [Tên chức năng ngắn gọn]** |
| **Actor(s)** | [Tên nhóm người dùng tương tác, ví dụ: Admin, QLĐV] |
| **Mức ưu tiên (Priority)** | [P0 (Critical) / P1 (High) / P2 (Medium) / P3 (Low)] |
| **Mô tả (Description)** | [Mô tả mục đích của chức năng và giá trị mang lại cho người dùng] |
| **Sự kiện kích hoạt (Trigger)**| [Hành động kích hoạt, ví dụ: Người dùng nhấn nút "Tạo mới" trên thanh công cụ] |
| **Tiền điều kiện (Pre-Conditions)** | 1. Người dùng đã đăng nhập thành công vào hệ thống.<br>2. Người dùng có quyền [Tên quyền/Vai trò].<br>3. Bản ghi tham chiếu đang ở trạng thái [ACTIVE/VALID]. |
| **Hậu điều kiện (Post-Conditions)**| 1. Bản ghi được tạo mới trong CSDL với trạng thái [PENDING/APPROVED].<br>2. Gửi thông báo thành công cho người dùng.<br>3. Ghi vết kiểm toán (Audit log). |
| **Luồng chính (Basic Flow)** | 1. Người dùng truy cập vào màn hình [Tên màn hình].<br>2. Hệ thống hiển thị form nhập liệu gồm các trường [A, B, C].<br>3. Người dùng điền thông tin hợp lệ và nhấn [Lưu/Gửi duyệt].<br>4. Hệ thống kiểm tra tính hợp lệ của dữ liệu (validation).<br>5. Hệ thống lưu dữ liệu, sinh mã định danh duy nhất và hiển thị thông báo thành công. |
| **Luồng thay thế (Alternative Flow)**| 3a. Người dùng tải lên file Excel thay vì nhập thủ công:<br>&nbsp;&nbsp;&nbsp;&nbsp;1. Người dùng nhấn nút [Import Excel].<br>&nbsp;&nbsp;&nbsp;&nbsp;2. Hệ thống kiểm tra cấu trúc file và xử lý từng dòng.<br>&nbsp;&nbsp;&nbsp;&nbsp;3. Thông báo kết quả: [X] dòng thành công, [Y] dòng lỗi kèm file báo cáo lỗi. |
| **Luồng ngoại lệ (Exception Flow)**| **EF-01**: Thiếu trường bắt buộc hoặc dữ liệu sai định dạng:<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống highlight các trường lỗi màu đỏ và hiển thị message tương ứng.<br>**EF-02**: Mất kết nối mạng / timeout hệ thống tích hợp:<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống hiển thị popup lỗi "Không thể kết nối đến máy chủ, vui lòng thử lại".<br>**EF-03**: Người dùng nhấn [Hủy bỏ]:<br>&nbsp;&nbsp;&nbsp;&nbsp;- Đóng form nhập liệu, không lưu bất kỳ thay đổi nào. |
| **Quy tắc nghiệp vụ (Business Rules)**| 1. Mã đơn vị / định danh không được trùng lặp trong cùng một tenant.<br>2. Tổng giá trị hạn mức không được vượt quá số dư khả dụng.<br>3. Trường ghi chú cho phép tối đa 500 ký tự unicode. |
| **Tiêu chí chấp nhận (Acceptance Criteria)**| **Given** người dùng có quyền Admin và chuẩn bị dữ liệu hợp lệ<br>**When** người dùng submit form tạo mới<br>**Then** hệ thống trả về mã bản ghi mới và hiển thị trong danh sách tìm kiếm. |
| **Thiết kế liên quan (Related Design)**| [Link Figma / Wireframe / Screenshot màn hình] |

---

## IV. Danh mục Quy tắc Nghiệp vụ Hệ thống (Business Rules Catalog)

Toàn bộ quy tắc cốt lõi của bài toán được chuẩn hóa thành 5 nhóm mã hiệu `BR-xxx`:

### 4.1 Nhóm quy tắc Phân quyền và Phạm vi Dữ liệu (Scope & Permissions - `BR-SCP`)
- **`BR-SCP-01`**: Mô hình phân quyền theo 4 cấp phạm vi: (1) Toàn hệ thống; (2) Cấp đơn vị quản lý; (3) Cấp đơn vị trực thuộc; (4) Người dùng cơ sở / Cá nhân.
- **`BR-SCP-02`**: Nguyên tắc thừa kế cây tổ chức: Cấp quản lý được xem và tổng hợp dữ liệu của toàn bộ đơn vị con trực thuộc; đơn vị con tuyệt đối không thể xem dữ liệu của đơn vị ngang hàng hoặc đơn vị cấp trên.
- **`BR-SCP-03`**: Kiểm soát 2 lớp độc lập: Lớp 1 chặn điều hướng trên UI; Lớp 2 bắt buộc lọc mệnh đề `unit_id IN (...)` tại câu lệnh truy vấn dữ liệu backend (Cấm tuyệt đối chỉ chặn ở giao diện).

### 4.2 Nhóm quy tắc Luồng duyệt và Phân vai (Workflow & Roles - `BR-WF`)
- **`BR-WF-01`**: Luồng chuyển trạng thái tuần tự: `DRAFT` (Dự thảo) ──► `SUBMITTED` (Đã nộp) ──► `UNIT_APPROVED` (Đơn vị duyệt) ──► `FINAL_APPROVED` (Cấp trên phê duyệt).
- **`BR-WF-02`**: Quy tắc trả lại hồ sơ: Cấp duyệt có quyền trả lại về trạng thái `REJECTED` kèm nội dung lý do bắt buộc; người lập có quyền sửa và nộp lại.
- **`BR-WF-03`**: Quy tắc thu hồi (Recall): Người lập chỉ được phép thu hồi hồ sơ khi cấp duyệt chưa xử lý (trạng thái đang `SUBMITTED`).

### 4.3 Nhóm quy tắc Tính toán và Thuật toán Nghiệp vụ (Calculations & Rollups - `BR-CALC`)
- **`BR-CALC-01`**: Thuật toán tự động tổng hợp số liệu: Cột tổng cộng của đơn vị cấp trên bằng tổng các giá trị chỉ tiêu tương ứng của các đơn vị con đã phê duyệt.
- **`BR-CALC-02`**: Bảo toàn số liệu tự nhập: Khi đơn vị cấp trên tự nhập số liệu điều chỉnh, hệ thống lưu trữ tại trường riêng và không được phép ghi đè lên số liệu nguyên bản của đơn vị cấp dưới gửi lên.

### 4.4 Nhóm quy tắc Bảo vệ Dữ liệu và Vết kiểm toán (Data Protection & Audit - `BR-SEC`)
- **`BR-SEC-01`**: Bảo vệ thông tin nhạy cảm: Số định danh cá nhân (CCCD), số điện thoại và thông tin tài chính phải được mã hóa trước khi lưu trữ trong CSDL; tra cứu sử dụng cơ chế Blind Index (HMAC-SHA256).
- **`BR-SEC-02`**: Tính bất biến của nhật ký kiểm toán: Bản ghi `app_security_audit_log` chỉ cho phép `INSERT`, tuyệt đối không hỗ trợ thao tác `UPDATE` hoặc `DELETE` ngay cả với tài khoản Quản trị cấp cao nhất.

### 4.5 Nhóm quy tắc Toàn vẹn Dữ liệu (Data Integrity - `BR-INT`)
- **`BR-INT-01`**: Khóa dữ liệu theo kỳ: Khi kỳ báo cáo hoặc kỳ kế toán chuyển trạng thái `CLOSED` (Đã đóng), toàn bộ thao tác thêm, sửa, xóa dữ liệu thuộc kỳ đó bị khóa cứng ở tầng Service.
- **`BR-INT-02`**: Tính duy nhất trong phạm vi đơn vị: Mã hồ sơ và mã nhân sự không được phép trùng lặp trong cùng một đơn vị và tenant.

---

## V. Yêu cầu phi chức năng (Non-Functional Requirements - NFR)

### 4.1 Tính tương thích (Compatibility)
- Hỗ trợ các trình duyệt phổ biến: Google Chrome (bản 90 trở lên), Microsoft Edge, Safari, Firefox.
- Độ phân giải màn hình tối thiểu trên Web: 1366 x 768 px.
- Ứng dụng di động: Hỗ trợ iOS 14.0+ và Android 10.0+.

### 4.2 Hiệu năng và độ ổn định (Performance & Reliability)
- **Thời gian phản hồi (Response Time)**: 95% yêu cầu truy vấn thông thường hoàn thành dưới 1000ms; tác vụ báo cáo phức tạp dưới 3000ms.
- **Khả năng chịu tải (Throughput & Concurrency)**: Hệ thống chịu tải tối thiểu [1000] người dùng đồng thời (CCU) tại thời điểm cao điểm.
- **Tỷ lệ truy vấn CSDL thành công**: Đạt >= 99.9%.

### 4.3 An toàn và bảo mật (Security & Compliance)
- Xác thực qua chuẩn JWT với cơ chế Refresh Token an toàn.
- Phân quyền theo mô hình RBAC (Role-Based Access Control) đến từng chức năng và phạm vi dữ liệu đơn vị.
- Mã hóa dữ liệu nhạy cảm ở trạng thái lưu trữ (AES-256) và toàn bộ kết nối truyền tải (TLS 1.3).
- Lưu vết kiểm toán (Audit log) đầy đủ cho mọi thao tác CUD (Create, Update, Delete).

### 4.4 Quản trị và sao lưu (Operations & Backup)
- Thời gian lưu trữ log truy cập tối thiểu 90 ngày.
- Cơ chế sao lưu CSDL: Backup đầy đủ hàng tuần, backup vi sai hàng ngày, backup transaction log định kỳ 15 phút.
- Chỉ tiêu phục hồi thảm họa: RPO < 15 phút, RTO < 2 giờ.

---

## VI. Tiêu chuẩn nghiệm thu hệ thống (System Acceptance Criteria)
1. 100% chức năng P0 và P1 vượt qua toàn bộ ca kiểm thử chức năng (Functional Test Cases).
2. Không còn tồn tại lỗi mức độ nghiêm trọng (Blocker/Critical/High) chưa được xử lý.
3. Hoàn thành đầy đủ bộ tài liệu bàn giao: HLD, LLD, Thiết kế CSDL, Hướng dẫn cài đặt, Hướng dẫn vận hành và Biên bản UAT có chữ ký đại diện các bên.
