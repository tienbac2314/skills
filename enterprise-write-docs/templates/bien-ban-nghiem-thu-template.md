# [TÊN DỰ ÁN] - BIÊN BẢN KIỂM THỬ VÀ VẬN HÀNH THỬ NGHIỆM THU HỆ THỐNG
*(SYSTEM ACCEPTANCE & PILOT OPERATIONS MINUTES)*

## 0. Quản lý tài liệu (Document Control)

| Thông tin | Giá trị |
| :--- | :--- |
| **Tên dự án** | [Tên dự án phần mềm] |
| **Mã hiệu dự án** | [MÃ-DỰ-ÁN] |
| **Mã hiệu tài liệu** | [MÃ-DỰ-ÁN-BBKT] |
| **Phiên bản** | 1.0 |
| **Ngày lập biên bản** | YYYY-MM-DD |
| **Địa điểm thực hiện** | [Địa điểm / Phòng họp / Trụ sở đơn vị] |

---

## I. THÀNH PHẦN THAM GIA NGHIỆM THU

### 1.1 Đại diện Đơn vị Sử dụng / Nghiệp vụ (Business Stakeholders)
1. Ông/Bà: [Họ và tên] - Chức vụ: [Trưởng ban / Giám đốc Nghiệp vụ] - **Chủ trì**
2. Ông/Bà: [Họ và tên] - Chức vụ: [Chuyên viên Nghiệp vụ chính] - **Thành viên**

### 1.2 Đại diện Đơn vị Quản lý CNTT / Giám sát (IT & Architecture Oversight)
1. Ông/Bà: [Họ và tên] - Chức vụ: [Trưởng phòng CNTT / Kiến trúc sư giải pháp] - **Thành viên**
2. Ông/Bà: [Họ và tên] - Chức vụ: [Cán bộ Giám sát An toàn thông tin] - **Thành viên**

### 1.3 Đại diện Đơn vị Phát triển / Triển khai (Development & QA Team)
1. Ông/Bà: [Họ và tên] - Chức vụ: [Quản trị Dự án / PM] - **Thư ký**
2. Ông/Bà: [Họ và tên] - Chức vụ: [Trưởng nhóm Kiểm thử / QA Lead] - **Cán bộ kiểm thử**
3. Ông/Bà: [Họ và tên] - Chức vụ: [Trưởng nhóm Kỹ thuật / Tech Lead] - **Đầu mối kỹ thuật**

---

## II. CĂN CỨ VÀ TÀI LIỆU THAM CHIẾU

| STT | Tên tài liệu | Mã hiệu | Phiên bản | Nội dung đối chiếu |
| :---: | :--- | :--- | :---: | :--- |
| 1 | Hợp đồng / Quyết định giao nhiệm vụ | [SỐ-HỢP-ĐỒNG] | - | Phạm vi, tiến độ và điều khoản bàn giao |
| 2 | Phân tích Thiết kế Hệ thống (PTTKHT / SRS-HLD-LLD) | [MÃ-DỰ-ÁN-PTTK] | 1.0 | Đặc tả Use Case, Quy tắc nghiệp vụ, Thiết kế CSDL |
| 3 | Kịch bản Kiểm thử và Vận hành thử | [MÃ-DỰ-ÁN-KBKT] | 1.0 | Danh mục 100% Test Cases và kịch bản vận hành thử |
| 4 | Hướng dẫn Cài đặt, Triển khai và Vận hành | [MÃ-DỰ-ÁN-HDCD] | 1.0 | Kiểm chứng cấu hình hạ tầng và cài đặt |

---

## III. NỘI DUNG VÀ MÔI TRƯỜNG KIỂM THỬ

### 3.1 Môi trường Kiểm chứng
- **Máy chủ Ứng dụng**: [Cấu hình OS, Runtime, Web Server/Container Platform]
- **Hệ quản trị CSDL**: [PostgreSQL / Oracle / SQL Server], phiên bản: [...]
- **Trình duyệt kiểm thử**: Microsoft Edge (v120+), Google Chrome (v120+), Mozilla Firefox (v115+).
- **Múi giờ & Ngôn ngữ**: Giờ chuẩn Việt Nam (UTC+7), Tiếng Việt (UTF-8).

