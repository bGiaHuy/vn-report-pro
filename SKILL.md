---
name: vn-report-pro-v3
description: >
  Phiên bản V3 — skill tạo văn bản học thuật / hành chính tiếng Việt chuẩn , tích hợp
  toàn bộ python-docx code triển khai + quy tắc ngôn ngữ hành chính + validation checklist.
  Hỗ trợ 4 loại tài liệu: Đề xuất dự án, Báo cáo nghiên cứu, Biên bản họp, Văn bản tổng hợp.
  Dùng khi người dùng yêu cầu: "tạo file word", "viết báo cáo", "đề xuất dự án", "soạn .docx",
  "làm biên bản họp", "viết tài liệu học thuật", "tạo văn bản hành chính", "generate academic doc",
  "create proposal document", hoặc bất kỳ yêu cầu tạo file Word tiếng Việt trang trọng nào.
  Toàn bộ văn bản màu đen (#000000), font Times New Roman, danh sách dùng dấu gạch ngang thủ công,
  TUYỆT ĐỐI KHÔNG dùng bullet dot (•).
---

# Vn Report Pro V3

Skill tạo văn bản học thuật / hành chính tiếng Việt — bản hợp nhất từ `vietnamese-academic-doc` (V1: code + cấu trúc) và `vn-report-pro-v2` (V2: quy tắc ngôn ngữ + trình bày).

## Prerequisites

```bash
pip install python-docx
```

## Workflow

### Bước 1: Xác định loại tài liệu

| Loại | Cấu trúc | Tín hiệu nhận biết |
|---|---|---|
| **Đề xuất dự án** (Proposal) | Cover → Thư ngỏ → Phần I (Team) → Phần II (8 mục: Tổng quan → Mục tiêu → Đối tượng → Giải pháp → AI Tools → Rủi ro → Tiêu chí → Tham khảo) → Phụ lục | "đề xuất", "proposal", "dự án", "khóa học" |
| **Báo cáo nghiên cứu** (Research Report) | Cover → Tóm tắt → Đặt vấn đề → Phương pháp → Kết quả → Thảo luận → Kết luận → Tham khảo | "báo cáo", "nghiên cứu", "report", "phân tích" |
| **Biên bản họp** (Meeting Minutes) | Header (thời gian/địa điểm) → Thành phần → Nội dung → Kết luận → Ký tên | "biên bản", "họp", "meeting", "minutes" |
| **Văn bản tổng hợp** (General Formal) | Cover → Nội dung (theo yêu cầu) → Kết luận | Các yêu cầu khác không khớp 3 loại trên |

Nếu không rõ loại, hỏi người dùng.

### Bước 2: Thu thập nội dung

Hỏi người dùng những thông tin còn thiếu:
- Tiêu đề, phụ đề, tên tổ chức
- Thành viên (tên, vai trò, ban)
- Nội dung chính từng phần
- Số liệu, dữ liệu cụ thể

Chỉ hỏi những gì thực sự thiếu. Dùng giá trị mặc định hợp lý cho phần còn lại.

### Bước 3: Xây dựng file DOCX

Dùng python-docx. Toàn bộ thông số kỹ thuật + code mẫu nằm trong:

- **[style-spec.md](references/style-spec.md)** — Load file này khi viết code. Chứa tất cả hàm helper, page setup, style, table, bullet, cover page, cover letter.

### Bước 4: Validate trước khi lưu

Sau khi tạo xong file, chạy qua checklist bắt buộc:

- **[validation-checklist.md](references/validation-checklist.md)** — Load file này trước khi `doc.save()`. Kiểm tra 10 lỗi phổ biến nhất.

### Bước 5: Lưu và bàn giao

Lưu vào thư mục người dùng yêu cầu, hoặc mặc định vào `output/`. Thông báo đường dẫn và dung lượng.

---

## Quy tắc bắt buộc (MANDATORY)

### Trình bày

1. **Màu sắc**: Toàn bộ văn bản dùng **đen tuyệt đối** (`#000000`). Không xanh, không xám, không màu nào khác — kể cả heading.
2. **Font**: Times New Roman toàn bộ, kể cả East-Asian fallback.
3. **Bullet**: Cấp 1 `- `, cấp 2 `+ `, cấp 3 `* `. **TUYỆT ĐỐI KHÔNG DÙNG `•` (bullet dot)**. Không dùng auto-numbering của Word.
4. **Dòng trống**: Phải có một dòng trống giữa các section lớn và giữa các đoạn văn.
5. **Căn lề**: Body text căn đều hai bên (JUSTIFY). Cover page căn giữa (CENTER). Signature căn phải (RIGHT).
6. **Bảng**: Style Table Grid, header bold, không shading màu nền.

### Ngôn ngữ

7. **Ngôi kể**: Ngôi thứ ba khách quan ("Dự án được triển khai...", "Nhóm thực hiện..."). Không dùng "Tôi", "Chúng tôi" quá nhiều. Ngoại lệ: Thư ngỏ được dùng "Chúng em".
8. **Từ vựng hành chính**: Dùng các cụm từ chuẩn:

| Ngữ cảnh | Cụm từ mẫu |
|---|---|
| Mở đầu / Lý do | "Căn cứ vào...", "Trên cơ sở khảo sát...", "Xuất phát từ thực trạng..." |
| Mục tiêu | "Hướng tới...", "Phấn đấu đạt...", "Nhằm mục tiêu..." |
| Giải pháp | "Đề xuất triển khai...", "Áp dụng phương pháp..." |
| Kết luận | "Từ những phân tích trên...", "Có thể khẳng định...", "Kiến nghị..." |
| Cam kết | "Đảm bảo tính...", "Cam kết thực hiện..." |

9. **Định lượng**: Mọi mục tiêu phải có con số cụ thể (%, số lượng, thời gian). Ví dụ: "Đạt 85% tỷ lệ hoàn thành", không viết "Đạt tỷ lệ cao".

---

## Cấu trúc tài liệu

### A. Đề xuất dự án (đầy đủ)

```
1. TRANG BÌA
   - Tên trường / tổ chức (bold, 16pt, centered)
   - Tên đề xuất (bold, 16pt, centered)
   - Phụ đề / khóa học (bold, 14pt, centered)
   - Bảng thông tin nhóm (centered)
   - Địa điểm, năm (italic, centered)

2. THƯ NGỎ
   - "THƯ NGỎ" (bold, 14pt, centered)
   - Lời chào → Bối cảnh → Đề xuất → Kêu gọi
   - "Trân trọng," + tên nhóm (right-aligned)

3. PHẦN I: THÔNG TIN CHUNG
   - 1.1 Thông tin dự án (bullet)
   - 1.2 Thành viên nhóm (bảng: STT | Họ tên | Vai trò | Ban)
   - 1.3 Mô tả vai trò (từng vai trò)

4. PHẦN II: THÔNG TIN DỰ ÁN
   - I. Tổng quan vấn đề
   - II. Mục tiêu dự án (3 mục con: kiến thức, thực hành, truyền thông)
   - III. Đối tượng hưởng lợi
   - IV. Giải pháp & Kế hoạch triển khai
   - V. Công cụ AI sử dụng (bảng: STT | Công cụ | Mục đích | Giai đoạn)
   - VI. Đánh giá tính khả thi & Rủi ro (bảng 3 cột: Rủi ro | Phòng ngừa | Khắc phục)
   - VII. Tiêu chí đánh giá hiệu quả
   - VIII. Tài liệu tham khảo

5. PHỤ LỤC (nếu có)
   - Kế hoạch chi tiết, giáo án, timeline...
```

### B. Báo cáo nghiên cứu

```
1. TRANG BÌA (như trên)
2. TÓM TẮT (abstract, 150-200 từ)
3. ĐẶT VẤN ĐỀ (bối cảnh, câu hỏi nghiên cứu)
4. PHƯƠNG PHÁP (cách thu thập & xử lý dữ liệu)
5. KẾT QUẢ (trình bày findings, bảng biểu)
6. THẢO LUẬN (ý nghĩa, so sánh với nghiên cứu trước)
7. KẾT LUẬN & KIẾN NGHỊ
8. TÀI LIỆU THAM KHẢO
```

### C. Biên bản họp

```
1. HEADER: Thời gian, Địa điểm, Chủ trì, Thư ký
2. THÀNH PHẦN THAM DỰ (bảng: STT | Họ tên | Vai trò)
3. NỘI DUNG (từng mục theo agenda)
4. KẾT LUẬN & PHÂN CÔNG (bảng: Công việc | Người phụ trách | Deadline)
5. KÝ TÊN (Chủ trì + Thư ký, right-aligned)
```

### D. Văn bản tổng hợp

```
1. TRANG BÌA (như trên)
2. NỘI DUNG (theo yêu cầu người dùng)
3. KẾT LUẬN
```

---

## Reference Files

- **[style-spec.md](references/style-spec.md)**: Toàn bộ python-docx code — page setup, style, helper functions cho heading, body, bullet, table, cover page, cover letter. Load file này trước khi viết code tạo DOCX.
- **[validation-checklist.md](references/validation-checklist.md)**: Checklist 10 mục kiểm tra trước khi lưu file. Load file này ở bước cuối.

---

## Prompt Template cho người dùng

```
Tạo file Word chuẩn FPT với nội dung sau:

Loại tài liệu: [đề xuất dự án / báo cáo nghiên cứu / biên bản họp / văn bản tổng hợp]
Tiêu đề: [tiêu đề chính]
Phụ đề: [tên môn học / khóa học / không bắt buộc]
Tổ chức: [tên trường / công ty]

Thành viên (nếu có):
- [Tên] - [Vai trò] - [Ban]

Nội dung:
[Viết tự do nội dung chính, AI sẽ tự tổ chức cấu trúc]

Lưu tại: [đường dẫn hoặc để trống]
```
