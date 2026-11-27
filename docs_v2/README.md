# Mục lục tài liệu UniSupport

Hệ thống quản lý hỗ trợ sinh viên cho Aurora University (khách hàng **Mô phỏng**): sinh viên gửi yêu cầu, nhân viên phòng ban tiếp nhận và xử lý, ban quản lý theo dõi bằng báo cáo. Kế hoạch công việc (milestone `MS-xx`, parent task) nằm ở tab Issues và Milestones của repo.

Nội dung không có trong mục lục này thì không phải tài liệu đang dùng. Mỗi thông tin chỉ có một nơi gốc; nơi khác dẫn lại bằng mã.

## Thứ tự đọc

1. **Bài toán và phạm vi:** [Proposal](management/proposal.docx) (bản đã gửi: [sent/](management/sent/README.md)) → [Tổng quan sản phẩm](product/README.md) (vấn đề, actor, goals, non-goals, thang ưu tiên).
2. **Yêu cầu:** PRD 5 module: [M01 Cổng Sinh viên](modules/M01-student-portal/prd.md), [M02 Cổng Nhân viên](modules/M02-staff-portal/prd.md), [M03 Cổng Admin](modules/M03-management-portal/prd.md), [M04 Xác thực và bảo mật](modules/M04-authentication-security/prd.md), [M05 Lõi hệ thống](modules/M05-core-platform/prd.md) (giới thiệu từng module: [M01](modules/M01-student-portal/README.md), [M02](modules/M02-staff-portal/README.md), [M03](modules/M03-management-portal/README.md), [M04](modules/M04-authentication-security/README.md), [M05](modules/M05-core-platform/README.md)) → [Yêu cầu phi chức năng](non-functional-requirements/README.md).
3. **Nghiệm thu:** [Kế hoạch kiểm thử](acceptance/README.md) → [Kịch bản test](acceptance/test-scenarios.md) → [Ma trận truy vết](acceptance/traceability-matrix.md).
4. **Giá và kế hoạch:** [Báo giá](management/quotation.xlsx) → [Master Plan](management/master-plan.md) và [file Excel](management/UniSupport_Masterplan.xlsx) → [Sổ rủi ro](management/risk-register.md) → [Giả định](project/assumptions.md), [Ràng buộc](project/constraints.md) → [Dự toán effort và giá nội bộ](management/Bang_Effort.md).
5. **Cách làm việc:** [Team Charter](management/team-charter.md) → [Kế hoạch quản lý chất lượng (QMP)](management/02-qmp-quan-ly-chat-luong.md).
6. **Sản phẩm sau bàn giao:** [Kế hoạch quản lý sản phẩm](management/01-quan-ly-san-pham.md).
7. **Quản trị và giám sát:** [Kế hoạch quản trị và giám sát](management/03-quan-tri-giam-sat.md) → [Hồ sơ quản trị](management/records/README.md) ([Bảng theo dõi giờ](management/records/time-tracking-log.md), [Decision Log](management/records/decision-log.md), [Biên bản họp](management/records/meeting-minutes-template.md), GitHub Issues / Milestones).
8. **Tài liệu hỗ trợ (bằng chứng):** [Kiến trúc](architecture/README.md) (bối cảnh, thành phần, luồng, mô hình dữ liệu, API, bảo mật, quyết định kỹ thuật), [Thuật ngữ](project/glossary.md).

## Tra nhanh vị trí

| Nội dung | Ở đâu |
|---|---|
| Scope: làm và không làm | `management/proposal.docx` mục 2.1 và 2.2; [product/README.md](product/README.md) mục Goals và Non-goals; danh sách 42 FR trong `modules/` |
| Giá và hạng mục báo giá | [quotation.xlsx](management/quotation.xlsx) sheet Hạng mục (`Q-xx`), Chi tiết effort, Đơn giá; tóm tắt trong Proposal mục 5 |
| Nguồn lực | [UniSupport_Masterplan.xlsx](management/UniSupport_Masterplan.xlsx) sheet Resources; vai trò trong [team-charter.md](management/team-charter.md) mục 3 |
| Milestone, phần giao, cách client kiểm tra | Master Plan sheet Milestones; giải thích cách chia ở [master-plan.md](management/master-plan.md) |
| Việc, owner, hạn, phụ thuộc | Master Plan sheet WBS (cột Phụ thuộc là nơi gốc); parent task trên GitHub Issues |
| Rủi ro | [risk-register.md](management/risk-register.md) |
| Change request | Quy trình: [03-quan-tri-giam-sat.md](management/03-quan-tri-giam-sat.md) mục 5; CR thực tế: GitHub Issues nhãn `change-request` (mẫu Change request); phiên bản kế hoạch: sheet Change log |
| Definition of Done | [02-qmp-quan-ly-chat-luong.md](management/02-qmp-quan-ly-chat-luong.md) mục 5 |
| Mức lỗi, điều kiện release | QMP mục 6 và mục 2 |
| Báo cáo, chỉ số, ngưỡng escalate | 03 mục 2, 3, 4 |
| Bảng theo dõi giờ, Decision Log, biên bản họp, GitHub Issues | [management/records/README.md](management/records/README.md) (mẫu & ví dụ `Mô phỏng`: [time-tracking-log.md](management/records/time-tracking-log.md), [decision-log.md](management/records/decision-log.md), [meeting-minutes-template.md](management/records/meeting-minutes-template.md)) và `03` mục 6.1 |
| Feedback, roadmap, chỉ số giá trị, ai quyết sau bàn giao | 01-quan-ly-san-pham mục 5, 8, 6, 7.3 |
| Giả định, ràng buộc | [assumptions.md](project/assumptions.md) (`A-xx`), [constraints.md](project/constraints.md) |

