# UniSupport – Kế hoạch quản lý sản phẩm

Phiên bản 0.6

**Phạm vi tài liệu:** quản lý UniSupport như một sản phẩm **sau khi đưa vào sử dụng**: phục vụ ai, theo nguyên tắc nào, nhận phản hồi và đo kết quả ra sao, phát triển tiếp theo hướng nào, xét duyệt thay đổi bằng tiêu chí gì, và tiếp thị để người dùng thật sự dùng. Tài liệu không chứa kế hoạch hay lịch của dự án (xem GitHub), tiêu chí chất lượng ([02](02-qmp-quan-ly-chat-luong.md)) hay cách quản trị dự án ([03](03-quan-tri-giam-sat.md)).

## 1. Mục tiêu sản phẩm và điểm xuất phát

### 1.1 Giá trị sản phẩm

UniSupport hướng đến ba kết quả:

- **Sinh viên** gửi yêu cầu thuận tiện, hiểu được tình trạng xử lý và nhận được kết quả.
- **Nhân viên** tiếp nhận tập trung, giảm thất lạc hồ sơ, rõ ai chịu trách nhiệm xử lý.
- **Nhà trường** nhận biết điểm nghẽn và cải thiện chất lượng phục vụ bằng số liệu.

### 1.2 Điểm xuất phát khi bàn giao

- **Có sẵn:** các chức năng đã thống nhất trong hợp đồng cho 3 vai trò Sinh viên, Nhân viên, Admin; web responsive (điện thoại và máy tính); VN/EN; thông báo trong app; dashboard và 3 báo cáo cho Admin.
- **Chưa có (ngoài phạm vi hợp đồng):** tích hợp SIS/LMS, thanh toán; ứng dụng di động riêng; thông báo email/SMS/push; trích xuất dữ liệu; Admin xem hoặc xử lý ticket; mở lại hồ sơ đã đóng; màn hình sửa danh mục.
- **Sau bàn giao:** bên phát triển sửa lỗi trong thời gian bảo hành (Proposal 6.3). Nhà trường tự vận hành hệ thống (Proposal 6.4). Mọi tính năng mới là change request riêng, xét theo mục 7.

## 2. Khách hàng và khách hàng của khách hàng

Khách hàng ký hợp đồng là **Aurora University**. Nhưng giá trị sản phẩm do **người dùng của trường** quyết định: nếu sinh viên không dùng, nhân viên thấy phiền hoặc Admin không có số liệu tin được, trường sẽ không coi sản phẩm là thành công.

| Nhóm | Vai trò với sản phẩm | Cần gì | Lắng nghe qua | Đo bằng |
|---|---|---|---|---|
| **Aurora University** (Director of Student Services) | Khách hàng: người mua, quyết định đầu tư tiếp | Một kênh hỗ trợ thống nhất; số liệu để đánh giá và điều phối; chi phí vận hành hợp lý | Họp rà soát sản phẩm (mục 4) | Chỉ số ở mục 6.3; kết quả rà soát |
| **Sinh viên** (khoảng 3.000, chủ yếu dùng điện thoại) | Người thụ hưởng cuối cùng | Gửi nhanh, biết phòng nào xử lý, theo dõi tiến độ, nhận kết quả, được bảo vệ dữ liệu cá nhân | Đánh giá kết quả sau khi hồ sơ đóng, khảo sát UI/UX, kênh phản hồi (mục 5) | Mức hài lòng với kết quả hỗ trợ, tỷ lệ hài lòng UI/UX (mục 6.2) |
| **Nhân viên** các phòng ban | Người dùng hằng ngày, tạo ra kết quả hỗ trợ | Hàng đợi rõ ràng, ít thao tác thừa, phân công và chuyển phòng dễ, không bị quá tải | Phỏng vấn ngắn, khảo sát UI/UX, kênh phản hồi | Thời gian xử lý, khối lượng theo phòng, tỷ lệ hài lòng UI/UX |
| **Admin** (quản trị phía trường) | Người quản trị hệ thống | Cấp/thu quyền, thiết lập SLA, đọc dashboard và báo cáo đúng. **Không** xem chi tiết hồ sơ hay tệp | Kênh phản hồi, họp rà soát | Số hồ sơ quá hạn, báo cáo dùng được cho điều phối |