### 3.2 Phương pháp Kiểm thử
- Kiểm thử theo kịch bản chi tiết (Scripted Testing) trên 100% Test Cases.
- Kiểm thử ma trận phân vai và phạm vi dữ liệu (Role & Scope RBAC) với 4 tài khoản đại diện 4 cấp và phiên khách.
- Kiểm thử tiêu cực (Negative Testing): Cố tình gửi yêu cầu vượt quyền, truy cập trái phép phạm vi dữ liệu để kiểm chứng hệ thống chặn ở tầng nghiệp vụ.
- Kiểm thử hồi quy (Regression Testing) sau khi vá lỗi.

---

## IV. TỔNG HỢP KẾT QUẢ KIỂM THỬ HỆ THỐNG

### 4.1 Bảng kết quả theo phân hệ và cấp độ kiểm thử

| STT | Phân nhóm kiểm thử | Mã nhóm TC | Tham chiếu Use Case | Số lượng TC | Kết quả Lần 1 | Kết quả Lần Cuối | Ghi chú / Đánh giá |
| :---: | :--- | :---: | :--- | :---: | :---: | :---: | :--- |
| **A** | **KIỂM THỬ CHỨC NĂNG (FUNCTIONAL)** | | | **[XX]** | | | |
| 1 | Xác thực và Quản lý tài khoản | TC-AUTH | UC-01, UC-11 | 12 | Đạt | **Đạt** | Kiểm tra đăng nhập, đổi pass, timeout |
| 2 | Phân quyền theo phạm vi dữ liệu | TC-RBAC | Toàn hệ thống | 12 | Đạt | **Đạt** | Gồm 8 ca kiểm thử tiêu cực vượt quyền/vượt phạm vi |
| 3 | Trang người dùng & Giao diện công khai| TC-PORTAL | UC-01 - UC-10 | 14 | Đạt | **Đạt** | Kiểm tra phiên khách chưa đăng nhập |
| 4 | Nghiệp vụ Core / Quản lý thực thể | TC-CORE | UC-13 - UC-25 | 24 | Đạt | **Đạt** | Luồng tạo, cập nhật, tìm kiếm, kết xuất |
| 5 | Luồng duyệt xuyên suốt (E2E Workflow) | TC-WF | UC-26 - UC-35 | 18 | Đạt | **Đạt** | Luồng duyệt 2 cấp, trả lại, thu hồi, duyệt vượt cấp |
| 6 | Quản lý Hồ sơ & Dữ liệu danh mục | TC-DATA | UC-36 - UC-45 | 16 | Đạt | **Đạt** | Gồm kiểm chứng mã hóa dữ liệu cá nhân trong DB |
| 7 | Báo cáo nghiệp vụ & Tổng hợp số liệu | TC-REP | UC-46 - UC-55 | 22 | Đạt | **Đạt** | Đối chiếu công thức cộng tay với tổng hợp tự động |
| 8 | Quản trị hệ thống & Cấu hình | TC-SYS | UC-56 - UC-60 | 18 | Đạt | **Đạt** | Quản trị tài khoản, phòng ban, cấu hình tham số |
| 9 | Nhật ký kiểm toán & Giám sát | TC-AUDIT | UC-SYS-LOG | 6 | Đạt | **Đạt** | Bắt buộc ghi đủ IP, Actor, Action, Old/New value |
| **B** | **KIỂM THỬ PHI CHỨC NĂNG (NFR)** | | | **[XX]** | | | |
| 10 | An toàn thông tin & Bảo mật | TC-SEC | NFR-SEC-01..15 | 12 | Đạt | **Đạt** | SQLi, XSS, mã hóa AES-256, blind index, sạch data test |
| 11 | Hiệu năng, Tương thích, Sẵn sàng | TC-PERF | NFR-PERF, UX | 9 | Đạt | **Đạt** | Latency p95 < 1s, tương thích 3 trình duyệt |
| **C** | **NGHIỆM THU CÀI ĐẶT (DEPLOYMENT)** | | | **[XX]** | | | |
| 12 | Kiểm chứng sau triển khai thực tế | TC-CD | Hướng dẫn cài đặt | 12 | Đạt | **Đạt** | Biến môi trường khóa, kết nối DB, giải mã dữ liệu |
| | **TỔNG CỘNG** | | | **[TỔNG_TC]**| | **100% ĐẠT**| |

### 4.2 Thống kê Định lượng Kết quả Kiểm thử

