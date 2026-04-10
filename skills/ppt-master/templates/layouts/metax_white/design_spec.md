# MetaX Standard White (metax_white) - Design Specification

> MetaX (沐曦) brand standard template for product launches, technology presentations, investor roadshows, business visits, and corporate communications.

---

## I. Template Overview

| Property       | Description                                                |
| -------------- | ---------------------------------------------------------- |
| **Template Name** | 沐曦标准白底模板 (metax_white)                            |
| **Use Cases**  | Product technology showcase, investor relations, business visits, corporate communications |
| **Design Tone** | Tech-forward, professional, premium semiconductor industry style |
| **Theme Mode** | Light theme (white background + purple accent + green highlight) |
| **Brand Identity** | MetaX Integrated Circuits (Shanghai) Co., Ltd. / 沐曦集成电路（上海）有限公司 / Stock Code: 688802.SH |

---

## II. Canvas Specification

| Property       | Value                         |
| -------------- | ----------------------------- |
| **Format**     | Standard 16:9                 |
| **Dimensions** | 1280 x 720 px                |
| **viewBox**    | `0 0 1280 720`               |
| **Page Margins** | Left 88px, Right 66px, Top 85px, Bottom 40px |
| **Safe Area**  | x: 88-1220, y: 113-680       |

---

## III. Color Scheme

### Primary Brand Colors

| Role           | Color Value | Notes                            |
| -------------- | ----------- | -------------------------------- |
| **Brand Purple (Primary)** | `#660874` | Top bar, title accents, decorative elements |
| **Brand Purple Deep** | `#51318E` | Gradient end for top bar |
| **Background White** | `#FFFFFF` | Main page background            |
| **Teal/Cyan**  | `#1E8E94`  | Data visualization accent       |
| **Bright Green** | `#48BB00` | Chart accent                    |
| **Soft Green** | `#A4D487`  | Secondary data color            |
| **Lavender Gray** | `#A894B1` | Tertiary accent                 |
| **Light Gray** | `#E5E6E9`  | Dividers, subtle fills          |

### Decorative Colors

| Role           | Color Value | Notes                            |
| -------------- | ----------- | -------------------------------- |
| **Accent Teal-Green** | `#06DEB7` | Chapter connector line accent   |
| **Accent Purple Alt** | `#660773` | Chapter connector gradient end  |

### Text Colors

| Role           | Color Value | Usage                  |
| -------------- | ----------- | ---------------------- |
| **Primary Text** | `#000000` | Body text, headings    |
| **White Text** | `#FFFFFF`  | Text on dark backgrounds |
| **Footer/Muted** | `#898989` | Page numbers, notices  |

---

## IV. Typography System

### Font Stack

**Primary**: `"思源黑体 CN Bold", "思源黑体 CN Regular", "思源黑体 CN Light", "Source Han Sans CN", Arial, sans-serif`

| Level | Usage              | Size (px) | Weight  |
| ----- | ------------------ | --------- | ------- |
| H1    | Cover main title   | 59        | Bold    |
| H2    | Page heading       | 43        | Bold    |
| H3    | Section title      | 32        | Bold    |
| P     | Body content       | 24        | Light/Regular |
| Sub1  | Secondary content  | 21        | Light   |
| Sub2  | Annotations        | 19        | Light   |
| Micro | Page numbers       | 14        | Regular |

---

## V. Page Structure

### Common Content Page Navigation

| Area       | Position/Size | Description                            |
| ---------- | ------------- | -------------------------------------- |
| **Top Accent Bar** | x=0, y=83, w=351, h=6 | Purple gradient bar (`#660874` -> `#51318E`) |
| **Top Connector Line** | y=85, w=1096, 1px | Purple line extending from bar end |
| **Title Area** | x=35, y=10, w=1056, h=68 | Page title, 43px bold |
| **Logo (Header)** | x=1103, y=49, w=117, h=28 | Small MetaX logo, top-right |
| **Content Area** | x=88, y=113, w=1094, h=536 | Main content area |
| **Footer Left** | x=13, y=686, w=428, h=24 | Confidential notice, `#898989` |
| **Footer Right** | x=1125, y=686, w=134, h=26 | Page number, `#898989` |

