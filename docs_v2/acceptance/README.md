# Kế hoạch kiểm thử và nghiệm thu

Tài liệu này mô tả cách kiểm thử UniSupport trong từng milestone. Mức lỗi, cách xử lý lỗi, điều kiện đóng milestone và điều kiện Go-live nằm trong QMP ([02-qmp-quan-ly-chat-luong.md](../management/02-qmp-quan-ly-chat-luong.md)), không chép lại ở đây. Lịch và giờ công của các việc kiểm thử nằm ở sheet WBS của [Master Plan](../management/UniSupport_Masterplan.xlsx).

| Tài liệu | Nội dung |
|---|---|
| [test-scenarios.md](test-scenarios.md) | Nơi gốc của kịch bản `TS-01` đến `TS-64`; mỗi kịch bản ghi mã AC hoặc NFR nó kiểm và có cột Regression |
| [traceability-matrix.md](traceability-matrix.md) | Bảng tổng hợp theo từng AC: AC → kịch bản test → issue chịu trách nhiệm → milestone đạt lần đầu → hạng mục báo giá |

## Phạm vi

| Loại kiểm thử | Nội dung | Kịch bản |
|---|---|---|
| Chức năng | Mọi AC của 42 FR trong `modules/` | `TS-21` đến `TS-62`, mỗi FR một kịch bản |
| Trọn luồng | Một hồ sơ đi qua nhiều vai trò, từ tạo đến báo cáo; client dùng khi demo milestone | `TS-01` đến `TS-08` |
| Chéo & phi chức năng | Phạm vi dữ liệu, đồng thời, gửi lại, lỗi tệp/CSDL, lịch sử và nhật ký, phiên, VN/EN và responsive, danh mục, múi giờ, rate limit, bảo mật đầu vào CSRF/XSS và `correlation_id`/log vận hành | `TS-09` đến `TS-18`, `TS-63` |
| Regression | Chạy lại các kịch bản có Regression = có của các milestone đã giao, trước mỗi lần bàn giao và sau mỗi bản sửa Hotfix | Cột Regression trong test-scenarios.md |
| Hiệu năng | 100–200 người dùng đồng thời trên hạ tầng của trường | `TS-19` (`NFR-PERF-01` đến `NFR-PERF-03`) |
| Usability | Tác vụ chính của 3 vai trò với người dùng thật | `TS-20` (`NFR-USE-01`) |
| Sẵn sàng production | HTTPS, cấm SSO giả lập trên production, rà bí mật repo/env, không tự xóa dữ liệu và chạy lại `TS-01`..`TS-08` trên server trường | `TS-64` (`NFR-SEC-01, 10, 11`, `NFR-DEP-03, 04`, `NFR-REL-10`) |

Mỗi AC trong `modules/` có nhãn nhóm (Happy Path, Validation, Boundary, Security, Concurrency, Resilience, Rate Limit, Business Logic, Idempotency…) và ghi ở cuối dòng các business rule (`BR-xx`), error flow (`E-xx`) mà nó kiểm ("Kiểm cho"). Mỗi `BR-xx`, `E-xx` của 42 FR có ít nhất một AC. Kịch bản chéo chọn AC theo nhãn này.

## Môi trường và dữ liệu

- Kiểm thử trên Staging, dùng SSO giả lập. Không dùng dữ liệu sinh viên thật.
- Dữ liệu seed (`MS-01`): tài khoản 3 vai trò, ít nhất 2 phòng ban, danh mục loại yêu cầu, SLA mặc định, hồ sơ ở mọi trạng thái.
- Bộ 200 ticket mẫu (`MS-05`) dùng cho báo cáo Admin, regression và bàn giao: phủ 5 trạng thái, các phòng và loại yêu cầu, có tệp, có yêu cầu bổ sung, chuyển phòng, đánh giá và các mốc biên thời gian. QA / Senior chốt cách phân bổ 200 ticket.
- Với ca cần chờ hết hạn, dùng loại yêu cầu có SLA ngắn (ví dụ 1 giờ) trên Staging.
- `TS-19` chạy trên server của trường trong `MS-06`, trước Go-live.

## Quy trình theo milestone

| Bước | Ai làm | Khi nào | Dùng gì |
|---|---|---|---|
| 1. Viết test case từ AC của các FR trong milestone | QA / Senior; Tester / Junior hỗ trợ | Đầu milestone (dòng WBS "Viết test case từ AC") | Kịch bản chức năng `TS-21` đến `TS-62` |
| 2. Chạy test case theo từng chức năng ngay khi hoàn thành lên Staging; ghi lỗi | Tester / Junior | Trong milestone (bắt đầu sau khi hoàn tất dòng WBS "Viết test case từ AC") | Mẫu issue Bug trên GitHub, mức lỗi theo QMP mục 6 |
| 3. Chạy kịch bản trọn luồng của milestone và regression của các milestone đã giao | Tester / Junior; QA / Senior duyệt | Đợt cuối milestone, **sau khi hoàn tất toàn bộ dòng WBS phát triển, phần dùng chung và dữ liệu mẫu của milestone** (dòng WBS "Test trọn luồng và regression") | Kịch bản `TS-01` đến `TS-18` có Regression = có |
| 4. Đối chiếu điều kiện đóng milestone, báo PM | QA / Senior | Trước buổi demo milestone | QMP mục 2.1; sheet Milestones cột "Điều kiện nghiệm thu" |
| 5. Demo cho client, client tự chạy kịch bản trọn luồng | PM / Senior, BA / Mid, QA / Senior | Cuối milestone | Sheet Milestones cột "Client kiểm tra bằng cách"; góp ý ghi bằng mẫu issue Feedback |

## Vai trò

| Vai trò | Việc |
|---|---|
| QA / Senior | Viết và duyệt test case, xác nhận mức lỗi, tổng hợp kết quả, báo cáo kiểm thử |
| Tester / Junior | Chuẩn bị dữ liệu, chạy test case và kịch bản, ghi lỗi, kiểm tra lại sau sửa |
| BE Dev, FE Dev | Sửa lỗi, hỗ trợ tái hiện lỗi |
| UI/UX / Mid | Tổ chức usability test (`TS-20`) |
| DevOps / Mid | Chạy test hiệu năng (`TS-19`) trên server của trường |

## Ghi kết quả

- Mỗi lần chạy ghi: kịch bản `TS-xx` và mã AC, phiên bản, môi trường, trình duyệt và thiết bị, người chạy, ngày, kết quả (Đạt / Không đạt / Bị chặn / Chưa chạy), issue lỗi nếu có.
- Test case bị chặn hoặc chưa chạy không tính là đã hoàn thành.

## Đầu ra

- Báo cáo kiểm thử chức năng và Usability Test Report (hạng mục bàn giao theo Proposal mục 6.1).