| Chỉ tiêu thống kê | Số lượng | Tỷ lệ (%) | Tiêu chuẩn nghiệm thu |
| :--- | :---: | :---: | :--- |
| Số trường hợp kiểm thử Đạt (Passed) | [TỔNG_TC] | 100% | Bắt buộc 100% với P0, P1 |
| Số trường hợp kiểm thử Không đạt (Failed) | 0 | 0.0% | Bắt buộc bằng 0 |
| Số trường hợp Đang xem xét / Tạm hoãn (Pending) | 0 | 0.0% | Bắt buộc bằng 0 |
| Tổng số trường hợp kiểm thử thực hiện | **[TỔNG_TC]** | **100%** | |

### 4.3 Các Trường hợp Kiểm thử Bắt buộc Đạt (Non-Negotiable Invariants)

| Mã TC | Nội dung kiểm thử trọng yếu | Kết quả kiểm chứng | Trạng thái |
| :--- | :--- | :--- | :---: |
| **TC-SEC-01** | Dữ liệu nhạy cảm được mã hóa trong CSDL (cột plaintext rỗng, cột cipher chứa bản mã, có cột `key_version`) | Kiểm tra trực tiếp bảng CSDL bằng câu lệnh SQL | **ĐẠT** |
| **TC-RBAC-04** | Chặn đứng hành vi giả mạo `unit_id` trên API để xem dữ liệu đơn vị khác | Gọi API bằng cURL/Postman với Token hợp lệ nhưng mã đơn vị khác | **ĐẠT** |
| **TC-REP-08** | Số liệu tổng hợp tự động khớp tuyệt đối với kết quả cộng tay; số liệu đơn vị tự nhập không bị ghi đè | Đối chiếu ma trận số liệu tổng hợp với bảng tính Excel độc lập | **ĐẠT** |
| **TC-SEC-12** | Đã dọn sạch 100% tài khoản, dữ liệu thử nghiệm trên môi trường chính thức; mật khẩu mặc định đã đổi | Kiểm tra danh mục tài khoản trên production database | **ĐẠT** |
| **TC-CD-04** | Giải mã dữ liệu nhạy cảm thành công sau khi cài đặt mới / khởi động lại hệ thống | Đăng nhập tài khoản hợp lệ, mở màn hình chi tiết xem đúng dữ liệu | **ĐẠT** |
| **TC-PERF-09** | Sao lưu và phục hồi thử nghiệm thành công; dữ liệu giải mã chính xác sau khi khôi phục | Backup CSDL sang máy chủ khác, restore và chạy kiểm tra tính toàn vẹn | **ĐẠT** |

---

## V. KẾT QUẢ VẬN HÀNH THỬ NGHIỆM (PILOT OPERATIONS)

### 5.1 Thông tin Đợt Vận hành thử
- **Thời gian vận hành thử**: [Số tuần, ví dụ: 04 tuần], từ ngày [YYYY-MM-DD] đến ngày [YYYY-MM-DD].
- **Đơn vị tham gia**: [Liệt kê các phòng ban, đơn vị trực thuộc tham gia thí điểm].
- **Số tài khoản tham gia vận hành**: [Tối thiểu X tài khoản theo các vai trò nghiệp vụ thực tế].

### 5.2 Đánh giá Kịch bản Vận hành thử

| Mã kịch bản | Tên kịch bản vận hành thực tế | Thời lượng | Đơn vị tham gia | Kết quả thực tế | Tồn tại (nếu có) |
| :---: | :--- | :---: | :--- | :---: | :--- |
| **VHT-01** | Vận hành chu kỳ nghiệp vụ cốt lõi trọn vẹn (Khai báo, Nhập số liệu thực tế, Duyệt đơn vị, Duyệt cấp trên) | 03 tuần | Toàn bộ đơn vị thí điểm | **Đạt** | Không có lỗi phát sinh |
| **VHT-02** | Luồng truyền thông, thông báo và phối hợp liên phòng ban | 02 tuần | Đơn vị quản lý và cơ sở | **Đạt** | Thông báo tức thời, chính xác |
| **VHT-03** | Khai thác báo cáo, kết xuất số liệu phục vụ điều hành và giao ban | 02 tuần | Lãnh đạo và chuyên viên | **Đạt** | Tốc độ kết xuất nhanh, format chuẩn |

### 5.3 Khảo sát Ý kiến Người dùng Trực tiếp

