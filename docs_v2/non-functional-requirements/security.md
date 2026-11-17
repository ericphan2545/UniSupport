# Bảo mật

Proposal không yêu cầu tuân thủ chuẩn bảo mật chuyên sâu, chỉ bảo vệ dữ liệu sinh viên ở mức cơ bản (giả định `A-04`, non-goal `NG-10`): đăng nhập bằng tài khoản trường, giới hạn phạm vi truy cập dữ liệu sinh viên, bảo vệ tệp đính kèm, ghi nhật ký hệ thống.

Cách thực hiện từng biện pháp nằm ở `architecture/security-design.md`. Tài liệu này ghi yêu cầu, cách kiểm chứng và ngưỡng rate limit.

## Yêu cầu và cách kiểm chứng

| Mã | Yêu cầu | Cách đo |
|---|---|---|
| `NFR-SEC-01` | Chỉ đăng nhập qua SSO của trường; SSO giả lập không chạy ở production | `TS-01`; khởi động bản production với cấu hình bật SSO giả lập → ứng dụng từ chối khởi động |
| `NFR-SEC-02` | Phiên trong cookie HttpOnly, Secure, SameSite=Lax; 8 giờ; thu hồi khi đăng xuất hoặc tài khoản bị vô hiệu hóa | `TS-01`, `TS-14` |
| `NFR-SEC-03` | Vai trò và phạm vi dữ liệu kiểm tra ở backend; ngoài phạm vi trả 404, không có quyền ghi trả 403 | `TS-01`, `TS-09` |
| `NFR-SEC-04` | Request ghi kiểm tra CSRF token và Origin | Gửi request ghi thiếu CSRF token hoặc từ Origin khác → 403 |
| `NFR-SEC-05` | Nội dung người dùng nhập không thực thi được | Nhập thẻ HTML và script vào mô tả, ghi chú, kết quả, lý do, nhận xét, tên tệp → hiển thị nguyên văn |
| `NFR-SEC-06` | Tệp chỉ tải được qua API có kiểm tra quyền; định dạng kiểm tra bằng magic bytes | `TS-09`, `TS-12` |
| `NFR-SEC-07` | Trang và tệp có dữ liệu cá nhân không bị lưu cache | `TS-14`; header `Cache-Control: private, no-store` |
| `NFR-SEC-08` | Lỗi không lộ thông tin hệ thống | Response lỗi của mọi test case không có stack trace, câu SQL, đường dẫn kho, token |
| `NFR-SEC-09` | Nhật ký hệ thống chỉ thêm mới; log không chứa cookie, token, nội dung nhạy cảm | `TS-13`; rà log sau khi chạy các kịch bản `TS-01` đến `TS-08` |
| `NFR-SEC-10` | Chỉ phục vụ HTTPS | Gọi bằng HTTP → chuyển sang HTTPS |
| `NFR-SEC-11` | Bí mật không nằm trong repo | Rà repo và `.env.example` trước bàn giao |
| `NFR-SEC-12` | Admin không xem chi tiết hồ sơ, thông tin cá nhân, tệp của sinh viên | `TS-08` bước 2 và 7 |
| `NFR-SEC-13` | Rate limit theo bảng dưới | `TS-18` |

## Rate limit (`NFR-SEC-13`)

Vượt giới hạn trả 429 kèm `Retry-After`. Chức năng không có trong bảng chưa có ngưỡng riêng.

| Chức năng | Giới hạn | Tính theo |
|---|---|---|
| Callback đăng nhập SSO (`FR-SEC-01`) | 10 lần/phút | IP |
| Tạo yêu cầu hỗ trợ (`FR-STU-02`) | 10 hồ sơ/giờ | Sinh viên |
| Danh mục loại yêu cầu và phòng tiếp nhận (`FR-STU-03`) | 60 request/phút | Người dùng |
| Xem danh sách yêu cầu của mình (`FR-STU-06`) | 60 request/phút | Sinh viên |
| Xem trạng thái và tiến độ của từng yêu cầu (`FR-STU-07`) | 60 request/phút | Sinh viên |
| Hàng đợi hồ sơ của phòng ban (`FR-STF-01`) | 120 request/phút | Nhân viên |
| Xem thông tin sinh viên và nội dung yêu cầu (`FR-STF-02`) | 120 request/phút | Nhân viên |
| Xem và tải tệp đính kèm (`FR-STF-03`) | 30 lần tải/phút | Nhân viên |
| Ghi nhật ký xử lý (`FR-STF-04`) | 30 ghi chú/phút | Nhân viên |
| Dashboard: số hồ sơ mới, đang xử lý, quá hạn (`FR-MGT-01`) | 60 request/phút | Người dùng |
| Dashboard: thời gian xử lý trung bình (`FR-MGT-02`) | 60 request/phút | Người dùng |
| Báo cáo loại yêu cầu phổ biến (`FR-MGT-03`) | 30 request/phút | Người dùng |
| Báo cáo khối lượng công việc theo phòng ban (`FR-MGT-04`) | 30 request/phút | Người dùng |
| Báo cáo mức độ hài lòng của sinh viên (`FR-MGT-05`) | 30 request/phút | Người dùng |
| Quản lý phân quyền (`FR-MGT-06`) | 30 thao tác ghi/phút | Người dùng |
| Thiết lập SLA (`FR-MGT-07`) | 30 thao tác ghi/phút | Người dùng |

- Ngưỡng callback đăng nhập cần kiểm chứng nếu mạng trường dùng chung một IP (NAT).
- Gửi lại thao tác tạo hồ sơ đã thành công với cùng `Idempotency-Key` không tính thêm vào giới hạn 10 hồ sơ/giờ.
