# Ma trận truy vết

Bảng **tổng hợp** theo từng AC: AC → kịch bản test → parent task → milestone → hạng mục báo giá. Đây không phải nơi gốc để sửa: sửa ở nơi gốc trước (AC trong `modules/*/prd.md`, kịch bản trong [test-scenarios.md](test-scenarios.md), issue trên GitHub, milestone trong [Master Plan](../management/UniSupport_Masterplan_nhom2.xlsx), hạng mục trong [quotation.xlsx](../management/quotation.xlsx)) rồi mới cập nhật bảng này. Ma trận dùng để đo độ phủ tiêu chí chấp nhận theo QMP mục 7.

**Cách đọc**

- **Test:** kịch bản chức năng của FR (`TS-21` đến `TS-62`) đứng đầu, sau đó là kịch bản trọn luồng hoặc kịch bản chéo cũng kiểm AC này.
- **Parent task:** issue cha của FR trên GitHub. FR dùng chung cho nhiều màn hình (`FR-SEC-04`, `FR-SEC-05`, `FR-SEC-06`, `FR-CORE-06`, `FR-CORE-08`, `FR-CORE-09`) được làm dần qua các issue "Dùng chung" của từng milestone.
- **Báo giá:** hạng mục `Q-xx` chứa milestone của FR; `Q-xx` ghi danh sách FR ở sheet Hạng mục.
- Trạng thái chạy test ghi trong báo cáo kiểm thử, không ghi ở đây.

**Tổng hợp:** 42 FR, 339 AC; 339/339 AC có ít nhất một kịch bản test, một parent task, một milestone và một hạng mục báo giá. Mỗi business rule (`BR-xx`) và error flow (`E-xx`) của FR có ít nhất một AC kiểm nó (ghi ở cuối dòng AC trong `prd.md`, phần "Kiểm cho").

## Cổng Sinh viên (`modules/M01-student-portal`)

