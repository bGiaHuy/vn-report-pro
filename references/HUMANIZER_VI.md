# Humanizer tiếng Việt — Phát hiện & loại bỏ dấu vết AI trong văn bản hành chính

> **Nguồn gốc**: Phỏng theo [blader/humanizer](https://github.com/blader/humanizer) (30 pattern phát hiện AI-writing tiếng Anh, dựa trên Wikipedia "Signs of AI writing"), được điều chỉnh cho tiếng Việt và văn bản hành chính.
> **Tích hợp với**: SKILL_v5_NĐ30.md — áp dụng sau khi tạo văn bản để khử mùi AI.
> **Phiên bản**: 1.0

---

## Nguyên tắc chung

Sau khi AI tạo văn bản hành chính, áp dụng các quy tắc sau để văn bản đọc tự nhiên hơn, không bị lộ là do máy viết:

1. **Xác định pattern** — quét toàn bộ văn bản tìm các dấu hiệu bên dưới
2. **Viết lại, không xóa** — thay thế bằng cách diễn đạt tự nhiên, giữ nguyên nội dung
3. **Giữ ý nghĩa** — không thay đổi thông điệp cốt lõi
4. **Giữ giọng văn** — văn bản hành chính vẫn phải trang trọng, nhưng không máy móc

### Quy trình

```
Văn bản thô (AI) → Quét 20 pattern → Viết lại nháp → Tự audit "còn gì lộ AI?" → Bản cuối
```

---

## A. PATTERN NỘI DUNG (CONTENT PATTERNS)

### 1. Nhấn mạnh quá mức tầm quan trọng

**Từ khóa cần tránh:** đánh dấu bước ngoặt, có ý nghĩa then chốt, đóng vai trò quan trọng, thể hiện tầm nhìn chiến lược, góp phần to lớn, khẳng định vị thế

**Vấn đề:** AI thường thêm câu bình luận về tầm quan trọng một cách khiên cưỡng.

**Trước (AI):**
> Việc ban hành Quyết định này đánh dấu một bước ngoặt quan trọng trong công tác quản lý tài chính của đơn vị, thể hiện tầm nhìn chiến lược và quyết tâm đổi mới.

**Sau (người):**
> Quyết định này thay thế Quyết định số 15/QĐ-UBND ngày 03/3/2020, cập nhật định mức chi tiêu cho phù hợp với tình hình thực tế.

---

### 2. Ngôn ngữ quảng cáo, tô hồng

**Từ khóa cần tránh:** tự hào, nổi bật, xuất sắc, vượt trội, đẳng cấp, hàng đầu, tiên phong, đột phá

**Trước (AI):**
> Trường Đại học X tự hào là đơn vị đào tạo hàng đầu với đội ngũ giảng viên xuất sắc và cơ sở vật chất hiện đại.

**Sau (người):**
> Trường Đại học X được thành lập theo Quyết định số 123/QĐ-TTg. Hiện có 350 giảng viên, trong đó 65% có trình độ tiến sĩ.

---

### 3. Đổ lỗi mơ hồ (Weasel Words)

**Từ khóa cần tránh:** nhiều chuyên gia cho rằng, theo các nghiên cứu, thực tế cho thấy, có ý kiến cho rằng, nhiều nguồn tin

**Trước (AI):**
> Nhiều chuyên gia cho rằng việc áp dụng công nghệ AI vào quản lý sẽ mang lại hiệu quả vượt trội.

**Sau (người):**
> Theo báo cáo của Viện Chiến lược Thông tin năm 2025, các cơ quan đã áp dụng AI ghi nhận thời gian xử lý hồ sơ giảm 30-40%.

---

### 4. Phân tích hời hợt với cụm "-ing" (tương đương tiếng Việt: "nhằm", "góp phần", "hướng tới")

**Từ khóa cần tránh ở cuối câu:** nhằm nâng cao chất lượng, góp phần phát triển bền vững, hướng tới mục tiêu..., đáp ứng yêu cầu thực tiễn

**Trước (AI):**
> Đơn vị triển khai đồng bộ các giải pháp cải cách hành chính, nhằm nâng cao chất lượng phục vụ người dân, góp phần xây dựng nền hành chính hiện đại.

**Sau (người):**
> Đơn vị đã triển khai 3 giải pháp: rút ngắn thời gian xử lý hồ sơ từ 15 xuống 7 ngày, chuyển 12 thủ tục lên trực tuyến, và giảm 40% giấy tờ phải nộp.

---

### 5. Mục "Thách thức và triển vọng" rập khuôn

**Từ khóa cần tránh:** mặc dù đạt được..., vẫn còn tồn tại một số..., bên cạnh những kết quả..., tuy nhiên vẫn còn...

**Trước (AI):**
> Mặc dù đã đạt được nhiều kết quả tích cực, công tác quản lý vẫn còn tồn tại một số hạn chế như thiếu nhân lực, cơ sở vật chất chưa đồng bộ. Tuy nhiên, với quyết tâm của tập thể, đơn vị sẽ tiếp tục phát triển trong thời gian tới.

**Sau (người):**
> Còn 3 vấn đề cần giải quyết: phòng Kế hoạch thiếu 2 chuyên viên, máy chủ từ năm 2018 cần nâng cấp, và phần mềm quản lý văn bản chưa tích hợp với hệ thống của Sở.

---

## B. PATTERN NGÔN NGỮ (LANGUAGE PATTERNS)

### 6. Lạm dụng từ vựng AI (AI Vocabulary)

**Từ khóa tần suất cao trong văn bản AI tiếng Việt:**

| Từ AI | Thay bằng |
|-------|-----------|
| đồng thời | cũng, và, ngoài ra (chỉ dùng khi thực sự song song) |
| bên cạnh đó | ngoài ra, thêm nữa |
| trong bối cảnh | khi, trong lúc, hiện nay |
| đặc biệt là | nhất là, nổi bật |
| không chỉ... mà còn... | [tách thành 2 câu] hoặc chỉ nêu ý chính |
| một cách [tính từ] | bỏ "một cách", dùng trạng từ |
| mang tính | bỏ nếu không cần thiết |
| có thể nói rằng | bỏ, nói thẳng |
| cần phải nhấn mạnh rằng | bỏ, nói thẳng |
| như chúng ta đã biết | bỏ |

**Trước (AI):**
> Đồng thời, bên cạnh đó, cần phải nhấn mạnh rằng việc tổ chức thực hiện một cách nghiêm túc sẽ mang tính quyết định đến kết quả. Có thể nói rằng, đây là nhiệm vụ trọng tâm trong bối cảnh hiện nay.

**Sau (người):**
> Việc tổ chức thực hiện nghiêm túc quyết định kết quả. Đây là nhiệm vụ trọng tâm hiện nay.

---

### 7. Tránh động từ "là" (Copula Avoidance) — tương đương: lạm dụng "đóng vai trò", "được xem là"

**Trước (AI):**
> Văn phòng đóng vai trò là đầu mối liên lạc giữa các phòng ban. Hệ thống này được xem là giải pháp tối ưu cho bài toán quản lý.

**Sau (người):**
> Văn phòng là đầu mối liên lạc giữa các phòng ban. Đây là giải pháp phù hợp nhất cho bài toán quản lý.

**Lưu ý:** Trong văn bản hành chính, "là" hoàn toàn chấp nhận được và tự nhiên hơn "đóng vai trò là".

---

### 8. Cấu trúc phủ định kép / "Không chỉ... mà còn"

**Trước (AI):**
> Việc kiểm tra không chỉ nhằm phát hiện sai phạm mà còn giúp nâng cao ý thức chấp hành của cán bộ.

**Sau (người):**
> Việc kiểm tra nhằm phát hiện sai phạm và nâng cao ý thức chấp hành của cán bộ.

---

### 9. Liệt kê 3 ý máy móc (Rule of Three)

**Trước (AI):**
> Chương trình hướng tới ba mục tiêu: nâng cao nhận thức, tăng cường năng lực, và thúc đẩy hợp tác.

**Sau (người):**
> Chương trình tập trung vào tập huấn cho 200 cán bộ và ký kết thỏa thuận hợp tác với 5 trường đại học trong khu vực.

---

### 10. Lạm dụng dấu gạch ngang dài (—)

**Quy tắc:** Dấu gạch ngang dài (—) gần như không xuất hiện trong văn bản hành chính Việt Nam thực tế. Thay bằng dấu phẩy, dấu chấm, hoặc dấu ngoặc đơn.

**Trước (AI):**
> Các cơ quan — bao gồm Sở, Phòng, Ban — cần báo cáo trước ngày 15.

**Sau (người):**
> Các cơ quan (Sở, Phòng, Ban) cần báo cáo trước ngày 15.

---

### 11. In đậm quá mức

**Quy tắc:** Trong văn bản hành chính, in đậm chỉ dùng cho tên loại văn bản và tiêu đề. Không in đậm từ khóa trong đoạn văn.

**Trước (AI):**
> Các đơn vị cần **tổ chức thực hiện nghiêm túc** các nội dung đã được **phê duyệt** trong **kế hoạch**.

**Sau (người):**
> Các đơn vị cần tổ chức thực hiện nghiêm túc các nội dung đã được phê duyệt trong kế hoạch.

---

### 12. Tiêu đề viết hoa toàn bộ các từ (Title Case)

**Quy tắc:** Tiếng Việt không dùng Title Case. Chỉ viết hoa chữ cái đầu.

**Trước (AI):**
> ## Kế Hoạch Tổ Chức Hội Nghị Và Triển Khai Công Tác

**Sau (người):**
> ## Kế hoạch tổ chức hội nghị và triển khai công tác

---

### 13. Emoji trong văn bản hành chính

**Quy tắc:** TUYỆT ĐỐI không dùng emoji trong văn bản hành chính.

**Trước (AI):**
> 📋 Nhiệm vụ trọng tâm:
> ✅ Hoàn thành báo cáo quý
> ⚠️ Lưu ý: hạn chót 30/6

**Sau (người):**
> Nhiệm vụ trọng tâm:
> - Hoàn thành báo cáo quý
> - Lưu ý: hạn chót 30/6

---

### 14. Dấu ngoặc kép cong ("...")

**Quy tắc:** Văn bản hành chính Việt Nam dùng dấu ngoặc kép thẳng ("...") hoặc dấu ngoặc kép kiểu Pháp («...»). Không dùng dấu cong (“...”).

---

## C. PATTERN GIAO TIẾP (COMMUNICATION PATTERNS)

### 15. Tàn dư chatbot

**Từ khóa:** Tôi hy vọng..., Rất vui được..., Nếu cần thêm..., Xin cảm ơn... (ở đầu/cuối văn bản hành chính)

**Quy tắc:** Văn bản hành chính không có câu mở đầu/ kết thúc kiểu chatbot. Bắt đầu thẳng vào nội dung, kết thúc bằng Nơi nhận.

---

### 16. Giọng nịnh nọt, tâng bốc

**Trước (AI):**
> Kính thưa quý lãnh đạo, đây là một câu hỏi hết sức quan trọng và cấp thiết. Chúng tôi xin phép được trình bày một cách đầy đủ và chi tiết nhất.

**Sau (người):**
> Về vấn đề này, đơn vị báo cáo như sau:

---

## D. PATTERN ĐỆM VÀ NHẤN MẠNH THỪA

### 17. Cụm từ đệm (Filler Phrases)

| Trước (AI) | Sau (người) |
|------------|-------------|
| để có thể thực hiện được | để thực hiện |
| trong quá trình triển khai thực hiện | khi triển khai |
| cần phải tiến hành | cần |
| có trách nhiệm tổ chức thực hiện | thực hiện |
| trên cơ sở căn cứ vào | căn cứ |
| nhằm mục đích | để |
| bằng việc | bằng cách, thông qua |
| một số lượng lớn | nhiều |
| trong thời gian tới | sắp tới |

---

### 18. Nhấn mạnh thừa (Over-hedging)

**Trước (AI):**
> Có thể nói rằng, về cơ bản, kết quả đạt được là tương đối khả quan trong điều kiện thực tế.

**Sau (người):**
> Kết quả đạt khá so với mục tiêu đề ra.

---

### 19. Kết luận chung chung sáo rỗng

**Trước (AI):**
> Với những kết quả đã đạt được, tập thể đơn vị sẽ tiếp tục phát huy, phấn đấu hoàn thành xuất sắc mọi nhiệm vụ được giao.

**Sau (người):**
> Năm tới, đơn vị tập trung vào 3 nhiệm vụ: số hóa toàn bộ hồ sơ, tuyển thêm 5 chuyên viên, và triển khai phần mềm quản lý mới trước tháng 6.

---

### 20. Từ ghép có gạch nối không cần thiết

**Quy tắc:** Tiếng Việt không dùng gạch nối cho từ ghép thông thường. Chỉ dùng cho tên riêng phiên âm và thuật ngữ đặc biệt.

**Trước (AI):**
> đa-lĩnh-vực, liên-ngành, xuyên-suốt

**Sau (người):**
> đa lĩnh vực, liên ngành, xuyên suốt

---

## E. PATTERN RIÊNG CHO TIẾNG VIỆT HÀNH CHÍNH

### 21. Lạm dụng "đã" + động từ

AI tiếng Việt thường thêm "đã" vào trước mọi động từ quá khứ.

**Trước (AI):**
> Các đại biểu đã thảo luận và đã thống nhất các nội dung đã được trình bày.

**Sau (người):**
> Các đại biểu thảo luận và thống nhất các nội dung được trình bày.

---

### 22. Lặp cấu trúc câu

AI thường viết các câu có cùng độ dài, cùng cấu trúc.

**Trước (AI):**
> Phòng Tài chính đã hoàn thành báo cáo quý I. Phòng Kế hoạch đã triển khai đề án mới. Phòng Tổ chức đã tuyển dụng xong nhân sự.

**Sau (người):**
> Phòng Tài chính hoàn thành báo cáo quý I. Đề án mới đang được Phòng Kế hoạch triển khai. Về nhân sự, đợt tuyển dụng đã kết thúc — Phòng Tổ chức đang làm thủ tục tiếp nhận 3 người.

---

### 23. Thiếu chi tiết cụ thể

AI có xu hướng viết chung chung.

**Trước (AI):**
> Công tác kiểm tra đã đạt được nhiều kết quả tích cực.

**Sau (người):**
> Qua kiểm tra 15 đơn vị, phát hiện 3 trường hợp sai phạm về chứng từ thanh toán. Đã xử lý kỷ luật 2 cán bộ.

---

### 24. Giọng bị động tràn lan

**Trước (AI):**
> Hồ sơ được tiếp nhận bởi bộ phận một cửa. Quyết định được phê duyệt bởi Giám đốc. Kết quả được thông báo bởi Văn phòng.

**Sau (người):**
> Bộ phận một cửa tiếp nhận hồ sơ. Giám đốc phê duyệt quyết định. Văn phòng thông báo kết quả.

---

## F. TỔNG HỢP — AUDIT NHANH

Khi đọc lại văn bản, hỏi 5 câu:

| # | Câu hỏi | Nếu CÓ → AI |
|---|---------|-------------|
| 1 | Có câu nào dài hơn 4 dòng không ngắt? | Viết lại, tách câu |
| 2 | Có quá 2 lần "đồng thời", "bên cạnh đó"? | Cắt bớt |
| 3 | Có cụm "đánh dấu bước ngoặt", "có ý nghĩa then chốt", "mang tính"? | Bỏ |
| 4 | Mọi câu trong 1 đoạn có cùng độ dài? | Đảo cấu trúc, thay đổi nhịp |
| 5 | Có số liệu cụ thể, dẫn chứng, tên riêng không? | Nếu không → thêm vào |

---

## G. VÍ DỤ TỔNG HỢP

### Trước (AI-generated — đậm mùi máy):

> Kính gửi: Ban Giám đốc
>
> Đồng thời, bên cạnh những kết quả đã đạt được, có thể nói rằng, công tác quản lý tài chính trong thời gian qua đã đánh dấu một bước ngoặt quan trọng, góp phần nâng cao hiệu quả hoạt động của đơn vị. Trong bối cảnh hiện nay, việc tăng cường kiểm tra, giám sát không chỉ nhằm phát hiện sai phạm mà còn giúp nâng cao ý thức chấp hành — điều này mang tính quyết định đến sự phát triển bền vững.
>
> Tuy nhiên, vẫn còn tồn tại một số hạn chế như thiếu nhân lực, cơ sở vật chất chưa đồng bộ. Nhiều chuyên gia cho rằng cần phải có những giải pháp đột phá. Mặc dù vậy, với quyết tâm và nỗ lực, tập thể đơn vị sẽ tiếp tục phát huy, phấn đấu hoàn thành xuất sắc mọi nhiệm vụ được giao.
>
> Tôi hy vọng báo cáo này hữu ích. Nếu cần thêm thông tin, xin vui lòng liên hệ.

### Sau (đã humanize):

> Kính gửi: Ban Giám đốc
>
> Năm 2025, Phòng Tài chính kiểm tra 12 đơn vị trực thuộc. Kết quả: 10 đơn vị chấp hành đúng quy định, 2 đơn vị sai phạm về chứng từ thanh toán (tổng giá trị 156 triệu đồng). Đã thu hồi toàn bộ và xử lý kỷ luật 3 cán bộ liên quan.
>
> So với năm 2024, số sai phạm giảm từ 5 xuống 2 đơn vị. Nguyên nhân chính: tập huấn nghiệp vụ định kỳ và phần mềm cảnh báo tự động đưa vào từ tháng 3.
>
> Hạn chế còn lại: Phòng thiếu 1 kế toán trưởng từ tháng 7/2025 (nghỉ hưu, chưa tuyển được người thay). Đề nghị Ban Giám đốc cho phép tuyển gấp trong quý II/2026.

---

## Tham khảo

- [blader/humanizer](https://github.com/blader/humanizer) — 30 pattern AI-writing tiếng Anh
- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
- SKILL_v5_NĐ30.md — quy tắc trình bày văn bản hành chính Việt Nam
