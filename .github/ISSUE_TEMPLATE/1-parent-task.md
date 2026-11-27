---
name: Parent task / Task công việc
about: Tạo issue kế hoạch (FR cha, sub-issue BE/FE hoặc task 1 cấp) gắn với WBS và milestone
title: "[MS-xx] "
labels: task
---

<!--
QUY ƯỚC NHÃN VÀ CẤP ISSUE (đồng bộ bộ lọc ở docs_v2/management/03-quan-tri-giam-sat.md mục 3.1 để không đếm trùng cha/con):
1. FR Parent task (ví dụ #1): giữ nhãn `task` + thêm `loại: FR` + `cổng: ...` + Milestone `MS-xx`. KHÔNG gắn nhãn `vai trò: ...` (vì đã tách công việc xuống sub-issue BE/FE).
2. Sub-issue [BE] / [FE] (ví dụ #2, #3): GỠ nhãn `task`, gắn `loại: BE` hoặc `loại: FE` + `vai trò: ...` + `cổng: ...` + Milestone `MS-xx`.
3. Task 1 cấp (phân tích, thiết kế, dùng chung, viết test case, test trọn luồng, sửa lỗi, triển khai, bảo hành): giữ nhãn `task` + gắn `vai trò: ...` + Milestone `MS-xx`. KHÔNG gắn `loại: FR`.
4. Việc quản lý xuyên suốt (ví dụ #101, Q-09): giữ nhãn `task` + `loại: quản lý` + `vai trò: PM / Senior`, để trống Milestone và theo dõi tiến độ theo checklist 6 milestone (MS-01 đến MS-06) trong thân issue.
-->

**FR:** <!-- Mã FR cụ thể, ví dụ FR-STU-02 (hoặc "Không gắn FR" với việc quản lý/hạ tầng) -->
**Milestone:** <!-- MS-xx · <tên milestone> (hoặc "Xuyên suốt MS-01 đến MS-06" với #101) -->
**Owner:** <!-- Vai trò ở issue thực thi/1 cấp (khớp cột Owner trong sheet WBS); với FR Parent task ghi "Điều phối theo sub-issue BE/FE (#a, #b)" -->
**Hỗ trợ / duyệt:** <!-- Vai trò hỗ trợ/review, nếu có -->
**Hạn:** <!-- dd/mm/yyyy — khớp ngày Kết thúc của dòng WBS (với FR cha: ≥ max(BE, FE, Phần dùng chung) theo DoD mức chức năng) -->
**WBS:** <!-- Mã dòng WBS, ví dụ 2.02 -->
**Thời gian:** <!-- dd/mm/yyyy → dd/mm/yyyy, theo sheet WBS -->

## Mô tả và dẫn chiếu đặc tả

<!--
QUY ƯỚC LIÊN KẾT ĐẶC TẢ (xem docs_v2/management/master-plan.md mục "Quy ước liên kết từ GitHub Issue đến đặc tả"):
- Mọi FR parent task, issue Dùng chung và sub-issue [BE]/[FE] đều dẫn THẲNG đến heading anchor của FR tương ứng (`#fr-xxx-nn-...`), KHÔNG chỉ trỏ đến đầu file `prd.md`.
- Trước khi chốt phạm vi MS-01: dùng link nhánh `blob/main/...#<anchor>`. Ngay khi chốt baseline (ví dụ tag `baseline-v2.0`): dùng permalink cố định theo tag `blob/baseline-v2.0/...#<anchor>`. Khi có Change Request sửa PRD, cập nhật link sang tag mới tại bước 4 quy trình CR (03 mục 5).
- Issue Dùng chung (#117, #118, #123, #124, #129, #130, #155, #156, #157, #158) bắt buộc liệt kê rõ danh sách AC chịu trách nhiệm ở milestone đó.
- Sub-issue [BE]/[FE] ghi thêm dòng "Issue cha: #<số>" và dẫn thẳng heading FR + phạm vi AC mảng BE/FE.
-->

- **Issue cha (chỉ điền với sub-issue `[BE]`/`[FE]`):** <!-- Ví dụ: #1 -->
- **Đặc tả FR (dẫn thẳng heading anchor):** <!-- Ví dụ: [FR-STU-02 · Tạo yêu cầu hỗ trợ](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-02-tạo-yêu-cầu-hỗ-trợ) -->
- **Danh sách AC áp dụng:** <!-- Ví dụ: FR-STU-02/AC-01 đến FR-STU-02/AC-10 (với issue Dùng chung: liệt kê rõ từng mã AC thuộc milestone này) -->

## Điều kiện hoàn thành

- [ ] Đạt mọi AC đã liệt kê ở trên, kiểm bằng kịch bản `TS-xx` tương ứng
- [ ] Đạt Definition of Done mức task / chức năng (`02-qmp-quan-ly-chat-luong.md` mục 5, `team-charter.md` mục 5 bước 4)
- [ ] Đã cập nhật giờ thực tế vào Bảng theo dõi giờ kèm số issue này

## Sub-issue / Checklist theo milestone (tuỳ chọn)

- [ ]
