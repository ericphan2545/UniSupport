# Hiệu năng

## Quy mô

- Khoảng 3.000 sinh viên đăng ký, 100–200 người dùng đồng thời (Proposal).
- Hiệu năng phụ thuộc hạ tầng do trường cung cấp (Proposal).

## Mục tiêu

| Chỉ số | Mục tiêu |
|---|---|
| Thời gian phản hồi p95 ở 100–200 người dùng đồng thời | ≤ 3 giây |
| Tỷ lệ lỗi ở 100–200 người dùng đồng thời | ≤ 5% |
| Lỗi chặn khi chạy 100–200 người dùng đồng thời trên hạ tầng trường | Không có |

Đây là chỉ tiêu nội bộ của nhóm trong QMP và Team Charter, không phải điều khoản của Proposal.

## Timeout

Timeout là ngưỡng cắt để giao diện không bị treo, không phải cam kết thời gian phản hồi.

| Thao tác | Ngưỡng | Khi vượt |
|---|---|---|
| Đăng nhập: chờ SSO của trường | 5 giây | 503, báo hệ thống đăng nhập của trường đang gián đoạn |
| Lấy thông tin tài khoản từ SSO | 3 giây | 504 |
| Danh sách hồ sơ của sinh viên, hàng đợi của phòng | 5 giây | 503, giao diện có nút Thử lại |
| Dashboard và báo cáo của Admin | 10 giây | 503; dashboard giữ số liệu lần trước kèm cảnh báo "Số liệu chưa được cập nhật" |

## Cập nhật định kỳ và cache

| Nội dung | Quy tắc |
|---|---|
| Polling | 30 giây cho chuông thông báo, trạng thái hồ sơ, hàng đợi và dashboard; chỉ khi trang đang mở và phiên còn hợp lệ |
| Tác vụ quá hạn | Chạy mỗi 1 phút; hồ sơ vượt hạn chuyển `OVERDUE` trong tối đa 60 giây khi tác vụ chạy bình thường |
| Thông tin tài khoản từ SSO | Cache 15 phút, riêng từng tài khoản |
| Danh mục loại yêu cầu và phòng ban | Cache phía server |

## Giới hạn dữ liệu mỗi request

| Nội dung | Giới hạn |
|---|---|
| Danh sách hồ sơ của sinh viên | 10 mỗi trang, tối đa 50 |
| Thông báo; hàng đợi của phòng; danh sách chi tiết trên dashboard; danh sách nhận xét | 20 mỗi trang, tối đa 100 |
| Khoảng thời gian báo cáo | Mặc định 30 ngày gần nhất, tối đa 365 ngày |
| Tệp đính kèm | Tối đa 5 tệp mỗi lần, 10 MiB mỗi tệp, tổng request 55 MiB |

Rate limit: xem [security.md](security.md).

## Test hiệu năng

- Chạy kịch bản 100–200 người dùng đồng thời trên hạ tầng của trường, trước Go-live.
- Báo cáo test ghi rõ: hạ tầng, bộ dữ liệu, số người dùng, kịch bản và tốc độ gửi request, p95, tỷ lệ lỗi.
- Không dùng dữ liệu sinh viên thật.
- Công cụ và kịch bản chi tiết: chưa chốt.
