# Bàn giao Issue và Pull Request của UniSupport

**Bắt đầu tại [Prompt xử lý Issue và PR](00-PROMPT-XU-LY-ISSUE-GITHUB.md).**

## Mở Issue và Pull Request

1. Mở [bảng 12 mục và thứ tự PR](Bang-12-muc-va-thu-tu-PR.md). Trong [01-Issue-va-PR-moi](01-Issue-va-PR-moi/), mỗi mục có `issue.md` và `pull-request.md`. Dùng dòng đầu làm tiêu đề. Sau khi tạo issue, điền số thật vào `Closes #n` trong PR tương ứng.
2. Dùng 12 nhánh riêng để mở PR vào `main`. Người khác tác giả Approve trước khi squash. Sau mỗi lần squash, rebase nhánh kế tiếp bằng SHA ở cột **Cha trước khi squash** trong bảng bàn giao.
3. Nhánh tích hợp `docs/fix-feedback-nhom-2` dùng để xem cả bộ tài liệu. Nếu Đức push nhánh đó lên, GitHub sẽ cập nhật PR #172. Giữ PR #172 ngoài đợt squash 12 mục.

## Nhập các nhánh trên máy người quản lý

Mở PowerShell tại repo UniSupport đã clone. Thay đường dẫn trong dòng đầu bằng đường dẫn đầy đủ đến file [UniSupport-12-nhanh.bundle](03-Nhanh-Git/UniSupport-12-nhanh.bundle) trong thư mục này.

```powershell
$bundle = (Resolve-Path 'C:/duong-dan/00-BAN-GIAO-GITHUB-ISSUE-VA-PR/03-Nhanh-Git/UniSupport-12-nhanh.bundle').Path
git bundle verify "$bundle"
git fetch --no-tags "$bundle" 'refs/heads/docs/*:refs/heads/docs/*'
git branch --list 'docs/*-ready'
```

Kết quả là 12 nhánh local dùng để push từng mục theo thứ tự trong bảng.

## Cập nhật Issue đang có

- Mở [danh sách 158 issue](02-Cap-nhat-Issue-hien-co/Danh-sach-158-issue.md) để xem nhanh Hạn cũ, Hạn mới và file nháp của từng issue. Mỗi file `issue-N.md` tương ứng với issue GitHub số `#N`.
- Ưu tiên đối chiếu #1 với Hạn **16/10/2026**, checklist theo dõi sáu mốc ở #101, checklist AC dùng chung ở #117, #118, #123, #124, #129, #130, #155–#158 và điều kiện triển khai, nghiệm thu ở #142–#143.
- So sánh nội dung issue trên GitHub với bản nháp và giữ các cập nhật mới của thành viên, kể cả checkbox đã đánh dấu. Đặt tag `baseline-v2.0` trên `main` sau khi gộp đủ 12 PR rồi cập nhật permalink theo các bản nháp. Mở thử link ở một issue cha, một issue con và một issue Dùng chung.
