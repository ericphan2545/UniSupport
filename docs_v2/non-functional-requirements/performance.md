# Hiệu năng

## Quy mô

- Khoảng 3.000 sinh viên đăng ký, 100–200 người dùng đồng thời (Proposal mục 1.3, giả định `A-01`).
- Hiệu năng phụ thuộc hạ tầng do trường cung cấp (giả định `A-02`).

## Mục tiêu

| Mã | Chỉ số | Ngưỡng | Cách đo |
|---|---|---|---|
| `NFR-PERF-01` | Thời gian phản hồi p95 ở 100–200 người dùng đồng thời | ≤ 3 giây | `TS-19` trên hạ tầng trường |
| `NFR-PERF-02` | Tỷ lệ lỗi ở 100–200 người dùng đồng thời | ≤ 5% | `TS-19` |
| `NFR-PERF-03` | Lỗi chặn khi chạy 100–200 người dùng đồng thời trên hạ tầng trường | Không có | `TS-19` |

Ba mục tiêu trên là chỉ tiêu trong QMP và Team Charter, và là mục tiêu `G-05` trong Proposal mục 1.2.

## Timeout

Timeout là ngưỡng cắt để giao diện không bị treo, không phải cam kết thời gian phản hồi.

| Mã | Thao tác | Ngưỡng | Khi vượt | Cách đo |
|---|---|---|---|---|
| `NFR-PERF-04` | Đăng nhập: chờ SSO của trường | 5 giây | 503, báo hệ thống đăng nhập của trường đang gián đoạn | `FR-SEC-01/AC-06` |
| `NFR-PERF-05` | Lấy thông tin tài khoản từ SSO | 3 giây | 504 | `FR-STU-01/AC-06` |
| `NFR-PERF-06` | Danh sách hồ sơ của sinh viên, hàng đợi của phòng | 5 giây | 503, giao diện có nút Thử lại | `FR-STU-06/AC-06`, `FR-STF-01/AC-06` |
| `NFR-PERF-07` | Dashboard và báo cáo của Admin | 10 giây | 503; dashboard giữ số liệu lần trước kèm cảnh báo "Số liệu chưa được cập nhật" | `FR-MGT-01/AC-06` đến `FR-MGT-05/AC-06` |

## Cập nhật định kỳ và cache

| Mã | Nội dung | Ngưỡng | Cách đo |
|---|---|---|---|
| `NFR-PERF-08` | Polling chuông thông báo, trạng thái hồ sơ, hàng đợi, dashboard; chỉ khi trang đang mở và phiên còn hợp lệ | 30 giây | `TS-03`, `TS-08`, `FR-STU-07/AC-04`, `FR-STU-08/AC-01` |
| `NFR-PERF-09` | Tác vụ quá hạn chạy mỗi 1 phút; hồ sơ vượt hạn chuyển `OVERDUE` khi tác vụ chạy bình thường | Tối đa 60 giây | `TS-06` |
| `NFR-PERF-10` | Cache thông tin tài khoản lấy từ SSO, riêng từng tài khoản | 15 phút | `FR-STU-01/AC-05` |
| `NFR-PERF-11` | Cache danh mục loại yêu cầu và phòng ban phía server | Đọc từ cache khi nhiều người mở form | `FR-STU-03/AC-04` |

## Giới hạn dữ liệu mỗi request

| Mã | Nội dung | Giới hạn | Cách đo |
|---|---|---|---|
| `NFR-PERF-12` | Phân trang: danh sách hồ sơ của sinh viên 10 mỗi trang, tối đa 50; thông báo, hàng đợi, danh sách chi tiết trên dashboard, danh sách nhận xét 20 mỗi trang, tối đa 100 | Vượt tối đa trả 400 | `FR-STU-06/AC-03`, `FR-STF-01/AC-03` |
| `NFR-PERF-13` | Khoảng thời gian báo cáo | Mặc định 30 ngày gần nhất, tối đa 365 ngày | `FR-MGT-02/AC-03` |
| `NFR-PERF-14` | Tệp đính kèm | Tối đa 5 tệp mỗi lần, 10 MiB mỗi tệp, tổng request 55 MiB | `TS-12` |

Rate limit: xem [security.md](security.md) (`NFR-SEC-13`).

## Test hiệu năng

- Chạy kịch bản 100–200 người dùng đồng thời (`TS-19`) trên server của trường, trước Go-live, trong việc triển khai production của `MS-06` (dòng WBS "Triển khai lên server trường, Admin đầu tiên, nối SSO thật", issue #142): DevOps / Mid chạy, QA / Senior duyệt kết quả.
- Báo cáo test ghi rõ: hạ tầng, bộ dữ liệu, số người dùng, kịch bản và tốc độ gửi request, p95, tỷ lệ lỗi.
- Không dùng dữ liệu sinh viên thật.
- Công cụ và kịch bản chi tiết: QA / Senior chốt trước khi chạy test.
