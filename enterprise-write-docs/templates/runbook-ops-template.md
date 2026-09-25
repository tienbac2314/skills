# [TÊN DỰ ÁN] - TÀI LIỆU HƯỚNG DẪN VẬN HÀNH & XỬ LÝ SỰ CỐ (OPERATIONS RUNBOOK)

## 0. Quản lý tài liệu (Document Control)

| Thông tin | Giá trị |
| :--- | :--- |
| **Tên dự án** | [Tên dự án phần mềm] |
| **Mã hiệu dự án** | [MÃ-DỰ-ÁN] |
| **Mã hiệu tài liệu** | [MÃ-DỰ-ÁN-RUNBOOK] |
| **Đơn vị vận hành** | Trung tâm Quản trị Vận hành Hệ thống / DevOps Team |
| **Phiên bản** | 1.0 |
| **Ngày ban hành** | YYYY-MM-DD |

---

## I. Giới thiệu và Chỉ tiêu Giám sát (Operations Overview & KPIs)

### 1.1 Mục đích quy trình
Hướng dẫn kỹ sư vận hành kiểm tra, theo dõi hoạt động hàng ngày, thực hiện quy trình bật/tắt an toàn, sao lưu dữ liệu và xử lý nhanh chóng các sự cố phát sinh nhằm đảm bảo tính liên tục của dịch vụ 24/7.

### 1.2 Bảng chỉ tiêu giám sát hệ thống (KPI Thresholds)

| Tài nguyên / Dịch vụ | Chỉ số giám sát | Ngưỡng bình thường | Ngưỡng Cảnh báo (Warning) | Ngưỡng Nguy cấp (Critical) |
| :--- | :--- | :---: | :---: | :---: |
| **Máy chủ Ứng dụng** | CPU Utilization | < 60% | 70% - 85% | > 85% kéo dài > 5 phút |
| | RAM Utilization | < 70% | 75% - 85% | > 85% |
| | Dung lượng đĩa (Disk) | < 75% | 80% - 89% | >= 90% |
| **Cơ sở dữ liệu** | Connection Pool Active | < 60% max pool | 70% - 85% | > 85% |
| | Slow Query (> 1000ms) | 0 query | 1 - 5 query/phút | > 5 query/phút |
| **Cổng API Gateway** | Tỷ lệ lỗi HTTP 5xx | < 0.05% | 0.1% - 1% | > 1% tổng số request |
| | Độ trễ trung bình (p95) | < 500ms | 500ms - 1000ms | > 1000ms |

---

## II. Quy trình Bật / Tắt Hệ thống (Startup & Shutdown Procedures)

*LƯU Ý QUAN TRỌNG: Phải tuân thủ nghiêm ngặt thứ tự phụ thuộc (Dependency Order). Tuyệt đối không bật/tắt tùy tiện làm hỏng dữ liệu hoặc nghẽn kết nối.*

### 2.1 Quy trình Bật hệ thống (Startup Sequence)

```
[1. Storage / CSDL] ──► [2. Cache / Broker] ──► [3. Service Registry] ──► [4. Core Microservices] ──► [5. API Gateway] ──► [6. Web Frontend]
```

| Bước | Dịch vụ / Tiến trình | Lệnh thực thi / Thao tác | Tiêu chí kiểm tra thành công |
| :---: | :--- | :--- | :--- |
| **1** | CSDL chính (PostgreSQL / Oracle) | `sudo systemctl start postgresql-15`<br>hoặc kiểm tra cluster | `pg_isready -h localhost -p 5432`<br>Trả về `accepting connections` |
| **2** | Redis Cache & Kafka Queue | `docker start redis-cluster`<br>`docker start kafka-cluster` | `redis-cli ping` trả lời `PONG`<br>`kcat -b localhost:9092 -L` hiển thị topics |
| **3** | Service Registry (Consul / Eureka) | `docker start ework-registry` | Truy cập Dashboard `http://ip:8761` hiển thị trạng thái UP |
| **4** | Cụm Core Microservices | `docker start ework-core ework-auth ework-report` | Log hiển thị `Started Application in X seconds`<br>Health endpoint `/actuator/health` trả về `UP` |
| **5** | API Gateway (Kong / Nginx) | `docker start ework-gateway` | Gọi thử endpoint `/healthz` trả về HTTP 200 |
| **6** | Web Portal Frontend | `docker start ework-web` | Truy cập trang chủ Web hiển thị màn hình đăng nhập |

### 2.2 Quy trình Tắt hệ thống (Graceful Shutdown Sequence)

*Quy trình tắt thực hiện theo chiều ngược lại của quy trình bật để đảm bảo không thất thoát request đang xử lý:*

