# [TÊN DỰ ÁN] - MA TRẬN & KỊCH BẢN KIỂM THỬ HỆ THỐNG (TEST TRACEABILITY MATRIX & TEST CASES)

## 0. Quản lý tài liệu (Document Control)

| Thông tin | Giá trị |
| :--- | :--- |
| **Tên dự án** | [Tên dự án phần mềm] |
| **Mã hiệu dự án** | [MÃ-DỰ-ÁN] |
| **Mã hiệu tài liệu** | [MÃ-DỰ-ÁN-TESTPLAN] |
| **Phiên bản** | 1.0 |
| **Ngày lập** | YYYY-MM-DD |

---

## I. Ma trận Truy vết Yêu cầu Kiểm thử (Requirements Traceability Matrix - RTM)

*Bảng liên kết từ mã yêu cầu URD/SRS sang kịch bản kiểm thử:*

| STT | Phân hệ (Module) | Mã yêu cầu (URD / SRS) | Tên tính năng kiểm thử | Loại kiểm thử | Mức ưu tiên | Tổng số Test Cases |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: |
| 1 | Xác thực (Auth) | FR-AUTH-01 | Đăng nhập tài khoản & Kiểm tra phân quyền | Functional / Security | P0 | 12 |
| 2 | Thành viên (User) | FR-USER-01 | Đồng bộ danh sách người dùng từ Base Platform | Integration | P0 | 8 |
| 3 | Đơn vị (Dept) | FR-DEPT-03 | Import danh sách quản lý đơn vị bằng Excel | Functional / Boundary | P1 | 15 |
| 4 | Công việc (Task) | FR-TASK-02 | Tạo mới và giao việc cho nhân viên | Functional / E2E | P0 | 20 |
| 5 | Báo cáo (Report) | FR-REP-01 | Xuất báo cáo tiến độ công việc theo đơn vị | Performance / Functional | P2 | 6 |

---

## II. Bảng Kịch bản Kiểm thử Chi tiết (Detailed Test Case Execution Table)

### Phân hệ: [Tên Phân hệ / Module, ví dụ: Quản lý Công việc]

| Mã Test Case | Mã Yêu cầu | Tên kịch bản kiểm thử | Tiền điều kiện | Các bước thực hiện (Steps to Reproduce) | Dữ liệu kiểm thử (Test Data) | Kết quả mong muốn (Expected Results) | Mức ưu tiên | Lần test 1 | Lần test 2 | Mã lỗi (Defect ID) | Ghi chú |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **TC-TASK-001** | FR-TASK-02 | Kiểm tra tạo công việc hợp lệ | Người dùng đăng nhập quyền QLĐV | 1. Vào menu "Việc của tôi"<br>2. Nhấn nút "Tạo mới"<br>3. Điền tiêu đề, người thực hiện, hạn chót<br>4. Nhấn nút "Lưu" | Tiêu đề: "Soạn thảo HLD"<br>Assignee: `user_dev01`<br>Hạn chót: `CURRENT_DATE + 3` | Hệ thống lưu thành công, thông báo "Tạo công việc thành công", công việc hiển thị trên tab "Đã giao" | **P0** | **Passed** | - | - | Happy path |
| **TC-TASK-002** | FR-TASK-02 | Kiểm tra chặn tạo việc khi thiếu tiêu đề | Người dùng đăng nhập quyền QLĐV | 1. Vào menu "Tạo mới công việc"<br>2. Để trống trường tiêu đề<br>3. Điền các trường khác hợp lệ<br>4. Nhấn "Lưu" | Tiêu đề: `""` (Empty string)<br>Assignee: `user_dev01` | Hệ thống chặn submit, highlight ô tiêu đề màu đỏ, thông báo lỗi: "Tiêu đề không được để trống" | **P0** | **Passed** | - | - | Validation check |
| **TC-TASK-003** | FR-TASK-02 | Kiểm tra nhập hạn chót trong quá khứ | Người dùng đăng nhập quyền QLĐV | 1. Vào menu "Tạo mới"<br>2. Nhập tiêu đề hợp lệ<br>3. Chọn hạn chót là ngày hôm qua<br>4. Nhấn "Lưu" | Hạn chót: `CURRENT_DATE - 1` | Hệ thống báo lỗi: "Hạn chót hoàn thành không được nhỏ hơn ngày hiện tại" | **P1** | **Failed** | **Passed** | DEF-089 | Đã fix tại build v1.0.2 |
| **TC-TASK-004** | FR-DEPT-03 | Import file Excel chứa bản ghi lỗi | Người dùng quyền Admin | 1. Chọn menu "Import QLĐV"<br>2. Upload file có 10 dòng (8 dòng hợp lệ, 2 dòng sai mã nhân viên)<br>3. Nhấn "Tiến hành Import" | File: `import_dept_err.xlsx` | Hệ thống import thành công 8 dòng, cảnh báo lỗi 2 dòng và cho phép tải file log chứa chi tiết 2 dòng lỗi | **P1** | **Passed** | - | - | Partial import test |
| **TC-TASK-005** | FR-AUTH-01 | Kiểm tra đăng nhập với mật khẩu sai quá 5 lần | Tài khoản đang ở trạng thái ACTIVE | 1. Truy cập trang đăng nhập<br>2. Nhập username đúng<br>3. Nhập mật khẩu sai liên tiếp 5 lần | Username: `admin_system`<br>Password: `wrong_pass` | Hệ thống khóa tạm thời tài khoản trong 15 phút, hiển thị thông báo "Tài khoản bị khóa tạm thời do nhập sai quá số lần quy định" | **P0** | **Passed** | - | - | Security lockout test |

---

## III. Thống kê Kết quả Kiểm thử (Test Execution Summary)

| Trạng thái | Số lượng (Test Cases) | Tỷ lệ (%) | Đánh giá nghiệm thu |
| :--- | :---: | :---: | :--- |
| **Passed (Đạt)** | 58 | 95.1% | Đạt yêu cầu |
| **Failed (Không đạt)** | 0 | 0.0% | 100% bug đã được resolve |
| **Blocked (Bị nghẽn)** | 0 | 0.0% | Không có tính năng bị chặn |
| **Not Run (Chưa chạy)** | 3 | 4.9% | Tính năng phụ thuộc bên thứ 3 (SMS) |
| **TỔNG CỘNG** | **61** | **100%** | **HỆ THỐNG ĐỦ ĐIỀU KIỆN UAT** |
