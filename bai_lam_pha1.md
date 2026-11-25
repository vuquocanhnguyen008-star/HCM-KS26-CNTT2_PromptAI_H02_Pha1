# BÀI LÀM PHA 1 

- **Họ tên:**Nguyễn Vũ Quốc Anh
- **Lớp:** HCM-KS26-CNTT2
- **Link repository Pha1:**

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
[TEN_KHACH] 
[SO_DIEN_THOAI] 
[SO_CCCD] 
[Email] 
đặt 3 vé gói SVT trận bán kết Cúp Sao Vàng ngày 18/12/2026 cho 2 người lớn và 1 trẻ em 9 tuổi. 
Khán giả đã chuyển khoản đặt cọc 1.500.000 đồng. Khán giả yêu cầu 3 ghế ngồi cạnh nhau gần lối đi.

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
Câu 1: Nếu khán giả huỷ vé trước sự kiện 4 ngày thì được hoàn bao nhiêu tiền? Thì hãy dựa vào file đính kèm mà xử lí theo quy định. Còn nếu file không có thông tin hãy hỏi lại tôi để được giải quyết, không tự tiện đưa ra phương án, giải pháp nếu không có trong quy định file đính kèm

Câu 2: Khi trả lời đáp án hãy ghi kèm mục cũng như điều khoản mà bạn đã dựa vào để gửi tôi đáp án, nếu không có cái nào theo mục, điều khoản thì báo lại là không có.

```

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**PROMPT**

```text
C - Context
Bạn là nhân viên mới của Ban tổ chức giải bóng đá Cúp Sao Vàng, phụ trách hỗ trợ khán giả mua vé các trận đấu. Bạn xử lý thông tin khán giả, tin nhắn đặt vé, báo giá và khiếu nại một cách an toàn, chính xác.
R - Request
Chuyển TN1, TN2, TN3 thành một mảng JSON 3 đối tượng, đúng thứ tự, đúng 6 trường như tin nhắn mẫu

E - Explanation
BẢNG MÃ GÓI
SVT – Sân Vận Động: vé trận đấu
FZ – Fan Zone: khu cổ vũ
VIP – Khán đài VIP
QUY TẮC CHUẨN HOÁ (5 quy tắc)
1. ma_goi lấy theo bảng mã gói.
2. ngay_su_kien dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
4. loai_ve chỉ nhận "thuong", "vip" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về chỗ ngồi, hỗ trợ di chuyển hoặc ăn uống; không có thì null.
A - Action
chuyển TN1, TN2, TN3 thành một mảng JSON 3 đối tượng, đúng thứ tự, đúng 6 trường như tin nhắn mẫu
Chỉ trả về JSON, không chào hỏi, không giải thích.

đưa bảng giá, quy tắc và yêu cầu báo giá vào cặp dấu phân tách
•	Đặt 3 tin nhắn cần xử lý trong cặp dấu phân tách (ví dụ <tin_nhan>…</tin_nhan>).
T - Tone
Giọng văn không nịnh hót, rõ ràng, chính xác, có lập trường riêng của mình. 
E - Extras.
Tin nhắn mẫu 1:
Đặt giúp mình 2 vé VIP trận chung kết ngày 15/11/2026, 2 người lớn và 1 bé. Cần chỗ ngồi cạnh lối đi.
Kết quả đúng:
{"ma_goi": "VIP", "ngay_su_kien": "2026-11-15", "nguoi_lon": 2, "tre_em": 1, "loai_ve": "vip", "ghi_chu": "chỗ ngồi cạnh lối đi"}

Tin nhắn mẫu 2:
Cho mình một vé Fan Zone, ngày nào cũng được, mình đi một mình.
Kết quả đúng:
{"ma_goi": "FZ", "ngay_su_kien": null, "nguoi_lon": 1, "tre_em": 0, "loai_ve": "thuong", "ghi_chu": null}

