# Validation Checklist — Kiểm tra trước khi lưu DOCX

Chạy qua 10 mục này trước mỗi lần `doc.save()`. Mỗi mục chỉ cần YES/NO. Nếu có NO, sửa ngay.

---

## 1. Màu sắc (CRITICAL)

- [ ] Toàn bộ text trong file có màu `#000000` không?
- [ ] Không có heading xanh, xám, hay màu nào khác?

```python
# Quick check: confirm all style color settings use BLACK
assert doc.styles['Heading 1'].font.color.rgb == BLACK
assert doc.styles['Heading 2'].font.color.rgb == BLACK
assert doc.styles['Heading 3'].font.color.rgb == BLACK
```

## 2. Bullet dots (CRITICAL)

- [ ] KHÔNG có ký tự `•` (bullet dot) ở bất kỳ đâu trong file?
- [ ] Tất cả danh sách dùng `- ` (cấp 1), `+ ` (cấp 2), `* ` (cấp 3)?
- [ ] KHÔNG dùng auto-numbering của Word?

```python
# Quick check
full_text = "\n".join([p.text for p in doc.paragraphs])
assert "•" not in full_text, "FOUND BULLET DOT — replace with '-'"
```

## 3. Font

- [ ] Toàn bộ văn bản dùng Times New Roman?
- [ ] East-Asian fallback cũng là Times New Roman?

## 4. Dòng trống giữa section

- [ ] Có dòng trống giữa các section lớn (PHẦN I, PHẦN II, ...)?
- [ ] Có dòng trống giữa các đoạn văn dài?

## 5. Căn lề

- [ ] Body text căn đều hai bên (JUSTIFY)?
- [ ] Cover page căn giữa (CENTER)?
- [ ] Signature căn phải (RIGHT)?

## 6. Bảng biểu

- [ ] Tất cả bảng dùng style Table Grid?
- [ ] Header row in đậm (bold)?
- [ ] Không có màu nền (shading) trong ô?
- [ ] Bảng rủi ro có đủ 3 cột: Rủi ro | Phòng ngừa | Khắc phục?
- [ ] Bảng thành viên có đủ 4 cột: STT | Họ tên | Vai trò | Ban?

## 7. Ngôi kể & Ngôn ngữ

- [ ] Dùng ngôi thứ ba khách quan (trừ Thư ngỏ)?
- [ ] Có dùng các cụm từ hành chính chuẩn ("Căn cứ vào...", "Từ những phân tích trên...")?
- [ ] Không có "Tôi", "Tớ", "Mình" trong body text?

## 8. Định lượng (Quantification)

- [ ] Mọi mục tiêu có con số cụ thể?
- [ ] Các tỉ lệ % có ý nghĩa và đo lường được?

## 9. Cấu trúc tài liệu

- [ ] Đúng loại tài liệu người dùng yêu cầu? (Proposal / Report / Minutes / General)
- [ ] Đầy đủ các phần bắt buộc của loại tài liệu đó?
- [ ] Có trang bìa với đầy đủ: tên trường, tiêu đề, phụ đề, thành viên, địa điểm, năm?

## 10. Page setup

- [ ] Letter size (8.5 x 11 inch)?
- [ ] Margins: Trái 3cm, Phải 3cm, Trên 2cm, Dưới 2cm?
- [ ] Page break giữa các section lớn?

---

## Tự động kiểm tra nhanh (Python snippet)

```python
def validate_doc(doc):
    errors = []
    full_text = "\n".join([p.text for p in doc.paragraphs])

    # 1. No bullet dot
    if "•" in full_text:
        errors.append("Có bullet dot (•) — thay bằng '-'")

    # 2. All headings black
    for level in [1, 2, 3]:
        style_name = f'Heading {level}'
        if style_name in [s.name for s in doc.styles]:
            color = doc.styles[style_name].font.color.rgb
            if color and color != RGBColor(0, 0, 0):
                errors.append(f"{style_name} có màu {color}, cần #000000")

    # 3. Page size
    section = doc.sections[0]
    if abs(section.page_width - Inches(8.5)) > 1:
        errors.append("Page width không phải Letter (8.5 inch)")
    if abs(section.left_margin - Cm(3.0)) > 10000:
        errors.append("Left margin không phải 3cm")

    # 4. Justified body
    if doc.styles['Normal'].paragraph_format.alignment != WD_ALIGN_PARAGRAPH.JUSTIFY:
        errors.append("Body text không căn đều (JUSTIFY)")

    return errors
```