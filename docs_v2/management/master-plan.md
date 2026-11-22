# Master Plan

Trang này giải thích các quyết định của kế hoạch. Số liệu (ngày, effort, owner, nguồn lực) chỉ nằm trong [UniSupport_Masterplan_nhom2.xlsx](UniSupport_Masterplan_nhom2.xlsx):

| Sheet | Nội dung |
|---|---|
| Milestones | Mã `MS-xx`, phần giao chạy được, cách client kiểm tra, điều kiện nghiệm thu, ngày đã chốt, phụ thuộc |
| WBS | Mỗi dòng một hạng mục của một vai trò: milestone, hạng mục báo giá `Q-xx`, effort, owner, ngày, phụ thuộc, FR, issue GitHub |
| Resources | Giờ/tuần, số tuần, effort được giao, dư hay thiếu của từng vai trò; quỹ dự phòng |
| Gantt | Tự vẽ từ ngày trong WBS |
| Change log | Mỗi phiên bản kế hoạch, ghi giá trị cũ và mới |

Khách hàng và đội thực thi là **Mô phỏng**. Người chịu trách nhiệm chính file Excel: Phan Tiến Thịnh.

## Cách chia milestone

Kế hoạch có 6 milestone, chia theo luồng nghiệp vụ của một hồ sơ hỗ trợ. Mỗi milestone giao một phần phần mềm chạy trên Staging để khách tự dùng thử (sheet Milestones, cột "Phần giao chạy được"); riêng `MS-06` chạy trên server của trường.

| Milestone | Vì sao đặt ở vị trí này |
|---|---|
| `MS-01` Chốt yêu cầu, đăng nhập và phân quyền | Mọi chức năng khác cần phiên đăng nhập và vai trò. Hai tuần đầu dùng prototype dữ liệu mẫu để khách bấm thử và chốt yêu cầu; việc chốt yêu cầu nằm trong `MS-01` chứ không thành milestone riêng, vì prototype chưa phải phần mềm khách dùng được. Cuối tuần 3 khách đăng nhập thử được trên Staging. |
| `MS-02` Gửi và tiếp nhận yêu cầu | Tạo ra hồ sơ, dữ liệu đầu vào cho mọi bước sau. Ví dụ: tạo yêu cầu (`FR-STU-02`) cần sinh viên đã đăng nhập, danh mục đã cấu hình (`FR-CORE-05`) và SLA (`FR-MGT-07`), xem Preconditions trong [PRD M01](../modules/M01-student-portal/prd.md). |
| `MS-03` Phân công, xử lý, chuyển phòng ban | Cần hồ sơ đã có trong hàng đợi. Sau milestone này luồng chính gửi → phân công → xử lý → chuyển phòng chạy trọn, nên đợt thanh toán 2 gắn với nghiệm thu `MS-03`. |
| `MS-04` Trả kết quả cho sinh viên | Đóng hồ sơ và đánh giá cần hồ sơ đã được xử lý. |
| `MS-05` Thống kê báo cáo cho Admin | Báo cáo cần dữ liệu đủ trạng thái, thời gian xử lý và đánh giá; đối chiếu bằng bộ 200 ticket mẫu. Sau `MS-05` không thêm chức năng (feature freeze). |
| `MS-06` Triển khai, bàn giao và bảo hành | Chỉ triển khai lên server của trường khi đủ chức năng và đã chạy regression. Go-live 22/12/2026, bảo hành đến 22/01/2027. |

