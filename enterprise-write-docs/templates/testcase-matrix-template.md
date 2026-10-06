# [TÊN DỰ ÁN] - MA TRẬN & KỊCH BẢN KIỂM THỬ HỆ THỐNG
*(TEST TRACEABILITY MATRIX, MULTI-BROWSER TEST SUITES & EXECUTION RUNS)*

## 0. Quản lý tài liệu (Document Control)

| Thông tin | Giá trị |
| :--- | :--- |
| **Tên dự án** | [Tên dự án phần mềm] |
| **Mã hiệu dự án** | [MÃ-DỰ-ÁN] |
| **Mã hiệu tài liệu** | [MÃ-DỰ-ÁN-KBKT] |
| **Phiên bản** | 1.0 |
| **Ngày lập** | YYYY-MM-DD |

---

## I. Môi trường và Phương pháp Kiểm thử

### 1.1 Ma trận Trình duyệt Hỗ trợ (Browser Compatibility Matrix)
Mọi ca kiểm thử tối thiểu được thực thi trên 01 trình duyệt; nhóm giao diện, tương thích và tính năng cốt lõi bắt buộc kiểm chứng trên toàn bộ 03 trình duyệt:

| STT | Trình duyệt | Phiên bản tối thiểu | Ký hiệu | Phạm vi áp dụng |
| :---: | :--- | :--- | :---: | :--- |
| 1 | Microsoft Edge | 120+ | **EDG** | Mọi nhóm kiểm thử (Mặc định) |
| 2 | Google Chrome | 120+ | **CHR** | Mọi nhóm kiểm thử (Mặc định) |
| 3 | Mozilla Firefox | 115+ | **FF** | Kiểm thử tương thích và phi chức năng |

### 1.2 Các Cấp độ và Loại hình Kiểm thử (Testing Levels)
1. **Kiểm thử Chức năng (Functional Testing)**: Từng use case theo phân hệ (`TC-AUTH`, `TC-CORE`, `TC-REP`).
2. **Kiểm thử Phân quyền & Phạm vi Dữ liệu (Role & Data Scope RBAC)**: Ma trận Vai trò $\times$ Phạm vi dữ liệu (`TC-RBAC`), bao gồm bắt buộc các ca kiểm thử tiêu cực (cố tình đổi ID đơn vị trên URL/API để kiểm chứng hệ thống chặn).
3. **Kiểm thử Luồng xuyên suốt (E2E Workflow)**: Luồng duyệt nhiều cấp, chuyển trạng thái vòng đời thực thể (`TC-WF`).
4. **Kiểm thử An toàn thông tin & Mật mã (Security & Cryptography)**: Xác nhận dữ liệu nhạy cảm được mã hóa trong DB, blind index tìm kiếm, chặn SQLi/XSS (`TC-SEC`).
5. **Kiểm thử Phi chức năng & Tương thích (NFR & Compatibility)**: Tốc độ phản hồi, giao diện thích ứng, tương thích trình duyệt (`TC-PERF`).
6. **Nghiệm thu Cài đặt sau Triển khai (Post-Installation Acceptance)**: Kiểm chứng biến môi trường, kết nối DB, giải mã dữ liệu sau cài đặt (`TC-CD`).
7. **Vận hành Thử nghiệm (Pilot Operations)**: Chu kỳ nghiệp vụ thực tế với nhân sự và số liệu thật (`VHT`).

---

## II. Ma trận Truy vết Yêu cầu (Requirements Traceability Matrix - RTM)

| STT | Phân hệ (Module) | Mã yêu cầu (SRS) | Nhóm kiểm thử | Loại kiểm thử | Mức ưu tiên | Số ca (TCs) | Trạng thái |
| :---: | :--- | :--- | :---: | :--- | :---: | :---: | :---: |
| 1 | Xác thực & Phiên | FR-AUTH-01..03 | TC-AUTH | Functional / Security | P0 | 12 | Đạt |
| 2 | Phân quyền Phạm vi | FR-RBAC-01..05 | TC-RBAC | Role & Scope / Negative | P0 | 12 | Đạt |
| 3 | Nghiệp vụ Cốt lõi | FR-CORE-01..10 | TC-CORE | Functional / Business | P0 | 24 | Đạt |
| 4 | Luồng duyệt & Trạng thái | FR-WF-01..06 | TC-WF | E2E Workflow | P0 | 18 | Đạt |
| 5 | Báo cáo & Tổng hợp | FR-REP-01..08 | TC-REP | Calculations / Formulas | P1 | 22 | Đạt |
| 6 | An toàn & Mật mã | NFR-SEC-01..12 | TC-SEC | Security / Crypto | P0 | 12 | Đạt |
| 7 | Cài đặt & Triển khai | NFR-OPS-01..04 | TC-CD | Deployment Verification | P0 | 12 | Đạt |

