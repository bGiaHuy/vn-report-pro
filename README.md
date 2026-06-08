# VN Report Pro — Skill tạo văn bản hành chính tiếng Việt

Skill dành cho AI agent (Claude, Marvis, v.v.) để tạo file Word (.docx) theo chuẩn văn bản hành chính — học thuật Việt Nam, 

## SKILL v5 — NĐ30/2020 + Ngôn ngữ hành chính (Mới nhất)

Bản cập nhật lớn dựa trên **Nghị định 30/2020/NĐ-CP**, đối chiếu với **TT01/2011/TT-BNV**, kết hợp phân tích ngôn ngữ từ **6.010 văn bản train_data**. File: [`SKILL_v5_NĐ30.md`](./SKILL_v5_NĐ30.md).

**Cấu trúc (12 mục):**

| Mục | Nội dung | Mới? |
|---|---|---|
| **I–IV** | Thông số kỹ thuật, 9 thành phần thể thức, 16 loại văn bản, 5 skeleton biểu mẫu | v4 |
| **V** | Quy tắc viết hoa (22 trường hợp PL II) + viết tắt (27 mã PL III) | v4 |
| **VI** | Checklist 22 mục — từ quốc hiệu đến lỗi ngôn ngữ | Cập nhật |
| **VII** | Bảng tra nhanh cỡ chữ & kiểu chữ (24 thành phần) | v4 |
| **VIII** | Cách dùng từ & cụm cố định — top 10 động từ hành chính, động từ tình thái, từ nối, cụm cố định theo loại văn bản | **v5** |
| **IX** | Cấu trúc câu điển hình — mẫu câu cho văn bản quy phạm, biên bản, giáo án | **v5** |
| **X** | Lỗi thường gặp — 14 lỗi (cấu trúc + ngôn ngữ) kèm tỷ lệ và cách khắc phục | **v5** |
| **XI** | Nhận diện nhanh loại văn bản — 9 loại với dấu hiệu, từ khóa, cấu trúc đặc biệt | **v5** |
| **XII** | Bảng công thức câu theo loại — Quyết định/Nghị quyết/Chỉ thị, Biên bản, Giáo án | **v5** |

**SKILL v4** vẫn được giữ lại tại [`SKILL_v4_NĐ30.md`](./SKILL_v4_NĐ30.md).

## Tính năng (v3)

- **4 loại tài liệu**: Đề xuất dự án, Báo cáo nghiên cứu, Biên bản họp, Văn bản tổng hợp
- **Chuẩn trình bày**: Times New Roman, đen toàn bộ (#000000), Letter size, margin 3/3/2/2 cm
- **Bullet thủ công**: 3 cấp (`-`, `+`, `*`), tuyệt đối không dùng `•`
- **Bảng biểu**: Table Grid, header bold, không shading màu
- **Ngôn ngữ hành chính**: Ngôi thứ ba, từ vựng chuẩn ("Căn cứ vào...", "Từ những phân tích trên...")
- **Validation checklist**: 10 mục kiểm tra tự động trước khi lưu file
- **Code python-docx đầy đủ**: Helper functions cho mọi thành phần (trang bìa, thư ngỏ, heading, bullet, bảng, chữ ký)

## Cấu trúc thư mục

```
vn-report-pro/
├── SKILL.md                          # Hướng dẫn chính cho AI agent (v3)
├── SKILL_v4_NĐ30.md                  # Quy tắc trình bày theo NĐ30/2020/NĐ-CP (v4)
├── SKILL_v5_NĐ30.md                  # v5: thể thức + ngôn ngữ hành chính từ 6.010 tài liệu
└── references/
    ├── style-spec.md                 # Toàn bộ python-docx code blueprint
    └── validation-checklist.md       # Checklist 10 mục + code tự kiểm
```

## Cách dùng

### Với Marvis / Claude / gemini

Đặt thư mục skill vào `skills/market/`, sau đó ra lệnh bằng tiếng Việt:

```
Tạo đề xuất dự án Machine Learning cho lớp SSA101
Làm biên bản họp nhóm hôm nay
Viết báo cáo nghiên cứu về AI trong giáo dục
```

### Chạy độc lập

Cài `python-docx`, load `references/style-spec.md` và làm theo blueprint:

```bash
pip install python-docx
```

## Loại tài liệu hỗ trợ

| Loại | Cấu trúc | Tín hiệu nhận biết |
|---|---|---|
| Đề xuất dự án | Cover → Thư ngỏ → Phần I (Team) → Phần II (8 mục) → Phụ lục | "đề xuất", "proposal", "dự án" |
| Báo cáo nghiên cứu | Cover → Tóm tắt → Đặt vấn đề → Phương pháp → Kết quả → Thảo luận → Kết luận | "báo cáo", "nghiên cứu", "report" |
| Biên bản họp | Header → Thành phần → Nội dung → Kết luận → Ký tên | "biên bản", "họp", "meeting" |
| Văn bản tổng hợp | Cover → Nội dung → Kết luận | Các yêu cầu khác |

## Quy tắc bắt buộc

- Toàn bộ văn bản màu **đen** (#000000) — kể cả heading
- Font **Times New Roman** toàn bộ
- **Không** dùng bullet dot (`•`)
- **Không** dùng auto-numbering của Word
- Body text căn đều **JUSTIFY**
- Có dòng trống giữa các section
- Ngôi thứ ba khách quan
- Mọi mục tiêu có con số cụ thể

## License

MIT