| AC | Test | Parent task | Milestone | Báo giá |
|---|---|---|---|---|
| `FR-STU-01/AC-01` | `TS-21`, `TS-01` | #30 | MS-01 | Q-02 |
| `FR-STU-01/AC-02` | `TS-21` | #30 | MS-01 | Q-02 |
| `FR-STU-01/AC-03` | `TS-21` | #30 | MS-01 | Q-02 |
| `FR-STU-01/AC-04` | `TS-21` | #30 | MS-01 | Q-02 |
| `FR-STU-01/AC-05` | `TS-21` | #30 | MS-01 | Q-02 |
| `FR-STU-01/AC-06` | `TS-21` | #30 | MS-01 | Q-02 |
| `FR-STU-01/AC-07` | `TS-21` | #30 | MS-01 | Q-02 |
| `FR-STU-02/AC-01` | `TS-22`, `TS-02` | #1 | MS-02 | Q-03 |
| `FR-STU-02/AC-02` | `TS-22` | #1 | MS-02 | Q-03 |
| `FR-STU-02/AC-03` | `TS-22` | #1 | MS-02 | Q-03 |
| `FR-STU-02/AC-04` | `TS-22`, `TS-11` | #1 | MS-02 | Q-03 |
| `FR-STU-02/AC-05` | `TS-22`, `TS-18` | #1 | MS-02 | Q-03 |
| `FR-STU-02/AC-06` | `TS-22` | #1 | MS-02 | Q-03 |
| `FR-STU-02/AC-07` | `TS-22` | #1 | MS-02 | Q-03 |
| `FR-STU-02/AC-08` | `TS-22` | #1 | MS-02 | Q-03 |
| `FR-STU-02/AC-09` | `TS-22` | #1 | MS-02 | Q-03 |
| `FR-STU-02/AC-10` | `TS-22` | #1 | MS-02 | Q-03 |
| `FR-STU-03/AC-01` | `TS-23`, `TS-02` | #48 | MS-02 | Q-03 |
| `FR-STU-03/AC-02` | `TS-23` | #48 | MS-02 | Q-03 |
| `FR-STU-03/AC-03` | `TS-23` | #48 | MS-02 | Q-03 |
| `FR-STU-03/AC-04` | `TS-23`, `TS-18` | #48 | MS-02 | Q-03 |
| `FR-STU-03/AC-05` | `TS-23` | #48 | MS-02 | Q-03 |
| `FR-STU-03/AC-06` | `TS-23` | #48 | MS-02 | Q-03 |
| `FR-STU-03/AC-07` | `TS-23` | #48 | MS-02 | Q-03 |
| `FR-STU-04/AC-01` | `TS-24`, `TS-02` | #19 | MS-02 | Q-03 |
| `FR-STU-04/AC-02` | `TS-24` | #19 | MS-02 | Q-03 |
| `FR-STU-04/AC-03` | `TS-24` | #19 | MS-02 | Q-03 |
| `FR-STU-04/AC-04` | `TS-24` | #19 | MS-02 | Q-03 |
| `FR-STU-04/AC-05` | `TS-24` | #19 | MS-02 | Q-03 |
| `FR-STU-04/AC-06` | `TS-24`, `TS-12` | #19 | MS-02 | Q-03 |
| `FR-STU-04/AC-07` | `TS-24` | #19 | MS-02 | Q-03 |
| `FR-STU-04/AC-08` | `TS-24` | #19 | MS-02 | Q-03 |
| `FR-STU-04/AC-09` | `TS-24` | #19 | MS-02 | Q-03 |
| `FR-STU-05/AC-01` | `TS-25`, `TS-02` | #33 | MS-02 | Q-03 |
| `FR-STU-05/AC-02` | `TS-25` | #33 | MS-02 | Q-03 |
| `FR-STU-05/AC-03` | `TS-25` | #33 | MS-02 | Q-03 |
| `FR-STU-05/AC-04` | `TS-25` | #33 | MS-02 | Q-03 |
| `FR-STU-05/AC-05` | `TS-25` | #33 | MS-02 | Q-03 |
| `FR-STU-05/AC-06` | `TS-25` | #33 | MS-02 | Q-03 |
| `FR-STU-05/AC-07` | `TS-25` | #33 | MS-02 | Q-03 |
| `FR-STU-06/AC-01` | `TS-26`, `TS-02`, `TS-03` | #36 | MS-02 | Q-03 |
| `FR-STU-06/AC-02` | `TS-26` | #36 | MS-02 | Q-03 |
| `FR-STU-06/AC-03` | `TS-26` | #36 | MS-02 | Q-03 |
| `FR-STU-06/AC-04` | `TS-26` | #36 | MS-02 | Q-03 |
| `FR-STU-06/AC-05` | `TS-26`, `TS-18` | #36 | MS-02 | Q-03 |
| `FR-STU-06/AC-06` | `TS-26` | #36 | MS-02 | Q-03 |
| `FR-STU-06/AC-07` | `TS-26` | #36 | MS-02 | Q-03 |
| `FR-STU-06/AC-08` | `TS-26` | #36 | MS-02 | Q-03 |
| `FR-STU-07/AC-01` | `TS-27`, `TS-07` | #39 | MS-04 | Q-05 |
| `FR-STU-07/AC-02` | `TS-27` | #39 | MS-04 | Q-05 |
| `FR-STU-07/AC-03` | `TS-27` | #39 | MS-04 | Q-05 |
| `FR-STU-07/AC-04` | `TS-27` | #39 | MS-04 | Q-05 |
| `FR-STU-07/AC-05` | `TS-27` | #39 | MS-04 | Q-05 |
| `FR-STU-07/AC-06` | `TS-27` | #39 | MS-04 | Q-05 |
| `FR-STU-07/AC-07` | `TS-27` | #39 | MS-04 | Q-05 |
| `FR-STU-07/AC-08` | `TS-27` | #39 | MS-04 | Q-05 |
| `FR-STU-08/AC-01` | `TS-28`, `TS-04` | #16 | MS-02 | Q-03 |
| `FR-STU-08/AC-02` | `TS-28`, `TS-04` | #16 | MS-02 | Q-03 |
| `FR-STU-08/AC-03` | `TS-28` | #16 | MS-02 | Q-03 |
| `FR-STU-08/AC-04` | `TS-28` | #16 | MS-02 | Q-03 |
| `FR-STU-08/AC-05` | `TS-28`, `TS-03` | #16 | MS-02 | Q-03 |
| `FR-STU-08/AC-06` | `TS-28` | #16 | MS-02 | Q-03 |
| `FR-STU-08/AC-07` | `TS-28`, `TS-03` | #16 | MS-02 | Q-03 |
| `FR-STU-08/AC-08` | `TS-28`, `TS-03`, `TS-06`, `TS-07` | #16 | MS-02 | Q-03 |
| `FR-STU-08/AC-09` | `TS-28`, `TS-03` | #16 | MS-02 | Q-03 |
| `FR-STU-08/AC-10` | `TS-28`, `TS-03` | #16 | MS-02 | Q-03 |
| `FR-STU-09/AC-01` | `TS-29`, `TS-04` | #42 | MS-03 | Q-04 |
| `FR-STU-09/AC-02` | `TS-29` | #42 | MS-03 | Q-04 |
| `FR-STU-09/AC-03` | `TS-29` | #42 | MS-03 | Q-04 |
| `FR-STU-09/AC-04` | `TS-29` | #42 | MS-03 | Q-04 |
| `FR-STU-09/AC-05` | `TS-29`, `TS-10`, `TS-11` | #42 | MS-03 | Q-04 |
| `FR-STU-09/AC-06` | `TS-29` | #42 | MS-03 | Q-04 |
| `FR-STU-09/AC-07` | `TS-29` | #42 | MS-03 | Q-04 |
| `FR-STU-09/AC-08` | `TS-29` | #42 | MS-03 | Q-04 |
| `FR-STU-09/AC-09` | `TS-29` | #42 | MS-03 | Q-04 |
| `FR-STU-09/AC-10` | `TS-29` | #42 | MS-03 | Q-04 |
| `FR-STU-10/AC-01` | `TS-30`, `TS-07` | #45 | MS-04 | Q-05 |
| `FR-STU-10/AC-02` | `TS-30` | #45 | MS-04 | Q-05 |
| `FR-STU-10/AC-03` | `TS-30` | #45 | MS-04 | Q-05 |
| `FR-STU-10/AC-04` | `TS-30`, `TS-07` | #45 | MS-04 | Q-05 |
| `FR-STU-10/AC-05` | `TS-30`, `TS-11` | #45 | MS-04 | Q-05 |
| `FR-STU-10/AC-06` | `TS-30` | #45 | MS-04 | Q-05 |
| `FR-STU-10/AC-07` | `TS-30` | #45 | MS-04 | Q-05 |
| `FR-STU-10/AC-08` | `TS-30` | #45 | MS-04 | Q-05 |