1. **Bước 1**: Tắt hoặc điều hướng Traffic tại Ingress Load Balancer (chuyển sang trang bảo trì).
2. **Bước 2**: Tắt API Gateway để ngừng tiếp nhận request mới từ bên ngoài.
3. **Bước 3**: Chờ 30 giây để Core Microservices xử lý nốt các tác vụ đang thực thi dở dang (Graceful drain).
4. **Bước 4**: Tắt các Worker tiêu thụ hàng đợi (Kafka consumers) và Core Services.
5. **Bước 5**: Tắt cụm Service Registry và Redis Cache.
6. **Bước 6**: Thực hiện đồng bộ checkpoint và tắt CSDL chính: `sudo systemctl stop postgresql-15`.

---

## III. Quy trình Giám sát Hàng ngày (Daily Inspection Routine)

Kỹ sư vận hành thực hiện kiểm tra vào 08:30 sáng và 16:30 chiều mỗi ngày làm việc:

- [ ] **Kiểm tra trạng thái Service**: Thực hiện lệnh `docker ps` hoặc `kubectl get pods -n ework` đảm bảo 100% pod ở trạng thái `Running` và `Ready`.
- [ ] **Kiểm tra tài nguyên máy chủ**: Chạy `df -h` kiểm tra phân vùng `/u01` hoặc `/var/lib/docker` đảm bảo dung lượng trống > 20%.
- [ ] **Kiểm tra log lỗi**: Tra cứu lỗi trên Kibana / Grafana với bộ lọc `level: ERROR` trong 24 giờ qua.
- [ ] **Kiểm tra đồng bộ CSDL**: Kiểm tra độ trễ sao chép dữ liệu giữa Master và Standby (Replication Lag < 100MB).
- [ ] **Kiểm tra file Backup**: Xác nhận file backup của đêm hôm trước đã được tạo thành công và đồng bộ sang máy chủ lưu trữ dự phòng.

---

## IV. Ma trận Xử lý Sự cố Thường gặp (Troubleshooting Matrix)

*Cấu trúc chuẩn hóa: Mã lỗi / Hiện tượng -> Nguyên nhân gốc rễ -> Cách xử lý tức thời (Workaround) -> Giải pháp triệt để.*

| Mã lỗi / Triệu chứng | Triệu chứng nhận biết | Nguyên nhân gốc rễ (Root Cause) | Cách xử lý tức thời (Workaround) | Giải pháp triệt để (Permanent Fix) |
| :--- | :--- | :--- | :--- | :--- |
| **ERR_DB_POOL_EXHAUSTED** | Người dùng nhận lỗi HTTP 500; log ghi nhận `CannotGetJdbcConnectionException: Connection is not available` | Ứng dụng bị rò rỉ connection do transaction không được đóng hoặc số lượng kết nối tối đa (maxPoolSize) quá thấp khi có đột biến tải | 1. Tạm thời tăng `maximum-pool-size` từ 50 lên 100 trong cấu hình.<br>2. Khởi động lại service bị nghẽn | Audit toàn bộ mã nguồn tìm các hàm thiếu `@Transactional` hoặc mở kết nối thủ công mà không đóng; tối ưu hóa slow queries |
| **ERR_OUT_OF_MEMORY** | Service tự động bị tắt (OOMKilled); log máy chủ ghi nhận `java.lang.OutOfMemoryError: Java heap space` | Tác vụ xuất báo cáo dữ liệu lớn (Export Excel > 500,000 dòng) tải toàn bộ dữ liệu vào RAM | Khởi động lại service bằng lệnh `docker restart <service_name>` | Chuyển đổi cơ chế xuất file sang streaming (SXSSFWorkbook) hoặc xử lý bất đồng bộ tải file qua background job |
| **ERR_SYNC_USER_MISSING_DEPT** | Người dùng đăng nhập được qua SSO nhưng vào hệ thống thấy trắng dữ liệu phòng ban | Dữ liệu người dùng từ hệ thống Base Platform gửi sang thiếu mã đơn vị hoặc định dạng mã đơn vị không khớp | Chạy câu lệnh SQL cập nhật thủ công mã đơn vị tương ứng cho tài khoản người dùng trong CSDL | Cập nhật service đồng bộ (Sync Service) để validate bắt buộc trường phòng ban và bổ sung fallback gán đơn vị mặc định |
| **ERR_DISK_FULL** | Không thể ghi thêm dữ liệu, CSDL tự động chuyển sang chế độ Read-Only | Log hệ thống tích tụ trong thư mục `/var/log` hoặc file tạm của Tomcat không được dọn dẹp | 1. Xóa các file log nén cũ hơn 30 ngày: `find /var/log -name "*.gz" -mtime +30 -delete`<br>2. Xóa cache tạm | Cấu hình Logrotate tự động nén và xóa log sau 14 ngày; bổ sung phân vùng đĩa chuyên biệt cho CSDL |