---

## III. Bảng Kịch bản Kiểm thử Chi tiết (Detailed Test Case Execution Table)

*Quy ước kết quả: **P** (Passed / Đạt), **F** (Failed / Không đạt), **PE** (Pending / Đang xem xét), **N** (Not Run / Chưa chạy)*

| Mã TC | Mã SRS | Tên kịch bản kiểm thử | Tiền điều kiện | Các bước thực hiện (Steps to Reproduce) | Dữ liệu kiểm thử (Test Data) | Kết quả mong muốn (Expected Results) | Ưu tiên | Trình duyệt (EDG/CHR/FF) | Lần 1 (L1) | Lần 2 (L2) | Lần 3 (L3) | Defect ID |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **TC-AUTH-01** | FR-AUTH-01 | Đăng nhập tài khoản hợp lệ | Tài khoản trạng thái ACTIVE | 1. Mở trang đăng nhập<br>2. Điền username/password<br>3. Nhấn nút Đăng nhập | Username: `user_core`<br>Pass: `Valid@2026` | Đăng nhập thành công, chuyển hướng vào Dashboard theo quyền | **P0** | EDG, CHR | **P** | - | - | - |
| **TC-RBAC-03** | FR-RBAC-04 | **[Tiêu cực]** Chặn truy cập dữ liệu đơn vị khác qua API | Đăng nhập tài khoản cấp Đơn vị B | 1. Dùng cURL/Postman gọi API chi tiết hồ sơ<br>2. Truyền `unit_id` của Đơn vị A (khác đơn vị) | Token: Cấp đơn vị B<br>Param: `unit_id=UNIT_A` | Hệ thống trả HTTP 403 Forbidden, không trả bất kỳ dữ liệu nào, ghi log cảnh báo an ninh | **P0** | CHR | **P** | - | - | - |
| **TC-CORE-02** | FR-CORE-02 | Tạo bản ghi với trường bắt buộc bị bỏ trống | Đăng nhập tài khoản nhân viên | 1. Nhấn nút "Tạo mới"<br>2. Bỏ trống tiêu đề<br>3. Nhấn "Lưu" | Title: `""` (Rỗng) | Hệ thống chặn submit, highlight ô tiêu đề màu đỏ, thông báo lỗi validation rõ ràng | **P1** | EDG, CHR, FF | **P** | - | - | - |
| **TC-WF-05** | FR-WF-03 | Duyệt hồ sơ 2 cấp: Đơn vị duyệt -> Cấp trên duyệt | Hồ sơ ở trạng thái `SUBMITTED` | 1. QL đơn vị vào duyệt -> Chuyển `UNIT_APPROVED`<br>2. Cấp trên vào kiểm tra -> Chuyển `FINAL_APPROVED` | Record ID: `REC-2026-001` | Trạng thái chuyển đổi chính xác, nhật ký workflow ghi nhận đầy đủ người duyệt, thời gian | **P0** | EDG, CHR | **F** | **P** | - | DEF-012 |
| **TC-SEC-05** | NFR-SEC-04 | Kiểm chứng mã hóa dữ liệu nhạy cảm trong CSDL | Đã tạo bản ghi hồ sơ nhân sự/khách hàng | 1. Mở công cụ truy vấn SQL Server/Postgres<br>2. Select các trường CCCD/Điện thoại | Query: `SELECT cccd_plaintext, cccd_cipher, key_version FROM app_profile` | Cột plaintext hoàn toàn rỗng/NULL; cột cipher chứa chuỗi byte mã hóa AES-256; có cột `key_version` | **P0** | DB Tool | **P** | - | - | - |
| **TC-CD-04** | NFR-OPS-02 | Giải mã dữ liệu thành công sau khi cài đặt mới | Đã triển khai bản build mới lên máy chủ | 1. Kiểm tra biến môi trường mã hóa ở cấp Machine<br>2. Khởi động ứng dụng<br>3. Đăng nhập và mở màn hình xem chi tiết | Tài khoản Quản trị viên | Màn hình hiển thị giải mã rõ ràng, đúng nội dung đã lưu, không phát sinh lỗi CryptographicException | **P0** | CHR | **P** | - | - | - |

