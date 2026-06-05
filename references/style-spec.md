# V3 Style Specification — Complete python-docx Blueprint

Toàn bộ code mẫu để tạo DOCX chuẩn hành chính Việt Nam. **Mọi màu sắc là `#000000`**.

---

## 1. PAGE SETUP

```python
from docx import Document
from docx.shared import Pt, Cm, Inches, RGBColor, Emu
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.enum.table import WD_TABLE_ALIGNMENT
from docx.oxml.ns import qn, nsdecls
from docx.oxml import parse_xml

doc = Document()

section = doc.sections[0]
section.page_width  = Inches(8.5)
section.page_height = Inches(11.0)
section.left_margin   = Cm(3.0)
section.right_margin  = Cm(3.0)
section.top_margin    = Cm(2.0)
section.bottom_margin = Cm(2.0)

BLACK = RGBColor(0x00, 0x00, 0x00)
```

## 2. STYLE DEFINITIONS

### 2.1 Normal (Body)

```python
style = doc.styles['Normal']
sf = style.font
sf.name = 'Times New Roman'
sf.size = Pt(12)
sf.color.rgb = BLACK
rPr = style.element.get_or_add_rPr()
rPr.append(parse_xml(f'<w:rFonts {nsdecls("w")} w:eastAsia="Times New Roman"/>'))
pf = style.paragraph_format
pf.alignment = WD_ALIGN_PARAGRAPH.JUSTIFY
pf.space_before = Pt(0)
pf.space_after  = Pt(12)   # mandatory blank space between paragraphs
pf.line_spacing = 1.15
```

### 2.2 Heading 1

```python
h1 = doc.styles['Heading 1']
h1.font.name = 'Times New Roman'
h1.font.size = Pt(16)
h1.font.bold = True
h1.font.color.rgb = BLACK
h1.paragraph_format.alignment = WD_ALIGN_PARAGRAPH.LEFT
h1.paragraph_format.space_before = Pt(24)
h1.paragraph_format.space_after  = Pt(12)
h1.paragraph_format.keep_with_next = True
```

### 2.3 Heading 2

```python
h2 = doc.styles['Heading 2']
h2.font.name = 'Times New Roman'
h2.font.size = Pt(13)
h2.font.bold = True
h2.font.color.rgb = BLACK
h2.paragraph_format.alignment = WD_ALIGN_PARAGRAPH.LEFT
h2.paragraph_format.space_before = Pt(18)
h2.paragraph_format.space_after  = Pt(8)
h2.paragraph_format.keep_with_next = True
```

### 2.4 Heading 3

```python
h3 = doc.styles['Heading 3']
h3.font.name = 'Times New Roman'
h3.font.size = Pt(12)
h3.font.bold = True
h3.font.color.rgb = BLACK
h3.paragraph_format.alignment = WD_ALIGN_PARAGRAPH.LEFT
h3.paragraph_format.space_before = Pt(12)
h3.paragraph_format.space_after  = Pt(6)
h3.paragraph_format.keep_with_next = True
```

## 3. HELPER FUNCTIONS

### 3.1 Body Paragraph

```python
def add_body(text):
    """Thêm đoạn văn bản body — justified, Times New Roman 12pt, đen."""
    p = doc.add_paragraph(text)
    p.alignment = WD_ALIGN_PARAGRAPH.JUSTIFY
    for r in p.runs:
        r.font.name = 'Times New Roman'
        r.font.size = Pt(12)
        r.font.color.rgb = BLACK
    return p
```

### 3.2 Blank Line (Mandatory Between Sections)

```python
def add_blank():
    """Dòng trống bắt buộc giữa các section / đoạn lớn."""
    p = doc.add_paragraph()
    pf = p.paragraph_format
    pf.space_before = Pt(0)
    pf.space_after  = Pt(0)
    return p
```

### 3.3 Centered Text

```python
def add_centered(text, size=16, bold=True, italic=False):
    p = doc.add_paragraph()
    p.alignment = WD_ALIGN_PARAGRAPH.CENTER
    r = p.add_run(text)
    r.font.name = 'Times New Roman'
    r.font.size = Pt(size)
    r.font.color.rgb = BLACK
    r.bold = bold
    r.italic = italic
    return p
```

### 3.4 Right-Aligned Text (Signature)

```python
def add_right(text, bold=False, italic=False, size=12):
    p = doc.add_paragraph()
    p.alignment = WD_ALIGN_PARAGRAPH.RIGHT
    r = p.add_run(text)
    r.font.name = 'Times New Roman'
    r.font.size = Pt(size)
    r.font.color.rgb = BLACK
    r.bold = bold
    r.italic = italic
    return p
```

### 3.5 Italic Quote

```python
def add_italic(text):
    p = doc.add_paragraph()
    p.alignment = WD_ALIGN_PARAGRAPH.JUSTIFY
    r = p.add_run(text)
    r.italic = True
    r.font.name = 'Times New Roman'
    r.font.size = Pt(12)
    r.font.color.rgb = BLACK
    return p
```