**Khi lợi ích các nhóm xung đột**, ưu tiên theo thứ tự:

1. An toàn dữ liệu và quyền riêng tư của sinh viên.
2. Sinh viên hoàn thành được việc cần làm.
3. Khối lượng và sự thuận tiện của nhân viên.
4. Nhu cầu báo cáo và quản lý của nhà trường.

Ví dụ: yêu cầu "cho Admin xem chi tiết hồ sơ để báo cáo kỹ hơn" vi phạm thứ tự 1, nên không đưa vào dù có lợi cho thứ tự 4.

## 3. Nguyên tắc quản lý sản phẩm

| # | Nguyên tắc | Nghĩa là | Không làm |
|---|---|---|---|
| 1 | Bắt đầu từ vấn đề có bằng chứng | Mỗi thay đổi phải chỉ ra vấn đề gì, xảy ra bao nhiêu lần, ai bị ảnh hưởng | Làm vì "có vẻ hay" hoặc vì một người yêu cầu mà không có dữ liệu |
| 2 | Nhìn qua khách hàng tới người dùng cuối | Đánh giá theo cả bốn nhóm ở mục 2, không chỉ theo người ký hợp đồng | Chỉ tối ưu báo cáo cho nhà trường mà làm khó sinh viên |
| 3 | Bảo vệ dữ liệu sinh viên là ràng buộc | Quyền truy cập và bảo vệ tệp là điều kiện bắt buộc của mọi thay đổi | Đánh đổi quyền riêng tư lấy tiện lợi |
| 4 | Đo trước và sau | Thay đổi nào cũng có chỉ số thành công, đo cùng một cách trước và sau | Tuyên bố cải tiến khi chưa có số liệu |
| 5 | Thử nhỏ rồi mới mở rộng | Áp dụng thử ở một phòng ban hoặc một nhóm người dùng trước | Đổi đồng loạt cho toàn trường ngay lần đầu |
| 6 | Tính chi phí toàn vòng đời | Tính cả chi phí xây dựng, vận hành và bảo trì, vì trường tự vận hành sau bàn giao | Chỉ nhìn chi phí làm ra tính năng |
| 7 | Giữ sản phẩm đơn giản | Ưu tiên làm tốt hơn cái đang có trước khi thêm tính năng mới | Thêm tính năng khi tính năng cũ còn ít người dùng |
| 8 | Ghi lại quyết định và lý do | Mọi yêu cầu đều có kết luận: chấp thuận, thử nghiệm, hoãn hay từ chối, kèm lý do | Để yêu cầu treo không có câu trả lời |

## 4. Quy trình quản lý sản phẩm

Quản lý sản phẩm là một vòng lặp liên tục, không có ngày kết thúc.

**Người tham gia sau bàn giao** (nhà trường chỉ định trước khi tiếp nhận vận hành):

- **Đầu mối sản phẩm của trường:** chủ trì rà soát, quyết định thứ tự ưu tiên.
- **Đầu mối tiếp nhận phản hồi:** nhận và theo dõi phản hồi (mục 5).
- **Bên triển khai:** đội phát triển trong thời gian bảo hành; sau đó là bên được trường giao theo change request.

