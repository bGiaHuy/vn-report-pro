# VN Report Pro — Skill tạo văn bản hành chính tiếng Việt

Skill dành cho AI agent (Claude, Marvis, v.v.) để tạo file Word (.docx) theo chuẩn văn bản hành chính — học thuật Việt Nam, phong cách FPT University.

## Tính năng

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
├── SKILL.md                          # Hướng dẫn chính cho AI agent
└── references/
    ├── style-spec.md                 # Toàn bộ python-docx code blueprint
    └── validation-checklist.md       # Checklist 10 mục + code tự kiểm
```

## Cách dùng

### Với Marvis / Claude

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