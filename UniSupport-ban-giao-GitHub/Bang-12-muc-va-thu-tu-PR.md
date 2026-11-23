# Bàn giao UniSupport · 12 hạng mục

Kho local `C:/Users/ADMIN/Downloads/UniSupport` có bản tích hợp tại `docs/fix-feedback-nhom-2` (nội dung tài liệu đồng bộ ở `baa03af`) và 12 nhánh riêng trong bảng. Bản tích hợp và đầu chuỗi 12 nhánh có cùng nội dung. Dùng các nhánh riêng làm PR. Giữ PR #172 ngoài đợt squash.

## Hồ sơ đã kiểm

- Baseline **1.109 giờ** gồm 1.021 giờ cơ sở và 88 giờ dự phòng rủi ro. Effort Q-01–Q-09 theo thứ tự là 290 / 90 / 235 / 130 / 61 / 106 / 108 / 42 / 47 giờ.
- Tổng tiền theo đơn giá vai trò trước VAT là **386.109.473 đồng**, VAT làm tròn đến đồng là **38.610.947 đồng**. Giá chào làm tròn lên nghìn là **424.721.000 đồng**. Các đợt thanh toán là **127.416.300 / 169.888.400 / 127.416.300 đồng**.
- Hai XLSX đã mở và lưu lại bằng LibreOffice. Đọc lại 1.342 giá trị công thức. Các ô Hạng mục F23:F26, WBS E204/E209 và cột Giờ vượt đỉnh đều bằng 0. Đối chiếu 109 dòng WBS có tiền nhiệm và 6 milestone đều `OK`.
- MS-02 hoàn thành phần dùng chung và dữ liệu ngày 16/10, test trọn luồng 19–20/10, sửa lỗi 21–22/10, kiểm lại và demo 23/10. Go-live giữ 22/12/2026 và bảo hành đến 22/01/2027.
- Kiểm 42 FR, 339 AC, 48 AC dùng chung, 64 TS, 50 NFR và 341 liên kết nội bộ. Đã đọc 7 trang PDF Proposal v2.0 phát hành mô phỏng ngày 06/09/2026.

## Thứ tự mở PR

| Mã | Nhánh local | Commit | Cha trước khi squash | Phạm vi |
|---|---|---|---|---|
| V1-01 | `docs/v1-01-boi-canh-du-an-ready` | `813112a` | `29871da` | Chuẩn hóa bối cảnh và hồ sơ đã phát hành |
| V1-02 | `docs/v1-02-effort-va-bao-gia-ready` | `31abe38` | `813112a` | Chốt baseline effort và báo giá từ dưới lên |
| V1-03 | `docs/v1-03-gia-dinh-gui-khach-ready` | `7df211d` | `31abe38` | Đồng bộ giả định trong hồ sơ gửi khách |
| V1-04 | `docs/v1-04-bo-loc-quan-tri-ready` | `5e477f2` | `7df211d` | Chuẩn hóa bộ lọc và mẫu số báo cáo GitHub |
| V1-05 | `docs/v1-05-quy-trinh-pr-ready` | `bde784a` | `5e477f2` | Áp dụng review độc lập và một PR mỗi hạng mục |
| V2-01 | `docs/v2-01-kiem-thu-theo-moc-ready` | `4c550d2` | `bde784a` | Khớp kịch bản trọn luồng với chức năng sẵn có |
| V2-02 | `docs/v2-02-ac-dung-chung-ready` | `8460c65` | `4c550d2` | Ghi rõ nghiệm thu và hồi quy cho sáu FR dùng chung |
| V2-03 | `docs/v2-03-phu-thuoc-wbs-ready` | `c2e061e` | `8460c65` | Sửa lịch WBS và kiểm ngày hoàn tất tiền nhiệm |
| V2-04 | `docs/v2-04-truy-vet-nfr-ready` | `7146e57` | `c2e061e` | Hoàn tất truy vết NFR và điều kiện Go-live |
| V3-01 | `docs/v3-01-quyet-dinh-cong-nghe-ready` | `8cc1e7d` | `7146e57` | Thống nhất giả định công nghệ giữa PRD và kiến trúc |
| V3-02 | `docs/v3-02-permalink-dac-ta-ready` | `d8067ce` | `8cc1e7d` | Cố định permalink đặc tả tại baseline có phiên bản |
| V3-03 | `docs/v3-03-ho-so-quan-tri-ready` | `b13ddf4` | `d8067ce` | Bổ sung hồ sơ quản trị có thể kiểm tra tại repo |

## Push và review

Tệp `UniSupport-GitHub-drafts.zip` chứa 12 bản nháp issue, 12 bản nháp PR và 158 nội dung issue hiện có cần đồng bộ. Công việc trên GitHub đang chờ bạn tự push và người khác tác giả Approve.

1. Tạo issue cho mục đầu tiên bằng bản nháp. Push nhánh đầu và mở PR vào `main`. Thay chỗ giữ số bằng `Closes #n` thật trong PR. Mời người khác tác giả review.

   ```powershell
   Set-Location 'C:/Users/ADMIN/Downloads/UniSupport'
   git push -u origin docs/v1-01-boi-canh-du-an-ready
   ```

2. Sau khi squash PR trước, dùng SHA ở cột **Cha trước khi squash** của nhánh kế tiếp để đưa riêng commit của nhánh đó lên `origin/main`. Ví dụ sau khi PR V1-01 đã được gộp.

   ```powershell
   git fetch origin main
   git rebase --onto origin/main 813112a docs/v1-02-effort-va-bao-gia-ready
   git log --oneline origin/main..HEAD
   git push -u origin docs/v1-02-effort-va-bao-gia-ready
   ```

   `git log` phải hiện đúng một commit của mục đang mở. Lặp lại theo thứ tự bảng. Nếu nhánh kế tiếp đã được push trước khi rebase, dùng `git push --force-with-lease` chỉ trên chính nhánh PR đó, sau khi kiểm tra người khác không cập nhật nhánh.

3. Sau khi cả 12 PR được duyệt và gộp, tạo tag `baseline-v2.0` tại commit phát hành trên `main`. Áp dụng nội dung trong `issue-updates/`, so với nội dung hiện tại trước mỗi cập nhật để giữ các chỉnh sửa và trạng thái checklist mới. Mở thử một liên kết ở issue cha, issue con và issue dùng chung sau khi tag xuất hiện.