## Cổng Nhân viên (`modules/M02-staff-portal`)

| AC | Test | Parent task | Milestone | Báo giá |
|---|---|---|---|---|
| `FR-STF-01/AC-01` | `TS-31`, `TS-02` | #50 | MS-02 | Q-03 |
| `FR-STF-01/AC-02` | `TS-31` | #50 | MS-02 | Q-03 |
| `FR-STF-01/AC-03` | `TS-31` | #50 | MS-02 | Q-03 |
| `FR-STF-01/AC-04` | `TS-31` | #50 | MS-02 | Q-03 |
| `FR-STF-01/AC-05` | `TS-31`, `TS-18` | #50 | MS-02 | Q-03 |
| `FR-STF-01/AC-06` | `TS-31` | #50 | MS-02 | Q-03 |
| `FR-STF-01/AC-07` | `TS-31` | #50 | MS-02 | Q-03 |
| `FR-STF-01/AC-08` | `TS-31` | #50 | MS-02 | Q-03 |
| `FR-STF-01/AC-09` | `TS-31` | #50 | MS-02 | Q-03 |
| `FR-STF-02/AC-01` | `TS-32`, `TS-02` | #68 | MS-02 | Q-03 |
| `FR-STF-02/AC-02` | `TS-32` | #68 | MS-02 | Q-03 |
| `FR-STF-02/AC-03` | `TS-32` | #68 | MS-02 | Q-03 |
| `FR-STF-02/AC-04` | `TS-32` | #68 | MS-02 | Q-03 |
| `FR-STF-02/AC-05` | `TS-32` | #68 | MS-02 | Q-03 |
| `FR-STF-02/AC-06` | `TS-32` | #68 | MS-02 | Q-03 |
| `FR-STF-02/AC-07` | `TS-32` | #68 | MS-02 | Q-03 |
| `FR-STF-03/AC-01` | `TS-33`, `TS-02` | #65 | MS-02 | Q-03 |
| `FR-STF-03/AC-02` | `TS-33` | #65 | MS-02 | Q-03 |
| `FR-STF-03/AC-03` | `TS-33` | #65 | MS-02 | Q-03 |
| `FR-STF-03/AC-04` | `TS-33`, `TS-18` | #65 | MS-02 | Q-03 |
| `FR-STF-03/AC-05` | `TS-33` | #65 | MS-02 | Q-03 |
| `FR-STF-03/AC-06` | `TS-33`, `TS-12` | #65 | MS-02 | Q-03 |
| `FR-STF-03/AC-07` | `TS-33` | #65 | MS-02 | Q-03 |
| `FR-STF-03/AC-08` | `TS-33` | #65 | MS-02 | Q-03 |
| `FR-STF-04/AC-01` | `TS-34`, `TS-04` | #71 | MS-03 | Q-04 |
| `FR-STF-04/AC-02` | `TS-34` | #71 | MS-03 | Q-04 |
| `FR-STF-04/AC-03` | `TS-34` | #71 | MS-03 | Q-04 |
| `FR-STF-04/AC-04` | `TS-34`, `TS-11` | #71 | MS-03 | Q-04 |
| `FR-STF-04/AC-05` | `TS-34`, `TS-04` | #71 | MS-03 | Q-04 |
| `FR-STF-04/AC-06` | `TS-34` | #71 | MS-03 | Q-04 |
| `FR-STF-04/AC-07` | `TS-34` | #71 | MS-03 | Q-04 |
| `FR-STF-04/AC-08` | `TS-34` | #71 | MS-03 | Q-04 |
| `FR-STF-04/AC-09` | `TS-34` | #71 | MS-03 | Q-04 |
| `FR-STF-05/AC-01` | `TS-35`, `TS-04` | #56 | MS-03 | Q-04 |
| `FR-STF-05/AC-02` | `TS-35` | #56 | MS-03 | Q-04 |
| `FR-STF-05/AC-03` | `TS-35` | #56 | MS-03 | Q-04 |
| `FR-STF-05/AC-04` | `TS-35` | #56 | MS-03 | Q-04 |
| `FR-STF-05/AC-05` | `TS-35`, `TS-11` | #56 | MS-03 | Q-04 |
| `FR-STF-05/AC-06` | `TS-35` | #56 | MS-03 | Q-04 |
| `FR-STF-05/AC-07` | `TS-35` | #56 | MS-03 | Q-04 |
| `FR-STF-05/AC-08` | `TS-35` | #56 | MS-03 | Q-04 |
| `FR-STF-06/AC-01` | `TS-36`, `TS-07`, `TS-11` | #59 | MS-04 | Q-05 |
| `FR-STF-06/AC-02` | `TS-36` | #59 | MS-04 | Q-05 |
| `FR-STF-06/AC-03` | `TS-36` | #59 | MS-04 | Q-05 |
| `FR-STF-06/AC-04` | `TS-36` | #59 | MS-04 | Q-05 |
| `FR-STF-06/AC-05` | `TS-36`, `TS-10` | #59 | MS-04 | Q-05 |
| `FR-STF-06/AC-06` | `TS-36` | #59 | MS-04 | Q-05 |
| `FR-STF-06/AC-07` | `TS-36` | #59 | MS-04 | Q-05 |
| `FR-STF-06/AC-08` | `TS-36` | #59 | MS-04 | Q-05 |
| `FR-STF-07/AC-01` | `TS-37`, `TS-04`, `TS-06` | #53 | MS-03 | Q-04 |
| `FR-STF-07/AC-02` | `TS-37` | #53 | MS-03 | Q-04 |
| `FR-STF-07/AC-03` | `TS-37` | #53 | MS-03 | Q-04 |
| `FR-STF-07/AC-04` | `TS-37` | #53 | MS-03 | Q-04 |
| `FR-STF-07/AC-05` | `TS-37`, `TS-10` | #53 | MS-03 | Q-04 |
| `FR-STF-07/AC-06` | `TS-37` | #53 | MS-03 | Q-04 |
| `FR-STF-07/AC-07` | `TS-37` | #53 | MS-03 | Q-04 |
| `FR-STF-07/AC-08` | `TS-37` | #53 | MS-03 | Q-04 |
| `FR-STF-08/AC-01` | `TS-38`, `TS-05` | #74 | MS-03 | Q-04 |
| `FR-STF-08/AC-02` | `TS-38` | #74 | MS-03 | Q-04 |
| `FR-STF-08/AC-03` | `TS-38` | #74 | MS-03 | Q-04 |
| `FR-STF-08/AC-04` | `TS-38` | #74 | MS-03 | Q-04 |
| `FR-STF-08/AC-05` | `TS-38` | #74 | MS-03 | Q-04 |
| `FR-STF-08/AC-06` | `TS-38` | #74 | MS-03 | Q-04 |
| `FR-STF-08/AC-07` | `TS-38` | #74 | MS-03 | Q-04 |
| `FR-STF-09/AC-01` | `TS-39`, `TS-05` | #62 | MS-03 | Q-04 |
| `FR-STF-09/AC-02` | `TS-39` | #62 | MS-03 | Q-04 |
| `FR-STF-09/AC-03` | `TS-39` | #62 | MS-03 | Q-04 |
| `FR-STF-09/AC-04` | `TS-39`, `TS-10` | #62 | MS-03 | Q-04 |
| `FR-STF-09/AC-05` | `TS-39` | #62 | MS-03 | Q-04 |
| `FR-STF-09/AC-06` | `TS-39` | #62 | MS-03 | Q-04 |
| `FR-STF-09/AC-07` | `TS-39` | #62 | MS-03 | Q-04 |
| `FR-STF-09/AC-08` | `TS-39` | #62 | MS-03 | Q-04 |
| `FR-STF-10/AC-01` | `TS-40`, `TS-05` | #77 | MS-03 | Q-04 |
| `FR-STF-10/AC-02` | `TS-40` | #77 | MS-03 | Q-04 |
| `FR-STF-10/AC-03` | `TS-40` | #77 | MS-03 | Q-04 |
| `FR-STF-10/AC-04` | `TS-40` | #77 | MS-03 | Q-04 |
| `FR-STF-10/AC-05` | `TS-40` | #77 | MS-03 | Q-04 |
| `FR-STF-10/AC-06` | `TS-40` | #77 | MS-03 | Q-04 |
| `FR-STF-10/AC-07` | `TS-40` | #77 | MS-03 | Q-04 |