---

## IV. Các Trường hợp Kiểm thử Bắt buộc Đạt (Non-Negotiable Invariants)

Bất kỳ trường hợp kiểm thử nào dưới đây không đạt sẽ lập tức dừng quy trình nghiệm thu:

| Mã TC | Tên kiểm thử bất biến | Tiêu chí vượt qua bắt buộc | Kết quả |
| :--- | :--- | :--- | :---: |
| **TC-SEC-01** | Kiểm chứng mã hóa CSDL | CSDL không chứa dữ liệu nhạy cảm dạng rõ; cột mã hóa và phiên bản khóa hợp lệ | **ĐẠT** |
| **TC-RBAC-NEG** | Kiểm chứng chặn vượt quyền | 100% ca kiểm thử tiêu cực (sửa ID đơn vị, đổi tham số quyền) bị chặn tại tầng API | **ĐẠT** |
| **TC-REP-ROLLUP**| Kiểm chứng khớp số liệu | Số liệu tổng hợp tự động khớp chính xác 100% với số liệu đối chiếu độc lập | **ĐẠT** |
| **TC-SEC-CLEAN** | Dọn sạch môi trường thật | Không còn tài khoản mẫu, dữ liệu test rác trên cơ sở dữ liệu production | **ĐẠT** |
| **TC-CD-BOOT** | Khởi động & giải mã sau cài đặt | Cài đặt mới hoặc khởi động lại hoàn tất không lỗi, dịch vụ mã hóa/giải mã thông suốt | **ĐẠT** |

---

## V. Phân loại Lỗi và Quy trình Quản lý Lỗi (Defect Management)

### 5.1 Bảng Phân cấp Mức độ Nghiêm trọng (Severity)

| Mức độ | Định nghĩa | Tiêu chuẩn đóng nghiệm thu |
| :--- | :--- | :--- |
| **Mức 1 (Blocker / Critical)** | Hệ thống sập, mất mát dữ liệu, lỗ hổng bảo mật nghiêm trọng, không thể tiếp tục kiểm thử | **Bắt buộc 0 lỗi** |
| **Mức 2 (Major)** | Tính năng chính không hoạt động đúng nghiệp vụ, không có giải pháp thay thế tạm thời | **Bắt buộc 0 lỗi** |
| **Mức 3 (Moderate / Minor)** | Lỗi giao diện, thông báo lỗi chưa chuẩn, có giải pháp thay thế tạm thời | Cho phép tối đa [X] lỗi với kế hoạch vá |
| **Mức 4 (Trivial / Suggestion)**| Đề xuất cải tiến trải nghiệm người dùng, căn chỉnh lề, chính tả | Xem xét ở phiên bản tiếp theo |

---

## VI. Thống kê Kết quả Thực thi Kiểm thử (Test Execution Summary)

| Phân nhóm kết quả | Số lượng Test Cases | Tỷ lệ (%) | Đánh giá nghiệm thu |
| :--- | :---: | :---: | :--- |
| **Passed (Đạt)** | [XX] | 100.0% | 100% ca kiểm thử cốt lõi đạt yêu cầu |
| **Failed (Không đạt)** | 0 | 0.0% | 0 lỗi tồn đọng mức 1 và mức 2 |
| **Pending / Blocked** | 0 | 0.0% | Không có tính năng bị chặn |
| **TỔNG CỘNG** | **[TỔNG_TC]** | **100%** | **ĐỦ ĐIỀU KIỆN KÝ BIÊN BẢN NGHIỆM THU** |
