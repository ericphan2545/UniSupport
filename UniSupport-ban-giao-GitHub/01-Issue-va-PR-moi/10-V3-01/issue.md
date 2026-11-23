# V3-01 · Thống nhất giả định công nghệ giữa PRD và kiến trúc

## Phạm vi

Đưa quyết định công nghệ và điều kiện dùng cache về ADR-005. Nối xác nhận tương thích hạ tầng với giả định, rủi ro, giá và WBS. ADR-005 chuyển sang Chấp nhận ngày 17/09/2026 (khớp DL-02), ghi rõ ngăn xếp NestJS, React, PostgreSQL, Redis và kho tệp ngoài web root; A-14 ghi đã xác nhận; R-27 đóng. Tách mốc 11/09 (hạn đề xuất) và 17/09 (xác nhận tương thích, chấp nhận ADR); dấu hiệu R-27 ghi theo từng mốc; ADR-005 giải thích phần setup WBS 1.16 (cấu hình dev tạm từ 14/09) và Staging 1.17–1.19 từ 17/09, chỉ áp dụng cấu hình phụ thuộc hạ tầng sau khi ADR được chấp nhận.

## Tiêu chí chấp nhận

- [ ] Đối chiếu aDR-005, tổng quan kiến trúc, PRD sinh viên, A-14 và R-27.
- [ ] Xác nhận bằng chứng kiểm tra ghi trong PR. Đã kiểm liên kết PRD, ADR, giá v2.0 và WBS 1.37.
- [ ] Người khác tác giả Approve.
