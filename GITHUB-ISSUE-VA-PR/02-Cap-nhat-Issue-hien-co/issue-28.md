**FR:** FR-CORE-07  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Mid (backend #29)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.02, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Định nghĩa một bộ quy tắc tệp dùng chung cho mọi nơi tải tệp lên, để việc kiểm tra thống nhất và bảo vệ được hệ thống.



**Đặc tả đầy đủ:** [docs_v2/modules/M05-core-platform/prd.md – FR-CORE-07](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M05-core-platform/prd.md#fr-core-07-gi%E1%BB%9Bi-h%E1%BA%A1n-t%E1%BB%87p-%C4%91%C3%ADnh-k%C3%A8m)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-CORE-07/AC-xx`)

- [ ] **AC-01** [Happy Path]: Gửi 1 PDF, 1 JPG, 1 PNG, mỗi tệp 2MB -> cả 3 được chấp nhận.

- [ ] **AC-02** [Validation]: Tệp `.docx` -> 415. Tệp PNG đổi đuôi thành `.pdf` -> chấp nhận và lưu với loại PNG đúng theo nội dung.

- [ ] **AC-03** [Boundary]: Tệp đúng 10MB -> chấp nhận. Tệp 10MB + 1 byte -> 413. Gửi đúng 5 tệp -> chấp nhận. Gửi 6 tệp -> 400.

- [ ] **AC-04** [Security]: Tên tệp `../../etc/passwd.pdf` -> các ký tự đường dẫn bị loại bỏ; tệp vẫn lưu với tên ngẫu nhiên. Gọi thẳng API bỏ qua bước kiểm tra ở giao diện -> backend vẫn chặn đúng.

- [ ] **AC-05** [Concurrency]: Nhiều sinh viên tải tệp 10MB cùng lúc -> mỗi request được kiểm tra độc lập, không ảnh hưởng nhau.

- [ ] **AC-06** [Resilience]: Request ngắt trước commit -> không ticket/bổ sung thành công, tệp tạm không tải được và dọn bù; mất response sau commit -> key trả kết quả cũ. File rỗng 400; 5×10 MiB hợp lệ trong body limit 55 MiB.
- [ ] **AC-07** [Boundary]: Tên tệp đúng 255 ký tự -> chấp nhận; 256 ký tự -> 400. Request multipart tổng lớn hơn 55 MiB -> 413. Gửi không kèm tệp -> hợp lệ.

- [ ] **AC-08** [Resilience]: Xóa bù tệp tạm thất bại -> log có mã tham chiếu, job dọn tạm xử lý lại ở lần chạy sau; hồ sơ không được báo thành công khi DB chưa commit. Frontend báo lỗi sớm với cùng ngưỡng byte và số tệp như backend, vì cả hai đọc từ một chỗ cấu hình.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-CORE-07/AC-01` đến `FR-CORE-07/AC-08` trên Staging, kiểm bằng kịch bản `TS-60`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