**Ưu tiên FR:** thứ tự milestone theo phụ thuộc của luồng nghiệp vụ. Trong từng milestone, FR mức `Must` làm trước; nếu thiếu giờ thì FR mức `Should` của milestone đó được đề xuất dời qua change request. Thang ưu tiên ở [product/README.md](../product/README.md#thang-ưu-tiên-fr).

## Phụ thuộc giữa các milestone

- Giữa các milestone: `MS-01` → `MS-02` → `MS-03` → `MS-04` → `MS-05` → `MS-06` (sheet Milestones, cột "Phụ thuộc").
- Giữa các việc: cột "Phụ thuộc" của sheet WBS là nơi gốc, ghi mã WBS phải xong trước. Mỗi issue GitHub ghi mã WBS của nó; issue không ghi lại phụ thuộc.
- Phát triển backend và frontend của mỗi milestone chỉ bắt đầu sau khi test trọn luồng của milestone trước kết thúc, để phần mới dựng trên phần đã nghiệm thu.
- Khi một milestone trễ: các milestone sau dời theo. Go-live 22/12/2026 là mốc đã chốt với khách, nên trước hết dùng quỹ dự phòng của milestone đó và năng lực dư ở sheet Resources; nếu vẫn không giữ được mốc thì PM / Senior báo khách và xử lý như thay đổi về mốc ([03-quan-tri-giam-sat.md](03-quan-tri-giam-sat.md) mục 5).

## Effort cho test, regression và sửa lỗi

Đây là việc biết trước nên nằm trong WBS và báo giá như việc khác, không lấy từ quỹ dự phòng:

- Mỗi milestone `MS-01` đến `MS-05` có 3 nhóm dòng WBS: "Viết test case từ AC", "Test trọn luồng và regression", "Sửa lỗi sau test". Chúng thuộc hạng mục báo giá của milestone đó (`Q-02` đến `Q-06`), loại effort "Kiểm thử", "Tích hợp và kiểm thử", "Sửa lỗi" ở sheet Chi tiết effort của [quotation.xlsx](quotation.xlsx).
- "Test trọn luồng và regression" chạy kịch bản của milestone đang làm và chạy lại các kịch bản đánh dấu Regression của các milestone trước ([test-scenarios.md](../acceptance/test-scenarios.md)), trước mỗi lần bàn giao cho khách.
- `MS-06`: usability test, báo cáo kiểm thử (`Q-07`); sửa lỗi trong bảo hành (`Q-08`).
- Số AC: bộ đặc tả có 339 AC (mỗi business rule và error flow có ít nhất một AC). Giờ "Viết test case từ AC" và "Test trọn luồng và regression" tính theo milestone, không theo số AC; nếu thực tế vượt giờ kế hoạch thì PM / Senior xếp thêm từ năng lực dư ở sheet Resources, không lấy từ quỹ dự phòng.
- Tích hợp: dựng Staging và CI deploy (`MS-01`), test trọn luồng trên Staging ở mỗi milestone, nối SSO thật trên server trường (`MS-06`).

## Dự phòng cho thay đổi và rủi ro

- **Rủi ro đã nhận diện:** mỗi hạng mục có một dòng WBS bắt đầu bằng "Dự phòng Q-xx", giao cho "Quỹ dự phòng" ở sheet Resources (tổng và tỷ lệ tự tính ở ô E20, E21). Dòng ghi mã rủi ro được dùng ([risk-register.md](risk-register.md)). PM / Senior duyệt việc dùng quỹ và ghi số giờ đã dùng vào báo cáo tuần. Quỹ này đã nằm trong 870 giờ và trong giá.
- **Không tính trùng:** sửa lỗi sau test và bảo hành là dòng WBS riêng; quỹ dự phòng chỉ dùng khi rủi ro trong sổ xảy ra. Dự phòng lao động 20% trong bảng giá nội bộ là dự phòng tài chính, không cấp thêm giờ ([Bang_Effort.md](Bang_Effort.md) Phần 3).
- **Change request:** báo giá và lên lịch riêng theo Proposal mục 4, không lấy từ quỹ dự phòng. Cột "Dư" ở sheet Resources cho biết năng lực còn trống của từng vai trò để xếp CR đã duyệt mà không kéo mốc khác. Các lần sửa nhỏ trong giới hạn 2–5 lần của Proposal làm trong dòng "Sửa lỗi sau test" của milestone đang chạy.

## Nguồn lực

Đội thực thi (Mô phỏng) có 10 người, mỗi vai trò 1 người, khai báo ở sheet Resources và [team-charter.md](team-charter.md). Giờ/tuần của mỗi người đủ cho tuần cao điểm nhất của vai trò đó trong kế hoạch; phần dư là năng lực còn trống. Ô kiểm tra "effort WBS chưa gán đúng người" phải bằng 0.

## Cách nhận feedback của client sau mỗi milestone

1. Cuối mỗi milestone có buổi demo: đại diện trường tự thao tác trên Staging bằng tài khoản thử theo kịch bản ghi ở sheet Milestones, cột "Client kiểm tra bằng cách".
2. Góp ý ghi bằng issue mẫu Feedback trên GitHub (nhãn `feedback`), trong buổi demo hoặc trong 2 ngày làm việc sau đó.
3. BA / Mid phân loại: lỗi đi theo quy trình lỗi ([02-qmp-quan-ly-chat-luong.md](02-qmp-quan-ly-chat-luong.md) mục 6); thay đổi phạm vi đi theo quy trình change request ([03-quan-tri-giam-sat.md](03-quan-tri-giam-sat.md) mục 5).
4. Đạt điều kiện nghiệm thu thì hai bên ký biên bản nghiệm thu milestone; PM / Senior gửi kèm báo cáo milestone (03 mục 2.3).

## Thay đổi kế hoạch

Kế hoạch baseline ban đầu là phiên bản `v1.0` (04/09/2026) và đã được làm rõ ở phiên bản `v1.1` (05/09/2026, giữ nguyên baseline 6 milestone, 870 giờ công và tổng giá 332.316.000 VNĐ) trong sheet Change log, gửi kèm Proposal v1.1 ([sent/](sent/README.md)). Mỗi thay đổi phạm vi, giá hoặc mốc được duyệt (CR) ghi một dòng mới vào Change log (phiên bản, ngày, CR, giá trị cũ và mới), không ghi đè.

Trước khi Aurora University ký chốt chính thức bộ đặc tả và kế hoạch, các đợt rà soát, chuẩn hóa nội bộ của nhóm được thực hiện qua Pull Request và phát hành phiên bản làm rõ trong Change log mà không tính là change request. Sau khi hai bên đã chốt phạm vi, mọi thay đổi phạm vi, giá hoặc mốc do khách yêu cầu (thật hoặc `Mô phỏng`) đều đi qua quy trình CR và ghi Change log.