Trước khi tạo Prompt hãy hỏi người dung gửi trước 3 tin nhắn để bạn duajw vào đó mà tạo
```

## 4.3. Báo giá vé bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
C - Context
Bạn là nhân viên tính tổng tiền cho yêu cầu báo giá một cách an toàn, chính xác.
R - Request
tính tổng tiền cho yêu cầu báo giá
E - Explanation
BẢNG GIÁ VÉ (đồng/người lớn)
SVT – Sân Vận Động: 1.200.000
FZ – Fan Zone: 800.000
VIP – Khán đài VIP: 2.500.000

QUY TẮC TÍNH GIÁ
1. Trẻ em từ 5 đến 11 tuổi tính 70% giá người lớn. Trẻ dưới 5 tuổi miễn phí.
2. Nhóm từ 5 người lớn trở lên được giảm 5% trên tiền vé (tiền người lớn + tiền trẻ em).
3. Phụ thu khu ghế ưu tiên: 300.000 đồng/người. Phụ thu không được giảm giá.
4. Tổng tiền = tiền vé sau giảm + phụ thu.

A - Action
Pha 1: 
suy luận từng bước theo thứ tự tiền người lớn → tiền trẻ em → tiền vé → giảm giá nhóm → phụ thu → tổng tiền, 
mỗi bước ghi phép tính; dòng cuối có dạng [RESULT: <tổng tiền> đồng].

đưa bảng giá, quy tắc và yêu cầu báo giá vào cặp dấu phân tách
</tin_nhan>...</tin_nhan>

T - Tone
Giọng văn không nịnh hót, rõ ràng, chính xác, có lập trường riêng của mình.
E - Extras
Tin nhắn mẫu 1:
Đặt giúp mình 2 vé VIP trận chung kết ngày 15/11/2026, 2 người lớn và 1 bé. Cần chỗ ngồi cạnh lối đi.
Kết quả đúng:
{"ma_goi": "VIP", "ngay_su_kien": "2026-11-15", "nguoi_lon": 2, "tre_em": 1, "loai_ve": "vip", "ghi_chu": "chỗ ngồi cạnh lối đi"}


```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
Bạn là nhân viên đọc email của Ban tổ chức Cúp Sao Vàng. Chỉ dùng chính sách trong thẻ <Doc_Email> để ddocj email khasch. Trả về đúng một đối tượng JSON, không giải thích:
đọc email, trả về chỉ một đối tượng JSON gồm đúng 4 trường:
{"loai_su_co": một trong "tran_gio" | "sai_khu_vuc" | "nhan_vien" | "khac", "so_khach": số nguyên, "so_hiep_bi_anh_huong": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}

```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. kết quả phân loại khiếu nại (JSON của Trạm 1) 

2.chính sách đền bù.

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
Dựa trên JSON trong thẻ <ket_qua_tram_2>, viết email phản hồi khán giả: xin lỗi, nêu đúng mức đền bù trong JSON, không tự đặt mức khác, giọng điệu chân thành, chuyên nghiệp, dưới 150 từ.
<ket_qua_tram_2>(JSON của Trạm 2)</ket_qua_tram_2>

```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
(Viết prompt vào đây – xoá dòng này)
```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Cúp Sao Vàng", nhân viên tư vấn vé và dịch vụ trên website của giải bóng đá Cúp Sao Vàng, giúp khán giả chọn và đặt một trong 3 gói: SVT (Sân Vận Động – vé trận đấu), FZ (Fan Zone – khu cổ vũ), VIP (Khán đài VIP).

[Quy tắc ràng buộc]
1. chỉ tư vấn 3 gói của giải
2.không hứa giảm giá ngoài bảng giá
3. không hỏi xin số CCCD, số thẻ ngân hàng.

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá vé trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: (Viết cách trả lời – xoá dòng này)
- Khách cung cấp thiếu thông tin:
```