---

## VI. Page Types

### 1. Cover Page (`01_cover.svg`)
- Full-bleed background image (`cover_bg.jpeg`, sci-fi/tech themed)
- MetaX logo (`metax_logo_cover.png`, 176x80px) at top-left (x=85, y=37)
- Stock code: `股票代码: 688802.SH` below logo, white text
- Main title: 59px bold, white, at (78, 300)
- Purple accent bar (x=90, y=404, w=218, h=11, `#660874`)
- Subtitle: white text below accent bar
- No footer

### 2. Chapter Divider Page (`02_chapter.svg`)
- White background with circuit board pattern (`circuit_board_bg.png`)
- MetaX logo (`logo_header_small.png`, 157x38px) at top-right
- Chapter title: 36pt bold, right-aligned area (x=593, y=318)
- Gradient connector line below title (`#06DEB7` -> `#660773`)
- Footer with confidential notice

### 3. Content Page (`03_content.svg`)
- White background
- Standard navigation (accent bar + connector line + logo)
- Title area: 43px bold
- Content area placeholder
- Footer with confidential notice and page number

### 4. Ending Page (`04_ending.svg`)
- Full-bleed background image (same as cover)
- MetaX logo with stock code at top-left
- `感谢观看！` centered, 59px bold, white
- Purple accent bar below
- No footer

---

## VII. Spacing Guidelines

| Element        | Value  |
| -------------- | ------ |
| Title to content gap | 28px |
| Card gap       | 24px   |
| Content block gap | 32px |
| Card padding   | 24px   |
| Card border radius | 8px |
| Icon-to-text gap | 12px |
| Footer margin from bottom | 14px |

---

## VIII. SVG Technical Constraints

1. viewBox: `0 0 1280 720`
2. Opacity via `fill-opacity` / `stroke-opacity`, no `rgba()`
3. Forbidden: `clipPath`, `mask`, `<style>`, `class`, `foreignObject`
4. Forbidden: `textPath`, `animate*`, `script`, `marker-end`
5. Inline styles only, no external CSS
6. No `<g opacity>` — set opacity on each child individually

---

## IX. Placeholder Specification

| Placeholder          | Description                    | Page Type |
| -------------------- | ------------------------------ | --------- |
| `{{TITLE}}`          | Main title                     | Cover     |
| `{{SUBTITLE}}`       | Subtitle                       | Cover     |
| `{{DATE}}`           | Date                           | Cover     |
| `{{AUTHOR}}`         | Author / Organization          | Cover     |
| `{{CHAPTER_NUM}}`    | Chapter number                 | Chapter   |
| `{{CHAPTER_TITLE}}`  | Chapter divider title          | Chapter   |
| `{{PAGE_TITLE}}`     | Page title                     | Content   |
| `{{CONTENT_AREA}}`   | Main content area              | Content   |
| `{{PAGE_NUM}}`       | Page number                    | Content/Ending |
| `{{THANK_YOU}}`      | Thank-you message              | Ending    |
| `{{CONTACT_INFO}}`   | Contact information            | Ending    |

---

## X. Asset Inventory

| Asset | Filename | Dimensions | Usage |
| ----- | -------- | ---------- | ----- |
| Logo (large, cover) | `metax_logo_cover.png` | 1663x759 | Cover/ending top-left |
| Logo (header) | `logo_header.png` | 265x64 | Content page top-right |
| Logo (header small) | `logo_header_small.png` | 183x44 | Chapter page top-right |
| Cover background | `cover_bg.jpeg` | 1920x1080 | Cover/ending full-bleed |
| Circuit board pattern | `circuit_board_bg.png` | 2000x1500 | Chapter divider background |
| Purple chevron | `purple_chevron.png` | 73x79 | Decorative bullet accent |