| Bước | Làm gì | Ai | Đầu ra |
|---|---|---|---|
| 1. Thu thập | Gom phản hồi (mục 5), số liệu dashboard Admin, khảo sát UI/UX, đánh giá của sinh viên | Đầu mối tiếp nhận phản hồi | Danh sách vấn đề và đề xuất |
| 2. Phân loại | Tách lỗi, khó khăn sử dụng, khiếu nại dịch vụ, đề xuất cải tiến; gộp trùng | Đầu mối tiếp nhận phản hồi | Mỗi mục có loại, nhóm người dùng bị ảnh hưởng, mức ảnh hưởng |
| 3. Đánh giá | Lỗi: chuyển sửa theo bảo hành. Đề xuất cải tiến: đánh giá theo tiêu chí ở mục 7 | Đầu mối sản phẩm, bên triển khai | Phiếu đánh giá từng đề xuất |
| 4. Quyết định | Chấp thuận, thử nghiệm, hoãn hoặc từ chối; xếp vào giai đoạn phù hợp ở mục 8 | Đầu mối sản phẩm của trường | Quyết định có lý do, ghi vào hồ sơ |
| 5. Thực hiện | Bên triển khai làm theo change request đã duyệt; thử nhỏ trước nếu có thể | Bên triển khai | Bản cập nhật |
| 6. Đo lại | Đo chỉ số thành công đã đặt ở bước 3, cùng phương pháp như trước | Đầu mối sản phẩm | Kết luận giữ, điều chỉnh hay quay lại |
| 7. Truyền thông | Báo người gửi phản hồi và người dùng bị ảnh hưởng về thay đổi (mục 9) | Đầu mối tiếp nhận phản hồi | Thông báo, hướng dẫn cập nhật |

**Nhịp rà soát:** hằng tháng trong giai đoạn Ổn định sử dụng; hằng quý khi đã sang các giai đoạn sau. Mỗi lần rà soát dùng phản hồi, số liệu vận hành và năng lực thực tế để điều chỉnh ưu tiên.

## 5. Quản lý phản hồi và khiếu nại

Phản hồi ở đây là phản hồi **về sản phẩm và dịch vụ hỗ trợ**, không phải nội dung yêu cầu hỗ trợ mà sinh viên gửi qua hệ thống.

### 5.1 Đầu vào

Tiếp nhận từ sinh viên, nhân viên, Admin và đại diện nhà trường qua các kênh được công bố. Mỗi trường hợp có mã theo dõi, thời điểm tiếp nhận, người gửi, nhóm người dùng, nội dung, ảnh hưởng, bằng chứng và người phụ trách.

### 5.2 Quy trình xử lý

| Bước | Hoạt động | Đầu ra |
|---|---|---|
| Tiếp nhận | Ghi thông tin, kiểm tra trùng, xác nhận nội dung | Hồ sơ phản hồi có mã theo dõi |
| Phân loại | Khiếu nại dịch vụ, khó khăn sử dụng, lỗi, hay đề xuất cải tiến | Loại phản hồi và mức ảnh hưởng |
| Đánh giá | Xác minh hiện tượng, nhóm người bị ảnh hưởng và phương án | Kết luận và đề xuất xử lý |
| Quyết định | Chọn cách giải quyết, người thực hiện, thời hạn | Phương án được xác nhận |
| Thực hiện | Xử lý hoặc chuyển đúng bên có trách nhiệm | Kết quả và bằng chứng |
| Phản hồi và đóng | Báo kết quả cho người gửi, kiểm tra vấn đề ban đầu đã được giải quyết chưa | Trạng thái kết thúc và nội dung còn tồn |

### 5.3 Quy tắc

- Mọi phản hồi được theo dõi đến khi có kết luận.
- Trường hợp ảnh hưởng nghiêm trọng đến dữ liệu hoặc việc sử dụng được chuyển xử lý ngay.
- Phản hồi thiếu thông tin được yêu cầu bổ sung và ghi rõ phần còn thiếu.
- Chuyển cho bên khác không làm mất trách nhiệm theo dõi.
- Đề xuất làm thay đổi sản phẩm chuyển sang mục 7.

### 5.4 Thời gian xử lý

Chỉ tiêu: xử lý xong trong **dưới 48 giờ làm việc** kể từ khi xác nhận tiếp nhận. Quy định chính thức phải xác định: sự kiện bắt đầu và kết thúc tính giờ, lịch làm việc áp dụng, cách tính thời gian chờ bổ sung thông tin, cách cảnh báo trước hạn và báo cáo quá hạn.

### 5.5 Đầu ra

Báo cáo định kỳ: số trường hợp tiếp nhận, đã xử lý, quá hạn, phản ánh lặp lại và nguyên nhân phổ biến, tách theo nhóm người dùng. Kết quả dùng để cải thiện sản phẩm, hướng dẫn và quy trình phục vụ.

## 6. Đo lường trải nghiệm và giá trị sử dụng