| Tiêu chí khảo sát | Tốt | Đạt | Chưa đạt | Nhận xét chi tiết của người sử dụng |
| :--- | :---: | :---: | :---: | :--- |
| Tính tiện dụng, dễ thao tác sau khi đào tạo | ☒ | ☐ | ☐ | Giao diện rõ ràng, luồng thao tác trực quan |
| Độ đáp ứng so với quy trình nghiệp vụ thực tế | ☒ | ☐ | ☐ | Khớp 100% với biểu mẫu và thẩm quyền quy định |
| Tốc độ xử lý và độ ổn định hệ thống | ☒ | ☐ | ☐ | Hệ thống chạy mượt mà, không gặp lỗi treo hay sập kết nối |
| Hiệu quả nâng cao so với phương thức làm việc cũ | ☒ | ☐ | ☐ | Giảm thiểu thời gian tổng hợp thủ công, dữ liệu tức thời |

---

## VI. BÁO CÁO TỒN TẠI VÀ XỬ LÝ LỖI (DEFECT & ISSUE TRACKING)

1. **Tồn tại lỗi phần mềm**:
   - Số lỗi mức Nghiêm trọng (Blocker/Critical): **0**
   - Số lỗi mức Nặng (Major): **0**
   - Số lỗi mức Nhẹ/Góp ý giao diện (Minor/Trivial): **0** *(Toàn bộ các góp ý giao diện trong đợt thử nghiệm đã được điều chỉnh hoàn tất)*.
2. **Tồn tại về hạ tầng & vận hành ngoại cảnh**:
   - Đã kiểm tra cấu hình mạng nội bộ, chứng chỉ số và tài nguyên máy chủ. Chi tiết quy trình xử lý sự cố hạ tầng được quy định tại Tài liệu Hướng dẫn Vận hành ([MÃ-DỰ-ÁN-HDVH]).

---

## VII. KẾT LUẬN CỦA HỘI ĐỒNG NGHIỆM THU

Căn cứ vào kết quả kiểm thử chức năng, phi chức năng, kiểm chứng cài đặt và kết quả vận hành thử nghiệm thực tế:

- [X] **ĐẠT YÊU CẦU NGHIỆM THU**: Phần mềm đáp ứng đầy đủ các yêu cầu kỹ thuật và nghiệp vụ theo tài liệu Phân tích Thiết kế Hệ thống; đủ điều kiện bàn giao đưa vào khai thác sử dụng chính thức.
- [ ] **ĐẠT YÊU CẦU CÓ ĐIỀU KIỆN**: Các tồn tại ghi nhận tại Mục VI phải được khắc phục và kiểm chứng lại trước ngày [YYYY-MM-DD].
- [ ] **CHƯA ĐẠT YÊU CẦU**: Đề nghị tiếp tục hoàn thiện và tổ chức kiểm thử lại.

Biên bản được lập thành [04] bản có giá trị pháp lý như nhau, mỗi bên giữ [01] bản để làm căn cứ bàn giao và đưa hệ thống vào vận hành chính thức.

Biên bản hoàn thành và thông qua vào hồi ...... giờ ...... phút, ngày ...... tháng ...... năm ......, các bên tham gia cùng nhất trí ký tên dưới đây.

---

## VIII. CHỮ KÝ XÁC NHẬN CỦA CÁC BÊN

| ĐẠI DIỆN ĐƠN VỊ NGHIỆP VỤ<br>(CHỦ TRÌ NGHIỆM THU) | ĐẠI DIỆN ĐƠN VỊ CNTT<br>(GIÁM SÁT KỸ THUẬT) | ĐẠI DIỆN ĐƠN VỊ PHÁT TRIỂN<br>(QUẢN TRỊ DỰ ÁN - THƯ KÝ) |
| :---: | :---: | :---: |
| *(Ký và ghi rõ họ tên)* | *(Ký và ghi rõ họ tên)* | *(Ký và ghi rõ họ tên)* |
| <br><br><br>__________________________ | <br><br><br>__________________________ | <br><br><br>__________________________ |
| **CÁN BỘ NGHIỆP VỤ CHÍNH** | **CÁN BỘ AN TOÀN THÔNG TIN** | **TRƯỞNG NHÓM KIỂM THỬ (QA LEAD)** |
| *(Ký và ghi rõ họ tên)* | *(Ký và ghi rõ họ tên)* | *(Ký và ghi rõ họ tên)* |
| <br><br><br>__________________________ | <br><br><br>__________________________ | <br><br><br>__________________________ |