## Cổng Admin (`modules/M03-management-portal`)

| AC | Test | Parent task | Milestone | Báo giá |
|---|---|---|---|---|
| `FR-MGT-01/AC-01` | `TS-41`, `TS-08` | #86 | MS-05 | Q-06 |
| `FR-MGT-01/AC-02` | `TS-41` | #86 | MS-05 | Q-06 |
| `FR-MGT-01/AC-03` | `TS-41` | #86 | MS-05 | Q-06 |
| `FR-MGT-01/AC-04` | `TS-41` | #86 | MS-05 | Q-06 |
| `FR-MGT-01/AC-05` | `TS-41`, `TS-18` | #86 | MS-05 | Q-06 |
| `FR-MGT-01/AC-06` | `TS-41` | #86 | MS-05 | Q-06 |
| `FR-MGT-01/AC-07` | `TS-41` | #86 | MS-05 | Q-06 |
| `FR-MGT-01/AC-08` | `TS-41` | #86 | MS-05 | Q-06 |
| `FR-MGT-02/AC-01` | `TS-42`, `TS-08` | #89 | MS-05 | Q-06 |
| `FR-MGT-02/AC-02` | `TS-42`, `TS-08` | #89 | MS-05 | Q-06 |
| `FR-MGT-02/AC-03` | `TS-42`, `TS-17` | #89 | MS-05 | Q-06 |
| `FR-MGT-02/AC-04` | `TS-42` | #89 | MS-05 | Q-06 |
| `FR-MGT-02/AC-05` | `TS-42`, `TS-18` | #89 | MS-05 | Q-06 |
| `FR-MGT-02/AC-06` | `TS-42` | #89 | MS-05 | Q-06 |
| `FR-MGT-02/AC-07` | `TS-42` | #89 | MS-05 | Q-06 |
| `FR-MGT-02/AC-08` | `TS-42` | #89 | MS-05 | Q-06 |
| `FR-MGT-03/AC-01` | `TS-43`, `TS-08` | #95 | MS-05 | Q-06 |
| `FR-MGT-03/AC-02` | `TS-43` | #95 | MS-05 | Q-06 |
| `FR-MGT-03/AC-03` | `TS-43`, `TS-17` | #95 | MS-05 | Q-06 |
| `FR-MGT-03/AC-04` | `TS-43` | #95 | MS-05 | Q-06 |
| `FR-MGT-03/AC-05` | `TS-43`, `TS-18` | #95 | MS-05 | Q-06 |
| `FR-MGT-03/AC-06` | `TS-43` | #95 | MS-05 | Q-06 |
| `FR-MGT-03/AC-07` | `TS-43` | #95 | MS-05 | Q-06 |
| `FR-MGT-03/AC-08` | `TS-43` | #95 | MS-05 | Q-06 |
| `FR-MGT-04/AC-01` | `TS-44`, `TS-08` | #98 | MS-05 | Q-06 |
| `FR-MGT-04/AC-02` | `TS-44` | #98 | MS-05 | Q-06 |
| `FR-MGT-04/AC-03` | `TS-44`, `TS-17` | #98 | MS-05 | Q-06 |
| `FR-MGT-04/AC-04` | `TS-44` | #98 | MS-05 | Q-06 |
| `FR-MGT-04/AC-05` | `TS-44`, `TS-18` | #98 | MS-05 | Q-06 |
| `FR-MGT-04/AC-06` | `TS-44` | #98 | MS-05 | Q-06 |
| `FR-MGT-04/AC-07` | `TS-44` | #98 | MS-05 | Q-06 |
| `FR-MGT-05/AC-01` | `TS-45`, `TS-08` | #92 | MS-05 | Q-06 |
| `FR-MGT-05/AC-02` | `TS-45` | #92 | MS-05 | Q-06 |
| `FR-MGT-05/AC-03` | `TS-45`, `TS-17` | #92 | MS-05 | Q-06 |
| `FR-MGT-05/AC-04` | `TS-45` | #92 | MS-05 | Q-06 |
| `FR-MGT-05/AC-05` | `TS-45`, `TS-18` | #92 | MS-05 | Q-06 |
| `FR-MGT-05/AC-06` | `TS-45` | #92 | MS-05 | Q-06 |
| `FR-MGT-05/AC-07` | `TS-45` | #92 | MS-05 | Q-06 |
| `FR-MGT-05/AC-08` | `TS-45` | #92 | MS-05 | Q-06 |
| `FR-MGT-06/AC-01` | `TS-46`, `TS-01` | #80 | MS-01 | Q-02 |
| `FR-MGT-06/AC-02` | `TS-46` | #80 | MS-01 | Q-02 |
| `FR-MGT-06/AC-03` | `TS-46`, `TS-01` | #80 | MS-01 | Q-02 |
| `FR-MGT-06/AC-04` | `TS-46` | #80 | MS-01 | Q-02 |
| `FR-MGT-06/AC-05` | `TS-46`, `TS-10` | #80 | MS-01 | Q-02 |
| `FR-MGT-06/AC-06` | `TS-46` | #80 | MS-01 | Q-02 |
| `FR-MGT-06/AC-07` | `TS-46` | #80 | MS-01 | Q-02 |
| `FR-MGT-06/AC-08` | `TS-46` | #80 | MS-01 | Q-02 |
| `FR-MGT-06/AC-09` | `TS-46` | #80 | MS-01 | Q-02 |
| `FR-MGT-07/AC-01` | `TS-47`, `TS-02`, `TS-16` | #83 | MS-02 | Q-03 |
| `FR-MGT-07/AC-02` | `TS-47`, `TS-16` | #83 | MS-02 | Q-03 |
| `FR-MGT-07/AC-03` | `TS-47`, `TS-02`, `TS-16` | #83 | MS-02 | Q-03 |
| `FR-MGT-07/AC-04` | `TS-47`, `TS-16` | #83 | MS-02 | Q-03 |
| `FR-MGT-07/AC-05` | `TS-47`, `TS-10`, `TS-16` | #83 | MS-02 | Q-03 |
| `FR-MGT-07/AC-06` | `TS-47`, `TS-16` | #83 | MS-02 | Q-03 |
| `FR-MGT-07/AC-07` | `TS-47` | #83 | MS-02 | Q-03 |
| `FR-MGT-07/AC-08` | `TS-47` | #83 | MS-02 | Q-03 |