### 6.1 Thang hài lòng UI/UX

Thang năm mức: 1 Rất không hài lòng · 2 Không hài lòng · 3 Bình thường · 4 Hài lòng · 5 Rất hài lòng.

Câu hỏi thành phần: giao diện rõ ràng, dễ tìm thông tin, thao tác thuận tiện, hiểu được trạng thái và thông báo. Có một câu đánh giá tổng thể để tính tỷ lệ hài lòng; các câu thành phần dùng để tìm điểm cần cải thiện. Khảo sát riêng cho sinh viên, nhân viên và Admin.

### 6.2 Cách tính

**Tỷ lệ hài lòng UI/UX = số người chọn 4 hoặc 5 ở câu tổng thể / tổng số người trả lời hợp lệ × 100%.**

Kết quả phải kèm số người trả lời, nhóm người dùng và phiên bản được đánh giá. Hài lòng về giao diện báo cáo riêng với hài lòng về kết quả hỗ trợ. Mục tiêu ban đầu đề xuất: **≥ 80%** cho từng nhóm người dùng.

### 6.3 Chỉ số vận hành sản phẩm

| Chỉ số | Cho thấy | Nguồn dữ liệu |
|---|---|---|
| Số hồ sơ mới / đang xử lý / quá hạn | Tải và điểm nghẽn | Dashboard Admin |
| Thời gian xử lý trung bình | Tốc độ phục vụ | Dashboard Admin |
| Loại yêu cầu phổ biến | Nhu cầu chính của sinh viên | Dashboard Admin |
| Khối lượng theo phòng ban | Phân bổ công việc của nhân viên | Dashboard Admin |
| Mức hài lòng của sinh viên với kết quả | Giá trị hỗ trợ nhận được | Dashboard Admin |
| Tỷ lệ phản hồi xử lý trong hạn 48 giờ | Chất lượng lắng nghe | Hồ sơ phản hồi (mục 5) |
| Tỷ lệ khiếu nại lặp lại cùng nguyên nhân | Vấn đề chưa được giải quyết tận gốc | Hồ sơ phản hồi (mục 5) |
| Tỷ lệ người bắt đầu gửi và gửi thành công; trung vị và phân vị 90 của thời gian xử lý; tuổi của hồ sơ chưa hoàn thành | Chỗ người dùng bỏ dở; các trường hợp chờ lâu | **Chưa có trong hệ thống bàn giao.** Muốn đo phải qua change request (mục 7) |

### 6.4 Hành động từ kết quả đo

Mỗi chỉ số chưa đạt phải có: phân tích nguyên nhân, hành động, người phụ trách, thời điểm đánh giá lại. Số liệu trước và sau thay đổi đo theo cùng phương pháp.

## 7. Đánh giá và phê duyệt change request

Mọi yêu cầu thay đổi sản phẩm, dù từ nhà trường, sinh viên, nhân viên, Admin hay bên triển khai, phải qua hai lớp: **cổng bắt buộc** rồi mới đến **chấm điểm ưu tiên**.

### 7.1 Cổng bắt buộc (trượt một điều là không đưa vào phát triển)

| # | Điều kiện | Câu hỏi kiểm tra |
|---|---|---|
| G1 | Đúng mục tiêu sản phẩm | Thay đổi có phục vụ ít nhất một kết quả ở mục 1.1 không? |
| G2 | An toàn dữ liệu sinh viên | Có làm lộ dữ liệu, mở rộng quyền xem hồ sơ/tệp ngoài vai trò, hoặc làm yếu bảo vệ tệp không? Có là trượt |
| G3 | Có bằng chứng nhu cầu | Có phản hồi, số liệu hoặc tần suất xác minh được không? |
| G4 | Khả thi | Có phương án kỹ thuật, dữ liệu và người thực hiện không? |
| G5 | Đo được | Có chỉ số thành công và cách đo trước/sau không? |
| G6 | Có người duyệt và nguồn kinh phí | Đầu mối sản phẩm của trường đồng ý; nếu cần bên triển khai làm thì đã có báo giá và xác nhận bằng văn bản (Proposal 6.3) |

