# V2-04 · Hoàn tất truy vết NFR và điều kiện Go-live

## Phạm vi

Nối từng NFR với TS hoặc AC, ghi vai trò thực hiện và mốc. TS-63 dẫn NFR-SEC-09 ở bảng đầu và chi tiết. TS-64 kiểm sẵn sàng triển khai trước Go-live. TS-63 bước 4 chỉ rà log của các kịch bản TS-01 đến TS-16 đã có chức năng trên Staging ở từng mốc, kịch bản mốc sau thêm dần vào hồi quy; mỗi mốc phải có log cho các sự kiện NFR-OBS-03 thuộc phạm vi mốc, đóng MS-02 phải phủ đủ 9 sự kiện; MS-02 rà thêm log của TS-23 (FR-STU-03/AC-05) và TS-54 (FR-CORE-01/AC-08).

## Tiêu chí chấp nhận

- [ ] Đối chiếu danh mục NFR, test scenarios, QMP mục 2.2 và hướng dẫn nghiệm thu.
- [ ] Xác nhận bằng chứng kiểm tra ghi trong PR. Đã kiểm 50 NFR đều có truy vết hợp lệ, 17 kịch bản NFR có vai trò và milestone.
- [ ] Người khác tác giả Approve.
