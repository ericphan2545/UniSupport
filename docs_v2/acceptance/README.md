# Kế hoạch kiểm thử và nghiệm thu

Tài liệu này mô tả cách nhóm kiểm thử UniSupport. Tiêu chí thế nào là đạt và cách xử lý lỗi nằm trong QMP (`management/02-qmp-quan-ly-chat-luong.md`), không chép lại ở đây.

| Tài liệu | Nội dung |
|---|---|
| [traceability-matrix.md](traceability-matrix.md) | Chức năng → tiêu chí chấp nhận → test case → milestone → issue → kịch bản |
| [test-scenarios.md](test-scenarios.md) | Kịch bản kiểm thử trọn luồng và kịch bản chéo (quyền, đồng thời, lỗi, giao diện, hiệu năng) |

## Phạm vi

| Loại kiểm thử | Nội dung | Ở đâu |
|---|---|---|
| Chức năng | Mọi tiêu chí chấp nhận của 42 chức năng trong `modules/`; mỗi tiêu chí là một test case | Ma trận truy vết |
| Trọn luồng | Một hồ sơ đi qua nhiều vai trò, từ tạo đến báo cáo | Kịch bản E2E |
| Chéo | Phạm vi dữ liệu, thao tác đồng thời, gửi lại, lỗi tệp và CSDL, lịch sử và nhật ký, phiên, VN/EN và responsive, danh mục, múi giờ, rate limit | Kịch bản X |
| Regression | Chạy lại kịch bản của các milestone đã đóng khi đóng milestone mới và sau mỗi bản sửa Hotfix | Kịch bản E2E của milestone cũ |
| Hiệu năng | 100–200 người dùng đồng thời | `non-functional-requirements/performance.md` |
| Usability | Tác vụ chính của 3 vai trò với người dùng thật | `non-functional-requirements/usability.md` |

Mỗi tiêu chí chấp nhận trong `modules/` có nhãn nhóm (Happy Path, Validation, Boundary, Security, Concurrency, Resilience, Rate Limit, Business Logic, Idempotency…). Test case giữ nguyên nhãn đó.

## Môi trường và dữ liệu

- Kiểm thử trên Staging, dùng SSO giả lập. Không dùng dữ liệu sinh viên thật.
- Dữ liệu seed: tài khoản 3 vai trò, ít nhất 2 phòng ban, danh mục loại yêu cầu, SLA mặc định, hồ sơ ở mọi trạng thái.
- Bộ 200 ticket mẫu dùng cho báo cáo Admin, regression và bàn giao: phủ 5 trạng thái, các phòng và loại yêu cầu, có tệp, có yêu cầu bổ sung, chuyển phòng, đánh giá và các mốc biên thời gian. QA chốt cách phân bổ 200 ticket.
- Với ca cần chờ hết hạn, dùng loại yêu cầu có SLA ngắn (ví dụ 1 giờ) trên Staging.

## Quy trình theo milestone

1. **Đầu milestone:** QA / Senior viết test case từ tiêu chí chấp nhận của các chức năng trong milestone.
2. **Trong milestone:** Tester / Junior chạy test case khi chức năng lên Staging, ghi lỗi trên GitHub theo QMP mục 6, cập nhật cột Trạng thái của ma trận truy vết.
3. **Cuối milestone:** chạy kịch bản trọn luồng của milestone và regression của các milestone đã đóng. QA / Senior đối chiếu điều kiện đóng milestone (QMP mục 2.1) rồi báo PM.

## Vai trò

| Vai trò | Việc |
|---|---|
| QA / Senior | Viết và duyệt test case, xác nhận mức lỗi, tổng hợp kết quả, báo cáo kiểm thử |
| Tester / Junior | Chuẩn bị dữ liệu, chạy test case và kịch bản, ghi lỗi, kiểm tra lại sau sửa |
| BE Dev, FE Dev | Sửa lỗi, hỗ trợ tái hiện lỗi |
| UI/UX / Mid | Tổ chức usability test |

## Ghi kết quả

- Mỗi lần chạy ghi: test case, phiên bản, môi trường, trình duyệt và thiết bị, người chạy, ngày, kết quả (Đạt / Không đạt / Bị chặn / Chưa chạy), issue lỗi nếu có.
- Test case bị chặn hoặc chưa chạy không tính là đã hoàn thành.

## Đầu ra

- Báo cáo kiểm thử chức năng và Usability Test Report (hạng mục bàn giao theo Proposal 6.1).
- Ma trận truy vết đã điền trạng thái cuối cùng.