## Xác thực và bảo mật (`modules/M04-authentication-security`)

| AC | Test | Parent task | Milestone | Báo giá |
|---|---|---|---|---|
| `FR-SEC-01/AC-01` | `TS-48`, `TS-01` | #4 | MS-01 | Q-02 |
| `FR-SEC-01/AC-02` | `TS-48` | #4 | MS-01 | Q-02 |
| `FR-SEC-01/AC-03` | `TS-48` | #4 | MS-01 | Q-02 |
| `FR-SEC-01/AC-04` | `TS-48`, `TS-14` | #4 | MS-01 | Q-02 |
| `FR-SEC-01/AC-05` | `TS-48`, `TS-18` | #4 | MS-01 | Q-02 |
| `FR-SEC-01/AC-06` | `TS-48`, `TS-14` | #4 | MS-01 | Q-02 |
| `FR-SEC-01/AC-07` | `TS-48` | #4 | MS-01 | Q-02 |
| `FR-SEC-01/AC-08` | `TS-48` | #4 | MS-01 | Q-02 |
| `FR-SEC-01/AC-09` | `TS-48` | #4 | MS-01 | Q-02 |
| `FR-SEC-02/AC-01` | `TS-49`, `TS-01` | #7 | MS-01 | Q-02 |
| `FR-SEC-02/AC-02` | `TS-49` | #7 | MS-01 | Q-02 |
| `FR-SEC-02/AC-03` | `TS-49`, `TS-14` | #7 | MS-01 | Q-02 |
| `FR-SEC-02/AC-04` | `TS-49` | #7 | MS-01 | Q-02 |
| `FR-SEC-02/AC-05` | `TS-49` | #7 | MS-01 | Q-02 |
| `FR-SEC-02/AC-06` | `TS-49`, `TS-14` | #7 | MS-01 | Q-02 |
| `FR-SEC-02/AC-07` | `TS-49` | #7 | MS-01 | Q-02 |
| `FR-SEC-03/AC-01` | `TS-50`, `TS-01` | #10 | MS-01 | Q-02 |
| `FR-SEC-03/AC-02` | `TS-50` | #10 | MS-01 | Q-02 |
| `FR-SEC-03/AC-03` | `TS-50` | #10 | MS-01 | Q-02 |
| `FR-SEC-03/AC-04` | `TS-50`, `TS-09`, `TS-14` | #10 | MS-01 | Q-02 |
| `FR-SEC-03/AC-05` | `TS-50` | #10 | MS-01 | Q-02 |
| `FR-SEC-03/AC-06` | `TS-50`, `TS-14` | #10 | MS-01 | Q-02 |
| `FR-SEC-03/AC-07` | `TS-50` | #10 | MS-01 | Q-02 |
| `FR-SEC-03/AC-08` | `TS-50` | #10 | MS-01 | Q-02 |
| `FR-SEC-04/AC-01` | `TS-51`, `TS-08` | #117, #123, #129, #155, #157 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-SEC-04/AC-02` | `TS-51` | #117, #123, #129, #155, #157 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-SEC-04/AC-03` | `TS-51`, `TS-07` | #117, #123, #129, #155, #157 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-SEC-04/AC-04` | `TS-51`, `TS-09` | #117, #123, #129, #155, #157 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-SEC-04/AC-05` | `TS-51`, `TS-05` | #117, #123, #129, #155, #157 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-SEC-04/AC-06` | `TS-51` | #117, #123, #129, #155, #157 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-SEC-04/AC-07` | `TS-51` | #117, #123, #129, #155, #157 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-SEC-04/AC-08` | `TS-51` | #117, #123, #129, #155, #157 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-SEC-05/AC-01` | `TS-52` | #123 | MS-02 | Q-03 |
| `FR-SEC-05/AC-02` | `TS-52` | #123 | MS-02 | Q-03 |
| `FR-SEC-05/AC-03` | `TS-52`, `TS-09` | #123 | MS-02 | Q-03 |
| `FR-SEC-05/AC-04` | `TS-52` | #123 | MS-02 | Q-03 |
| `FR-SEC-05/AC-05` | `TS-52` | #123 | MS-02 | Q-03 |
| `FR-SEC-05/AC-06` | `TS-52`, `TS-12` | #123 | MS-02 | Q-03 |
| `FR-SEC-05/AC-07` | `TS-52` | #123 | MS-02 | Q-03 |
| `FR-SEC-06/AC-01` | `TS-53`, `TS-13` | #117, #123, #129, #155 | MS-01, MS-02, MS-03, MS-04 | Q-02, Q-03, Q-04, Q-05 |
| `FR-SEC-06/AC-02` | `TS-53`, `TS-13` | #117, #123, #129, #155 | MS-01, MS-02, MS-03, MS-04 | Q-02, Q-03, Q-04, Q-05 |
| `FR-SEC-06/AC-03` | `TS-53`, `TS-13` | #117, #123, #129, #155 | MS-01, MS-02, MS-03, MS-04 | Q-02, Q-03, Q-04, Q-05 |
| `FR-SEC-06/AC-04` | `TS-53`, `TS-13` | #117, #123, #129, #155 | MS-01, MS-02, MS-03, MS-04 | Q-02, Q-03, Q-04, Q-05 |
| `FR-SEC-06/AC-05` | `TS-53`, `TS-13` | #117, #123, #129, #155 | MS-01, MS-02, MS-03, MS-04 | Q-02, Q-03, Q-04, Q-05 |
| `FR-SEC-06/AC-06` | `TS-53`, `TS-13` | #117, #123, #129, #155 | MS-01, MS-02, MS-03, MS-04 | Q-02, Q-03, Q-04, Q-05 |
| `FR-SEC-06/AC-07` | `TS-53` | #117, #123, #129, #155 | MS-01, MS-02, MS-03, MS-04 | Q-02, Q-03, Q-04, Q-05 |
| `FR-SEC-06/AC-08` | `TS-53` | #117, #123, #129, #155 | MS-01, MS-02, MS-03, MS-04 | Q-02, Q-03, Q-04, Q-05 |
| `FR-SEC-06/AC-09` | `TS-53` | #117, #123, #129, #155 | MS-01, MS-02, MS-03, MS-04 | Q-02, Q-03, Q-04, Q-05 |