### 7.2 Tiêu chí chấm điểm ưu tiên (mỗi tiêu chí 1 thấp – 2 trung bình – 3 cao)

| Nhóm | Tiêu chí | Nội dung đánh giá |
|---|---|---|
| Giá trị | Giá trị cho sinh viên | Mức cải thiện trải nghiệm hoặc kết quả nhận hỗ trợ |
| Giá trị | Giá trị cho nhân viên | Giảm thao tác, giảm thất lạc, giảm quá tải |
| Giá trị | Giá trị cho nhà trường và Admin | Hiệu quả phục vụ, điều phối, vận hành |
| Giá trị | Độ rộng ảnh hưởng | Bao nhiêu người hoặc hồ sơ được hưởng lợi |
| Chi phí – rủi ro | Tác động UI/UX | Mức thay đổi thói quen, nhu cầu hướng dẫn lại (3 = thay đổi ít) |
| Chi phí – rủi ro | Chất lượng và an toàn | Ảnh hưởng đến độ ổn định, quyền truy cập (3 = ảnh hưởng ít) |
| Chi phí – rủi ro | Chi phí toàn vòng đời | Chi phí xây dựng, vận hành, bảo trì cho trường (3 = chi phí thấp) |

**Quy tắc quyết định:**

- Trượt một cổng ở mục 7.1: từ chối, hoặc hoãn cho đến khi bổ sung đủ.
- Qua hết cổng: xếp ưu tiên theo tổng điểm nhóm Giá trị; nếu bằng nhau, chọn yêu cầu có tổng điểm nhóm Chi phí – rủi ro cao hơn.
- Có điểm 1 ở "Chất lượng và an toàn": chỉ được thử nghiệm trên phạm vi nhỏ trước (nguyên tắc 5).

### 7.3 Quy trình và thẩm quyền

Tiếp nhận → kiểm tra cổng → chấm điểm → so sánh phương án → chấp thuận, thử nghiệm, hoãn hoặc từ chối → thông báo quyết định cho người đề xuất → theo dõi kết quả theo bước 6 ở mục 4.

- **Trong thời gian dự án:** quy trình theo [03](03-quan-tri-giam-sat.md) mục 5; bộ tiêu chí ở mục 7.1–7.2 dùng ở bước "Đánh giá ảnh hưởng".
- **Sau bàn giao:** đầu mối sản phẩm của trường tổ chức xem xét. Thay đổi cần bên triển khai thực hiện phải thống nhất trước về trách nhiệm, nguồn lực và chi phí. Yêu cầu tính năng mới không thuộc bảo hành (Proposal 6.3).

### 7.4 Hồ sơ quyết định

Mỗi yêu cầu lưu: nội dung, người đề xuất, nhóm người dùng liên quan, bằng chứng, kết quả kiểm tra từng cổng, điểm từng tiêu chí, phương án, quyết định, lý do, kết quả đo sau khi thực hiện.

## 8. Lộ trình sau khi đưa vào sử dụng

Lộ trình theo **giá trị cần đạt**, không theo ngày. Chuyển sang giai đoạn sau khi đạt điều kiện, không phải khi hết thời gian.

| Giai đoạn | Trọng tâm | Việc chính | Điều kiện chuyển giai đoạn sau |
|---|---|---|---|
| 1. Ổn định sử dụng | Người dùng làm được việc chính | Hỗ trợ người dùng, xử lý khó khăn ban đầu, sửa lỗi bảo hành, theo dõi độ tin cậy | Lỗi nghiêm trọng đã xử lý; phản hồi xử lý trong hạn 48 giờ ổn định; nhân viên các phòng dùng hằng ngày |
| 2. Mở rộng sử dụng | Đưa mọi yêu cầu về một kênh | Truyền thông, hướng dẫn, mở rộng nhóm người dùng và phòng ban | Số hồ sơ qua hệ thống tăng đều; khiếu nại "không biết gửi ở đâu" giảm |
| 3. Tối ưu trải nghiệm | Làm cái đang có tốt hơn | Sửa các bước gây bỏ dở, nhầm lẫn, phải liên hệ lại; chỉnh theo khảo sát UI/UX | Tỷ lệ hài lòng UI/UX ≥ 80% ở cả ba nhóm người dùng |
| 4. Phát triển dài hạn | Đầu tư mới có giá trị và duy trì được | Đánh giá nhu cầu mới và khả năng mở rộng theo mục 7 | Mỗi đầu tư mới qua đủ cổng 7.1 |