## 4. BULLET LISTS (Manual — TUYỆT ĐỐI KHÔNG auto-numbering)

```python
def add_bullet(text, level=1):
    """
    Thêm bullet thủ công.
    level 1: "- "
    level 2: "+ "
    level 3: "* "
    KHÔNG dùng bullet dot (•).
    """
    prefixes = {1: "- ", 2: "+ ", 3: "* "}
    prefix = prefixes.get(level, "- ")
    p = doc.add_paragraph(prefix + text)
    p.alignment = WD_ALIGN_PARAGRAPH.JUSTIFY
    for r in p.runs:
        r.font.name = 'Times New Roman'
        r.font.size = Pt(12)
        r.font.color.rgb = BLACK
    # Indent cho cấp 2 và 3
    if level == 2:
        p.paragraph_format.left_indent = Cm(1.27)
    elif level == 3:
        p.paragraph_format.left_indent = Cm(2.54)
    return p

def add_bullets(items, level=1):
    """Thêm nhiều bullet cùng lúc."""
    for item in items:
        add_bullet(item, level)
```

## 5. TABLES

### 5.1 Table Grid Style

```python
def set_table_grid_style(table):
    table.style = 'Table Grid'
    for row in table.rows:
        for cell in row.cells:
            tc = cell._tc
            tcPr = tc.get_or_add_tcPr()
            shading = tcPr.find(qn('w:shd'))
            if shading is not None:
                tcPr.remove(shading)
```

### 5.2 Standard Data Table

```python
def add_table(headers, rows):
    """
    headers: list of column header strings
    rows: list of lists, each inner list is one row
    """
    tbl = doc.add_table(rows=1 + len(rows), cols=len(headers))
    set_table_grid_style(tbl)

    # Header row
    for i, h in enumerate(headers):
        c = tbl.cell(0, i)
        c.paragraphs[0].alignment = WD_ALIGN_PARAGRAPH.CENTER
        r = c.paragraphs[0].add_run(h)
        r.bold = True
        r.font.name = 'Times New Roman'
        r.font.size = Pt(12)
        r.font.color.rgb = BLACK

    # Data rows
    for ri, row in enumerate(rows):
        for ci, val in enumerate(row):
            c = tbl.cell(ri + 1, ci)
            c.paragraphs[0].alignment = WD_ALIGN_PARAGRAPH.LEFT
            r = c.paragraphs[0].add_run(str(val))
            r.font.name = 'Times New Roman'
            r.font.size = Pt(12)
            r.font.color.rgb = BLACK

    return tbl
```

## 6. COVER PAGE

```python
def add_cover_page(doc, org_name, title, subtitle, team_data, location="Hà Nội", year="2026"):
    """
    team_data: list of (label, value) tuples. Example:
        [("Lớp:", "SSA101"), ("Nhóm:", "ML-Zero"), ("Thành viên:", "...")]
    """
    add_centered(org_name, 16, True)
    add_centered("FPT UNIVERSITY", 13, False, True)  # dòng phụ tiếng Anh nếu là FPT
    add_blank()
    add_blank()

    add_centered(title, 16, True)
    add_blank()
    if subtitle:
        add_centered(subtitle, 14, True)

    add_blank()

    # Team info table
    if team_data:
        tbl = doc.add_table(rows=len(team_data), cols=2)
        tbl.style = 'Table Grid'
        tbl.alignment = WD_TABLE_ALIGNMENT.CENTER
        for i, (lbl, val) in enumerate(team_data):
            c0 = tbl.cell(i, 0)
            c0.paragraphs[0].alignment = WD_ALIGN_PARAGRAPH.RIGHT
            r0 = c0.paragraphs[0].add_run(lbl)
            r0.bold = True; r0.font.name = 'Times New Roman'; r0.font.size = Pt(12)

            c1 = tbl.cell(i, 1)
            c1.paragraphs[0].alignment = WD_ALIGN_PARAGRAPH.LEFT
            r1 = c1.paragraphs[0].add_run(val)
            r1.font.name = 'Times New Roman'; r1.font.size = Pt(12)

        for row in tbl.rows:
            row.cells[0].width = Cm(4)
            row.cells[1].width = Cm(10)

    add_blank()
    add_centered(f"{location}, {year}", 12, False, True)
    doc.add_page_break()
```

## 7. COVER LETTER (Thư ngỏ)

```python
def add_cover_letter(doc, greeting, body_paragraphs, signer_name):
    """
    body_paragraphs: list of strings
    """
    # "THƯ NGỎ" heading
    p = doc.add_paragraph()
    p.alignment = WD_ALIGN_PARAGRAPH.CENTER
    r = p.add_run("THƯ NGỎ")
    r.bold = True
    r.font.name = 'Times New Roman'
    r.font.size = Pt(14)
    r.font.color.rgb = BLACK

    add_blank()

    # Greeting
    p_greet = doc.add_paragraph(greeting)
    p_greet.alignment = WD_ALIGN_PARAGRAPH.LEFT
    for run in p_greet.runs:
        run.font.name = 'Times New Roman'
        run.font.size = Pt(12)
        run.font.color.rgb = BLACK

    add_blank()

    # Body
    for bp_text in body_paragraphs:
        add_body(bp_text)
        add_blank()

    # Signature
    add_right("Trân trọng,", False, False)
    add_right(signer_name, True)
    doc.add_page_break()
```