## Lõi hệ thống (`modules/M05-core-platform`)

| AC | Test | Parent task | Milestone | Báo giá |
|---|---|---|---|---|
| `FR-CORE-01/AC-01` | `TS-54`, `TS-02` | #24 | MS-02 | Q-03 |
| `FR-CORE-01/AC-02` | `TS-54` | #24 | MS-02 | Q-03 |
| `FR-CORE-01/AC-03` | `TS-54`, `TS-17` | #24 | MS-02 | Q-03 |
| `FR-CORE-01/AC-04` | `TS-54` | #24 | MS-02 | Q-03 |
| `FR-CORE-01/AC-05` | `TS-54`, `TS-10` | #24 | MS-02 | Q-03 |
| `FR-CORE-01/AC-06` | `TS-54` | #24 | MS-02 | Q-03 |
| `FR-CORE-01/AC-07` | `TS-54` | #24 | MS-02 | Q-03 |
| `FR-CORE-01/AC-08` | `TS-54` | #24 | MS-02 | Q-03 |
| `FR-CORE-02/AC-01` | `TS-55`, `TS-03`, `TS-06` | #12 | MS-02 | Q-03 |
| `FR-CORE-02/AC-02` | `TS-55` | #12 | MS-02 | Q-03 |
| `FR-CORE-02/AC-03` | `TS-55` | #12 | MS-02 | Q-03 |
| `FR-CORE-02/AC-04` | `TS-55` | #12 | MS-02 | Q-03 |
| `FR-CORE-02/AC-05` | `TS-55`, `TS-10` | #12 | MS-02 | Q-03 |
| `FR-CORE-02/AC-06` | `TS-55` | #12 | MS-02 | Q-03 |
| `FR-CORE-02/AC-07` | `TS-55` | #12 | MS-02 | Q-03 |
| `FR-CORE-02/AC-08` | `TS-55` | #12 | MS-02 | Q-03 |
| `FR-CORE-03/AC-01` | `TS-56`, `TS-02`, `TS-03`, `TS-04`, `TS-06` | #14 | MS-02 | Q-03 |
| `FR-CORE-03/AC-02` | `TS-56`, `TS-02`, `TS-04`, `TS-06` | #14 | MS-02 | Q-03 |
| `FR-CORE-03/AC-03` | `TS-56`, `TS-17` | #14 | MS-02 | Q-03 |
| `FR-CORE-03/AC-04` | `TS-56` | #14 | MS-02 | Q-03 |
| `FR-CORE-03/AC-05` | `TS-56` | #14 | MS-02 | Q-03 |
| `FR-CORE-03/AC-06` | `TS-56` | #14 | MS-02 | Q-03 |
| `FR-CORE-03/AC-07` | `TS-56` | #14 | MS-02 | Q-03 |
| `FR-CORE-03/AC-08` | `TS-56` | #14 | MS-02 | Q-03 |
| `FR-CORE-03/AC-09` | `TS-56` | #14 | MS-02 | Q-03 |
| `FR-CORE-04/AC-01` | `TS-57`, `TS-02`, `TS-05` | #26 | MS-02 | Q-03 |
| `FR-CORE-04/AC-02` | `TS-57` | #26 | MS-02 | Q-03 |
| `FR-CORE-04/AC-03` | `TS-57` | #26 | MS-02 | Q-03 |
| `FR-CORE-04/AC-04` | `TS-57` | #26 | MS-02 | Q-03 |
| `FR-CORE-04/AC-05` | `TS-57` | #26 | MS-02 | Q-03 |
| `FR-CORE-04/AC-06` | `TS-57` | #26 | MS-02 | Q-03 |
| `FR-CORE-04/AC-07` | `TS-57` | #26 | MS-02 | Q-03 |
| `FR-CORE-05/AC-01` | `TS-58`, `TS-02`, `TS-16` | #22 | MS-02 | Q-03 |
| `FR-CORE-05/AC-02` | `TS-58`, `TS-16` | #22 | MS-02 | Q-03 |
| `FR-CORE-05/AC-03` | `TS-58`, `TS-16` | #22 | MS-02 | Q-03 |
| `FR-CORE-05/AC-04` | `TS-58`, `TS-16` | #22 | MS-02 | Q-03 |
| `FR-CORE-05/AC-05` | `TS-58`, `TS-16` | #22 | MS-02 | Q-03 |
| `FR-CORE-05/AC-06` | `TS-58`, `TS-16` | #22 | MS-02 | Q-03 |
| `FR-CORE-05/AC-07` | `TS-58` | #22 | MS-02 | Q-03 |
| `FR-CORE-05/AC-08` | `TS-58` | #22 | MS-02 | Q-03 |
| `FR-CORE-06/AC-01` | `TS-59`, `TS-07`, `TS-13` | #123, #129, #155 | MS-02, MS-03, MS-04 | Q-03, Q-04, Q-05 |
| `FR-CORE-06/AC-02` | `TS-59`, `TS-04`, `TS-13` | #123, #129, #155 | MS-02, MS-03, MS-04 | Q-03, Q-04, Q-05 |
| `FR-CORE-06/AC-03` | `TS-59`, `TS-03`, `TS-13` | #123, #129, #155 | MS-02, MS-03, MS-04 | Q-03, Q-04, Q-05 |
| `FR-CORE-06/AC-04` | `TS-59`, `TS-05`, `TS-13` | #123, #129, #155 | MS-02, MS-03, MS-04 | Q-03, Q-04, Q-05 |
| `FR-CORE-06/AC-05` | `TS-59`, `TS-13` | #123, #129, #155 | MS-02, MS-03, MS-04 | Q-03, Q-04, Q-05 |
| `FR-CORE-06/AC-06` | `TS-59`, `TS-13` | #123, #129, #155 | MS-02, MS-03, MS-04 | Q-03, Q-04, Q-05 |
| `FR-CORE-06/AC-07` | `TS-59` | #123, #129, #155 | MS-02, MS-03, MS-04 | Q-03, Q-04, Q-05 |
| `FR-CORE-06/AC-08` | `TS-59` | #123, #129, #155 | MS-02, MS-03, MS-04 | Q-03, Q-04, Q-05 |
| `FR-CORE-07/AC-01` | `TS-60`, `TS-02` | #28 | MS-02 | Q-03 |
| `FR-CORE-07/AC-02` | `TS-60` | #28 | MS-02 | Q-03 |
| `FR-CORE-07/AC-03` | `TS-60` | #28 | MS-02 | Q-03 |
| `FR-CORE-07/AC-04` | `TS-60` | #28 | MS-02 | Q-03 |
| `FR-CORE-07/AC-05` | `TS-60` | #28 | MS-02 | Q-03 |
| `FR-CORE-07/AC-06` | `TS-60`, `TS-12` | #28 | MS-02 | Q-03 |
| `FR-CORE-07/AC-07` | `TS-60` | #28 | MS-02 | Q-03 |
| `FR-CORE-07/AC-08` | `TS-60` | #28 | MS-02 | Q-03 |
| `FR-CORE-08/AC-01` | `TS-61`, `TS-03`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-08/AC-02` | `TS-61`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-08/AC-03` | `TS-61`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-08/AC-04` | `TS-61`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-08/AC-05` | `TS-61`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-08/AC-06` | `TS-61`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-08/AC-07` | `TS-61` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-08/AC-08` | `TS-61`, `TS-03` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-09/AC-01` | `TS-62`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-09/AC-02` | `TS-62`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-09/AC-03` | `TS-62`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-09/AC-04` | `TS-62`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-09/AC-05` | `TS-62`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-09/AC-06` | `TS-62`, `TS-15` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-09/AC-07` | `TS-62` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
| `FR-CORE-09/AC-08` | `TS-62` | #118, #124, #130, #156, #158 | MS-01, MS-02, MS-03, MS-04, MS-05 | Q-02, Q-03, Q-04, Q-05, Q-06 |