## Đối chiếu cấu trúc thư mục tài liệu

Nhóm giữ tên thư mục gọn cho dự án; bảng dưới ánh xạ sang cấu trúc tài liệu tham chiếu.

| Cấu trúc chuẩn | Vị trí trong repo |
|---|---|
| `01-product/` | `product/` |
| `02-domain/` | Không có thư mục riêng: thuật ngữ ở `project/glossary.md`, mô hình dữ liệu ở `architecture/data-model.md` |
| `03-modules/` | `modules/` |
| `04-workflows/` | Không có thư mục riêng: luồng của từng module ở đầu mỗi `prd.md`, sơ đồ luồng và vòng đời hồ sơ ở `architecture/architecture-overview.md` |
| `05-non-functional-requirements/` | `non-functional-requirements/` |
| `06-acceptance/` | `acceptance/` |
| `07-architecture/` | `architecture/` |
| `08-project/` | `management/` (proposal, báo giá, Master Plan, rủi ro, Team Charter, `sent/`) và `project/` (giả định, ràng buộc) |
| `08-project/open-questions.md` | N/A: điểm còn chờ trường xác nhận ghi ở cột Trạng thái của `project/assumptions.md` |
| `09-quality/quality-plan.md` | `management/02-qmp-quan-ly-chat-luong.md` |
| `10-product-management/` | `management/01-quan-ly-san-pham.md` |
| `11-governance/` | `management/03-quan-tri-giam-sat.md` |

## Quy ước mã

Liên kết chỉ ghi ở một phía: phía ghi là cột "Liên kết phải ghi". Ví dụ kịch bản test ghi mã AC nó kiểm; PRD không ghi ngược lại mã test.

| Đối tượng | Dạng | Nơi gốc | Liên kết phải ghi |
|---|---|---|---|
| Module | `M01` | `modules/` | |
| Yêu cầu chức năng | `FR-STU-02` | `prd.md` của module | |
| Business rule, error flow | `BR-01`, `E-01` (mã cục bộ trong một FR; gọi đầy đủ `FR-STU-02/BR-01`) | `prd.md` của module, trong FR | |
| Tiêu chí nghiệm thu | `FR-STU-02/AC-04` (trong PRD viết `AC-04` dưới FR) | `prd.md` của module | Mã `BR-xx`, `E-xx` mà AC kiểm (cuối dòng AC: "Kiểm cho") |
| Goal, non-goal | `G-01`, `NG-01` | `product/README.md` | |
| Yêu cầu phi chức năng | `NFR-PERF-01` | `non-functional-requirements/` | Cách đo (`TS-xx` hoặc mã AC) |
| Kịch bản test | `TS-01` | `acceptance/test-scenarios.md` | Mã AC hoặc NFR được test |
| Hạng mục báo giá | `Q-01` | `quotation.xlsx` sheet Hạng mục | Module hoặc FR thuộc hạng mục |
| Giả định | `A-01` | `project/assumptions.md` | |
| Milestone | `MS-01` | Master Plan sheet Milestones; GitHub Milestones | Phần giao được, cách client kiểm tra, điều kiện nghiệm thu |
| Dòng WBS | `2.05` | Master Plan sheet WBS | Milestone, `Q-xx`, owner, ngày, phụ thuộc, FR, issue |
| Parent task | `#12` | GitHub Issue | Mã FR, `MS-xx`, owner, hạn, mã WBS, điều kiện hoàn thành |
| Rủi ro | `R-01` | `management/risk-register.md` | Owner, phương án xử lý |
| Change request | `CR-01` | GitHub Issue (mẫu Change request) | Tài liệu và task đã cập nhật |

`M01` là module, `MS-01` là milestone; không dùng lẫn.

## Dữ liệu mô phỏng

| Phần | Mô phỏng? | Ghi chú |
|---|---|---|
| Client Aurora University, hiện trạng của trường | Có | Proposal, `product/README.md` |
| Đội thực thi 10 vai trò | Có | Team Charter mục 3.2, sheet Resources, `Bang_Effort.md` |
| Proposal đã gửi khách | Có | `management/sent/`: bản [v2.0 (06/09/2026)](management/sent/proposal-v2.0-2026-09-06.pdf) đang hiệu lực (Mô phỏng) |
| Feedback, change request | Có khi phát sinh | Ghi `Mô phỏng` trong issue (mẫu Feedback, Change request) |
| Báo cáo, bảng giờ, Decision Log và biên bản ví dụ | Có | Ví dụ báo cáo tuần ở 03 mục 6.2; mẫu & số liệu minh họa ở [management/records/](management/records/README.md) |

## Người chịu trách nhiệm file Word/Excel

| File | Người chịu trách nhiệm chính |
|---|---|
| `management/proposal.docx` (và `sent/`) | Lê Hoàng Tùng |
| `management/quotation.xlsx` | Lê Hoàng Tùng |
| `management/UniSupport_Masterplan.xlsx` | Phan Tiến Thịnh |

Người chịu trách nhiệm các tài liệu Markdown: [team-charter.md](management/team-charter.md) mục 3.1. Lịch sử phiên bản: Proposal có bảng lịch sử phiên bản; báo giá có sheet Phiên bản; Master Plan có sheet Change log; mọi thay đổi khác xem lịch sử PR.