## 8. MEETING MINUTES (Biên bản họp)

```python
def add_meeting_minutes(doc, date, time, location, chair, secretary, attendees, agenda_items, conclusions, assignments):
    """
    attendees: list of (name, role) tuples
    agenda_items: list of strings (từng mục thảo luận)
    conclusions: list of strings
    assignments: list of (task, person, deadline) tuples
    """
    add_centered("BIÊN BẢN HỌP", 16, True)
    add_blank()

    # Header info
    add_bullet(f"Thời gian: {time}, ngày {date}")
    add_bullet(f"Địa điểm: {location}")
    add_bullet(f"Chủ trì: {chair}")
    add_bullet(f"Thư ký: {secretary}")
    add_blank()

    # Attendees
    doc.add_heading("THÀNH PHẦN THAM DỰ", level=2)
    add_table(
        ["STT", "Họ và tên", "Vai trò"],
        [[str(i+1), name, role] for i, (name, role) in enumerate(attendees)]
    )
    add_blank()

    # Agenda
    doc.add_heading("NỘI DUNG", level=2)
    for i, item in enumerate(agenda_items, 1):
        add_bullet(f"{i}. {item}")
    add_blank()

    # Conclusions
    doc.add_heading("KẾT LUẬN", level=2)
    for c in conclusions:
        add_bullet(c)
    add_blank()

    # Assignments
    doc.add_heading("PHÂN CÔNG", level=2)
    add_table(
        ["STT", "Công việc", "Người phụ trách", "Hạn hoàn thành"],
        [[str(i+1), task, person, deadline] for i, (task, person, deadline) in enumerate(assignments)]
    )
    add_blank()

    # Signatures
    add_right(f"Hà Nội, ngày {date.split('/')[0]} tháng {date.split('/')[1]} năm 20{date.split('/')[2]}")
    add_blank()
    add_right("CHỦ TRÌ", True)
    add_blank()
    add_blank()
    add_right(chair)
    add_blank()
    add_right("THƯ KÝ", True)
    add_blank()
    add_blank()
    add_right(secretary)
    doc.add_page_break()
```

## 9. ADMINISTRATIVE VOCABULARY — Context-Aware Templates

Khi viết nội dung, ưu tiên dùng các cụm từ sau theo ngữ cảnh:

```python
ADMIN_VOCAB = {
    "opening": [
        "Căn cứ vào thực trạng...",
        "Trên cơ sở khảo sát...",
        "Xuất phát từ nhu cầu...",
        "Trong bối cảnh...",
    ],
    "objective": [
        "Hướng tới mục tiêu...",
        "Phấn đấu đạt...",
        "Nhằm đảm bảo...",
        "Với mong muốn...",
    ],
    "solution": [
        "Đề xuất triển khai...",
        "Áp dụng phương pháp...",
        "Giải pháp được đưa ra là...",
    ],
    "conclusion": [
        "Từ những phân tích trên...",
        "Có thể khẳng định rằng...",
        "Dự án hứa hẹn mang lại...",
        "Kiến nghị Ban Giám hiệu...",
    ],
    "commitment": [
        "Đảm bảo tính khả thi...",
        "Cam kết thực hiện đúng tiến độ...",
        "Chịu trách nhiệm về chất lượng...",
    ],
}
```

## 10. HEADING HELPERS (Ensure Black After Style Override)

```python
def add_h1(text):
    p = doc.add_heading(text, level=1)
    for r in p.runs:
        r.font.name = 'Times New Roman'
        r.font.color.rgb = BLACK
    return p

def add_h2(text):
    p = doc.add_heading(text, level=2)
    for r in p.runs:
        r.font.name = 'Times New Roman'
        r.font.color.rgb = BLACK
    return p

def add_h3(text):
    p = doc.add_heading(text, level=3)
    for r in p.runs:
        r.font.name = 'Times New Roman'
        r.font.color.rgb = BLACK
    return p
```

## 11. DATA PRESENTATION CONVENTIONS

- **Số liệu trong văn xuôi**: Nhúng trực tiếp vào câu. VD: "61,7% sinh viên được khảo sát cho biết..."
- **Mục tiêu định lượng**: Luôn có % hoặc số cụ thể. VD: "100% học viên...", "tối thiểu 50 học viên..."
- **Bảng rủi ro**: Luôn 3 cột — Rủi ro | Biện pháp phòng ngừa | Biện pháp khắc phục
- **Bảng thành viên**: Luôn 4 cột — STT | Họ và tên | Vai trò | Ban
- **Timeline**: Dùng bảng (Giai đoạn | Công việc | Phụ trách | Thời hạn)
- **Ngày tháng**: Định dạng DD/MM/YYYY
- **Số**: Dùng dấu chấm phân cách hàng nghìn kiểu Việt Nam (vd: "1.000", không phải "1,000")