**Ứng viên cho giai đoạn 4** (chỉ là danh sách để đánh giá, chưa phải cam kết; mỗi mục phải qua mục 7):

- Proposal 2.2 nêu là phase sau: tích hợp SIS/LMS, thanh toán, ứng dụng di động riêng.
- Đã loại khỏi phạm vi hợp đồng: thông báo email/SMS, trích xuất dữ liệu, các chỉ số chưa đo được ở mục 6.3.
- Không đưa vào danh sách: Admin xem chi tiết hồ sơ (trượt cổng G2).

## 9. Tiếp thị và phát triển mức sử dụng

Nhà trường là bên chủ trì truyền thông qua kênh của trường. Bên phát triển cung cấp tài liệu: 3 bộ hướng dẫn sử dụng theo vai trò (Proposal 6.1).

### 9.1 Đối tượng và thông điệp

| Nhóm | Thông điệp chính | Kênh |
|---|---|---|
| Sinh viên | "Gửi một nơi, biết ai đang xử lý, theo dõi trên điện thoại" | Kênh chính thức của trường, sinh hoạt lớp và khoa, đại diện lớp, điểm tiếp nhận |
| Nhân viên | "Một hàng đợi cho cả phòng, không thất lạc hồ sơ, phân công rõ" | Buổi hướng dẫn theo phòng ban, hướng dẫn ngắn cho thao tác thường gặp |
| Admin và lãnh đạo | "Thấy được hồ sơ quá hạn và điểm nghẽn bằng số liệu" | Buổi giới thiệu dashboard, báo cáo định kỳ |

### 9.2 Chiến thuật theo giai đoạn

| Thời điểm | Việc làm |
|---|---|
| Trước khi mở cho người dùng | Hướng dẫn nhân viên trước để khi sinh viên gửi đã có người nhận; chuẩn bị hướng dẫn ngắn |
| Khi mở cho sinh viên | Giới thiệu qua kênh chính thức; hướng dẫn tại điểm tiếp nhận và các đợt sinh hoạt; nhân viên hướng sinh viên gửi qua hệ thống thay vì kênh cũ |
| Duy trì | Nhắc lại vào thời điểm nhiều yêu cầu trong học kỳ; chia sẻ kết quả cải thiện (ví dụ thời gian xử lý giảm) |
| Khi có cải tiến quan trọng | Truyền thông lại cho đúng nhóm bị ảnh hưởng; cập nhật hướng dẫn |

### 9.3 Đo hiệu quả

Theo dõi mức tiếp cận, lượt sử dụng có nhu cầu thực tế, tỷ lệ hoàn tất và khó khăn sau khi tiếp cận, tách theo nhóm người dùng.

### 9.4 Điều chỉnh

Tại mỗi lần rà soát (mục 4), quyết định duy trì, thay đổi hoặc dừng hoạt động truyền thông dựa trên hiệu quả và chi phí.

## Lịch sử thay đổi

Trong thời gian dự án, sửa tài liệu này theo [03](03-quan-tri-giam-sat.md) mục 5. Sau bàn giao, đầu mối sản phẩm của trường duyệt.

| Phiên bản | Nội dung |
|---|---|
| 0.2 | Bản trước |
| 0.3 | Tách khỏi QMP và Quản trị: bỏ ngày, người phụ trách, rủi ro, quy trình thay đổi |
| 0.4 | Viết lại theo hướng sau khi đưa vào sử dụng: bỏ kế hoạch dự án; thêm khách hàng của khách hàng, nguyên tắc, quy trình vòng lặp, cổng và tiêu chí change request, lộ trình theo giá trị, tiếp thị |
| 0.5 | Bỏ các mã chức năng |
| 0.6 | Hoàn thiện câu chữ |
