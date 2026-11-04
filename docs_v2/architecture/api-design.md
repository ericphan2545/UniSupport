# Thiết kế API

Danh sách endpoint dưới đây là đề xuất để thống nhất giữa backend và frontend. Bản chuẩn khi bàn giao là tài liệu OpenAPI/Swagger.

## Quy ước

| Mục | Quy ước |
|---|---|
| Định dạng | JSON; tải tệp lên bằng `multipart/form-data` |
| Phiên | Cookie phiên (HttpOnly, Secure, SameSite=Lax). Không bắt buộc header Authorization |
| Chống CSRF | Mọi request ghi gửi CSRF token gắn với phiên; server kiểm tra thêm Origin. API ghi không dùng GET |
| Idempotency | Header `Idempotency-Key` bắt buộc cho: tạo hồ sơ, bổ sung thông tin, ghi nhật ký xử lý, yêu cầu bổ sung, đóng hồ sơ |
| Version | Mọi thao tác thay đổi hồ sơ, tài khoản, SLA gửi `version` hiện tại; lệch trả 409 |
| Phân trang | `page` (≥1), `page_size` (giới hạn theo từng danh sách); response có `items`, `page`, `page_size`, `total_items`, `total_pages` |
| Rate limit | Vượt giới hạn trả 429 kèm `Retry-After` |
| Ngôn ngữ | Thông báo lỗi có cả VN và EN; nội dung người dùng nhập không tự dịch |

## Định dạng lỗi

```json
{
  "error_code": "TICKET_VERSION_CONFLICT",
  "message": { "vi": "Hồ sơ đã được cập nhật bởi người khác", "en": "The ticket was updated by someone else" },
  "field_errors": [],
  "correlation_id": "c1f3…"
}
```

Không trả stack trace, câu SQL, đường dẫn kho tệp hay token.

| HTTP | Khi nào |
|---|---|
| 400 | Input sai cú pháp, sai giới hạn |
| 401 | Thiếu, sai, hết hạn hoặc đã thu hồi phiên |
| 403 | Sai vai trò, tài khoản bị vô hiệu hóa, sai CSRF, hoặc có quyền xem nhưng không có quyền ghi |
| 404 | Không tồn tại hoặc ngoài phạm vi (cùng một response) |
| 409 | Lệch version, sai trạng thái, trùng unique, xung đột idempotency |
| 413 / 415 | Tệp quá dung lượng / sai định dạng |
| 422 | Thiếu cấu hình nghiệp vụ (ví dụ loại yêu cầu chưa có SLA), người nhận hoặc phòng đích không hợp lệ |
| 429 | Vượt rate limit |
| 500 | Lỗi CSDL hoặc ghi nghiệp vụ chưa commit |
| 502 / 504 | Dữ liệu SSO không hợp lệ / SSO quá thời gian chờ |
| 503 | Dịch vụ, kho tệp hoặc kiểm tra quyền tạm lỗi |

## Endpoint

### Xác thực và chung

| Method | Đường dẫn | Ai gọi | Mục đích |
|---|---|---|---|
| GET | `/api/auth/login` | Công khai | Chuyển hướng sang SSO |
| GET | `/api/auth/callback` | Công khai | Nhận kết quả SSO, tạo phiên |
| POST | `/api/auth/logout` | Mọi vai trò | Thu hồi phiên hiện tại (idempotent) |
| GET | `/api/me` | Mọi vai trò | Vai trò, phòng ban, CSRF token |
| GET | `/api/catalog/request-types` | STUDENT, STAFF | Loại yêu cầu kèm phòng ban tiếp nhận (60 request/phút) |
| GET | `/api/files/{file_id}?mode=inline\|download` | Sinh viên chủ hồ sơ, nhân viên phòng đang giữ | Xem hoặc tải tệp |

### Cổng Sinh viên

| Method | Đường dẫn | Mục đích |
|---|---|---|
| GET | `/api/student/profile` | Thông tin cá nhân lấy từ SSO |
| POST | `/api/student/tickets` | Tạo yêu cầu kèm tệp (Idempotency-Key; 10 hồ sơ/giờ) |
| GET | `/api/student/tickets` | Danh sách yêu cầu của mình (`page_size` 10, tối đa 50) |
| GET | `/api/student/tickets/{code}` | Chi tiết, tệp, yêu cầu bổ sung, lịch sử công khai, kết quả |
| POST | `/api/student/tickets/{code}/supplements` | Bổ sung thông tin (Idempotency-Key, version) |
| POST | `/api/student/tickets/{code}/rating` | Đánh giá 1–5 sao một lần |
| GET | `/api/student/notifications` | Danh sách và số chưa đọc (`page_size` 20, tối đa 100) |
| POST | `/api/student/notifications/{id}/read` | Đánh dấu đã đọc |
| POST | `/api/student/notifications/read-all` | Đánh dấu tất cả đến mốc `read_before` |

### Cổng Nhân viên

| Method | Đường dẫn | Mục đích |
|---|---|---|
| GET | `/api/staff/tickets` | Hàng đợi của phòng (lọc trạng thái, ưu tiên; sắp ưu tiên cao trước) |
| GET | `/api/staff/tickets/{code}` | Chi tiết, thông tin sinh viên, tệp, lịch sử đầy đủ, `allowed_actions` |
| POST | `/api/staff/tickets/{code}/assign` | Phân công người phụ trách cùng phòng |
| POST | `/api/staff/tickets/{code}/notes` | Ghi nhật ký xử lý (Idempotency-Key) |
| POST | `/api/staff/tickets/{code}/info-requests` | Yêu cầu sinh viên bổ sung (Idempotency-Key) |
| POST | `/api/staff/tickets/{code}/close` | Đóng hồ sơ kèm kết quả (Idempotency-Key) |
| PUT | `/api/staff/tickets/{code}/priority` | Đặt mức ưu tiên |
| POST | `/api/staff/tickets/{code}/escalate` | Tăng mức ưu tiên kèm lý do |
| POST | `/api/staff/tickets/{code}/transfer` | Chuyển sang phòng ban khác kèm lý do |

### Cổng Admin

| Method | Đường dẫn | Mục đích |
|---|---|---|
| GET | `/api/admin/dashboard` | Số hồ sơ mới, đang xử lý, quá hạn; thời gian xử lý trung bình; danh sách tóm tắt |
| GET | `/api/admin/reports/request-types` | Loại yêu cầu phổ biến |
| GET | `/api/admin/reports/departments` | Khối lượng theo phòng ban |
| GET | `/api/admin/reports/satisfaction` | Mức độ hài lòng của sinh viên |
| GET / POST / PATCH | `/api/admin/accounts` | Thêm email nhân viên, đổi vai trò, gán phòng ban, vô hiệu hóa |
| GET / PATCH | `/api/admin/request-types/{id}/sla` | Xem và sửa SLA của từng loại yêu cầu |

Báo cáo nhận `from`, `to` dạng `YYYY-MM-DD`, mặc định 30 ngày gần nhất, tối đa 365 ngày. Không có API trả chi tiết hồ sơ, thông tin cá nhân hay tệp cho Admin.
