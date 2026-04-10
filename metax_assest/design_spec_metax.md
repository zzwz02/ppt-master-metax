# MetaX (沐曦) Standard Template - Design Specification (White Background)

> Suitable for MetaX product launches, technology presentations, investor roadshows, business visits, and corporate communications.

---

## I. Template Overview

| Property       | Description                                                |
| -------------- | ---------------------------------------------------------- |
| **Template Name** | 沐曦股份标准PPT模板-白底 (metax_standard_white)          |
| **Use Cases**  | Product technology showcase, investor relations, business visits, corporate communications |
| **Design Tone** | Tech-forward, professional, premium semiconductor industry style |
| **Theme Mode** | Light theme (white background + purple accent + green highlight) |
| **Brand Identity** | MetaX Integrated Circuits (Shanghai) Co., Ltd. / 沐曦集成电路（上海）有限公司 / Stock Code: 688802.SH |

---

## II. Canvas Specification

| Property       | Value                         |
| -------------- | ----------------------------- |
| **Format**     | Standard 16:9                 |
| **Dimensions** | 1280 × 720 px (12192000 × 6858000 EMU) |
| **viewBox**    | `0 0 1280 720`               |
| **Page Margins** | Left 88px, Right 66px, Top 85px, Bottom 40px |
| **Safe Area**  | x: 88–1220, y: 113–680       |

---

## III. Color Scheme

### Primary Brand Colors

| Role           | Color Value | Notes                            |
| -------------- | ----------- | -------------------------------- |
| **Brand Purple (Primary)** | `#660874` | Top bar, title accents, decorative elements, chapter markers |
| **Brand Purple Deep** | `#51318E` | Gradient end for top bar, secondary accent |
| **Background White** | `#FFFFFF` | Main page background            |
| **Teal/Cyan**  | `#1E8E94`  | Theme dk2, data visualization accent |
| **Bright Green** | `#48BB00` | Theme hyperlink color, chart accent |
| **Soft Green** | `#A4D487`  | Chart fills, secondary data color |
| **Lavender Gray** | `#A894B1` | Tertiary accent, muted chart color |
| **Light Gray** | `#E5E6E9`  | Background blocks, dividers, subtle fills |

### Decorative Colors

| Role           | Color Value | Notes                            |
| -------------- | ----------- | -------------------------------- |
| **Accent Teal-Green** | `#06DEB7` | Chapter divider connector line accent |
| **Accent Purple Alt** | `#660773` | Chapter divider connector gradient end |

### Text Colors

| Role           | Color Value | Usage                  |
| -------------- | ----------- | ---------------------- |
| **Primary Text** | `#000000` | Body text, headings (theme dk1) |
| **White Text** | `#FFFFFF`  | Text on dark/image backgrounds (cover, ending) |
| **Footer/Muted Text** | `#898989` | Page numbers, confidential notice, annotations |

---

## IV. Typography System

### Font Stack

**Primary Font Stack**: `"思源黑体 CN Bold", "思源黑体 CN Regular", "思源黑体 CN Light", "Source Han Sans CN", "黑体", Arial, sans-serif`

| Weight Variant | Font Name | Usage |
| -------------- | --------- | ----- |
| **Bold** | 思源黑体 CN Bold | Cover title, page headings |
| **Medium** | 思源黑体 CN Medium | Emphasized labels |
| **Regular** | 思源黑体 CN Regular | Subtitles, general labels |
| **Light** | 思源黑体 CN Light | Body text, descriptions, disclaimer |
| **Fallback Latin** | Arial | All Latin/numeric text |
| **Fallback CJK** | 黑体 | CJK fallback |

### Font Size Hierarchy

| Level | Usage              | Size (pt) | Size (px approx) | Weight  |
| ----- | ------------------ | --------- | ----------------- | ------- |
| H1    | Cover main title   | 44pt      | ~59px             | Bold    |
| H2    | Page heading / Chapter title | 32pt | ~43px          | Bold    |
| H3    | Section title      | 24pt      | ~32px             | Bold    |
| P     | Body content       | 18pt      | ~24px             | Light/Regular |
| Sub1  | Secondary content  | 16pt      | ~21px             | Light   |
| Sub2  | Annotations, footer | 14pt     | ~19px             | Light   |
| Micro | Fine print, page numbers | 10–12pt | ~13–16px       | Regular |

---

## V. Page Structure

### Common Content Page Layout

| Area       | Position/Size | Description                            |
| ---------- | ------------- | -------------------------------------- |
| **Top Accent Bar** | x=0, y=83, w=351, h=6 | Purple gradient bar (`#660874` → `#51318E`), left-aligned |
| **Top Connector Line** | y=85, w=1096, 1–2px | Purple line (`#660874`) extending from bar end toward logo |
| **Title Area** | x=35, y=10, w=1056, h=68 | Page title text area, 32pt bold |
| **Logo (Header)** | x=1103, y=49, w=117, h=28 | Small MetaX logo, top-right corner |
| **Content Area** | x=88, y=113, w=1094, h=536 | Main content area |
| **Footer Left** | x=13, y=686, w=428, h=24 | Confidential notice text, `#898989` |
| **Footer Right** | x=1125, y=686, w=134, h=26 | Page number, `#898989` |

### Navigation Design

- **Top Accent Bar**: Purple gradient (`#660874` → `#51318E`), height 6px, width 351px, left-aligned at y=83
- **Top Connector Line**: Purple (`#660874`), ~1px, extends from bar end to near logo at y=85
- **Title Text**: Positioned at (35, 10), 32pt bold, primary text color `#000000`
- **Header Logo**: MetaX logo (small version, 117×28px) fixed at top-right (x=1103, y=49)
- **Footer Confidential**: `| 沐曦内部资料 | MetaX Confidential | All Rights Reserved |` in `#898989`
- **Footer Page Number**: Auto-numbered, right-aligned, `#898989`

---

## VI. Page Types

### 1. Cover Page (Layout 1 — slideLayout1)

- Full-bleed background image (1920×1080 JPEG, sci-fi/tech themed with X motif)
- Large MetaX logo (176×80px) at top-left (x=85, y=37)
- Stock code text: `股票代码: 688802.SH` below logo, white text
- Main title placeholder (x=78, y=256, w=960, h=144), 44pt bold, white text
- Subtitle placeholder (x=78, y=438, w=960, h=45), white text
- Purple accent bar (x=90, y=404, w=218, h=11, `#660874`) between title and subtitle
- No footer or page number on cover

### 2. Chapter Divider Page (Layout 2 — slideLayout2)

- Circuit board decorative background image (2000×1500 PNG, light gray line art)
- White background base
- Small MetaX logo (157×38px) at top-right (x=1075, y=52)
- Chapter title placeholder (x=593, y=318, w=600, h=71), 36pt bold, right-aligned area
- Decorative connector line below title (x=588, y=402, w=600), gradient `#06DEB7` → `#660773`
- Left vertical purple accent bar (decorative)
- Footer with confidential notice and page number

### 3. Single Content Page (Layout 3 — slideLayout3)

- White background
- Standard navigation bar (top accent bar + connector line + logo)
- Title area (x=35, y=10, w=1056, h=68), 32pt bold
- Single content placeholder (x=88, y=113, w=1094, h=536)
- Purple chevron icon (73×79px) available as decorative bullet
- Footer with confidential notice and page number

### 4. Two-Column Content Page (Layout 4 — slideLayout4)

- White background
- Standard navigation bar
- Title area (x=35, y=10, w=1056, h=68), 32pt bold
- Left content placeholder (x=31, y=115, w=595, h=527)
- Right content placeholder (x=640, y=115, w=595, h=527)
- Footer with confidential notice and page number

### 5. Two-Column Content Page B (Layout 5 — slideLayout5)

- Same structure as Layout 4, alternate two-column variant

### 6. Three-Column Content Page (Layout 6 — slideLayout6)

- White background
- Standard navigation bar
- Title area, 32pt bold
- Three content placeholder areas
- Footer with confidential notice and page number

### 7. Title-Only Page (Layout 7 — slideLayout7)

- White background
- Standard navigation bar
- Title area only (x=35, y=10, w=1056, h=68), 32pt bold
- No content placeholders — free-form layout area
- Footer with confidential notice and page number

### 8. Mixed Content Page (Layout 8 — slideLayout8)

- White background
- Standard navigation bar
- Title area (x=35, y=10, w=1056, h=68), 32pt bold
- Content placeholder (x=88, y=155, w=518, h=457) — left portion
- Available font sizes: 10pt–32pt hierarchy
- Footer with confidential notice and page number

### 9. Disclaimer Page (Layout 9 — slideLayout9)

- White background
- Standard navigation bar (top accent bar + connector line)
- Large bold title: `DISCLAIMER` (x=31, y=5, w=812, h=78), 32pt bold
- Full-width text area (x=41, y=124, w=1208, h=524) with disclaimer content
- Standard confidential and IP notice text in `#000000`, 16pt Light
- Mentions: MetaX, 沐曦, trademarks, confidentiality, http://www.metax-tech.com/
- Footer with confidential notice and page number

### 10. Ending Page (Slide 8)

- Full-bleed background image (same as cover, sci-fi/tech themed)
- Large MetaX logo at top-left with stock code
- Centered thank-you message: `感谢观看！` in 44pt bold, white text, 黑体/Arial
- Purple accent bar below thank-you text
- No footer

---

## VII. Layout Patterns (Recommended)

| Pattern              | Use Cases                      |
| -------------------- | ------------------------------ |
| **Single Column Full** | Key points, data highlights, full-page diagrams |
| **Left-Right Split (5:5)** | Comparison display, pros/cons |
| **Left-Right Split (4:6)** | Image-text mixed layout, product specs |
| **Three-Column Cards** | Feature listings, product portfolio |
| **Title-Only Freeform** | Diagrams, architecture charts, custom layouts |
| **Mixed Content** | Text + chart/image combination |
| **Table** | Data comparison, specification lists |

---

## VIII. Spacing Guidelines

| Element        | Value  |
| -------------- | ------ |
| Title to content gap | ~28px (title ends at y≈78, content starts at y=113) |
| Card gap       | 24px   |
| Content block gap | 32px |
| Card padding   | 24px   |
| Card border radius | 8px |
| Icon-to-text gap | 12px |
| Footer margin from bottom | 14px (y=686 on 720 canvas) |
| Left page margin | 88px (content), 35px (title) |

---

## IX. SVG Technical Constraints

### Mandatory Rules

1. viewBox: `0 0 1280 720`
2. Use `<rect>` elements for backgrounds
3. Text wrapping via `<tspan>` (no `<foreignObject>`)
4. Opacity via `fill-opacity` / `stroke-opacity`, no `rgba()`
5. Forbidden: `clipPath`, `mask`, `<style>`, `class`, `foreignObject`
6. Forbidden: `textPath`, `animate*`, `script`, `marker`/`marker-end`
7. Use `<polygon>` triangles for arrows instead of `<marker>`

### PPT Compatibility Rules

- No `<g opacity="...">` (group opacity) — set opacity on each child element individually
- Use overlay layers for image transparency
- Inline styles only — no external CSS or `@font-face`

---

## X. Placeholder Specification

Templates use `{{PLACEHOLDER}}` format. Common placeholders:

| Placeholder          | Description        |
| -------------------- | ------------------ |
| `{{TITLE}}`          | Main title         |
| `{{SUBTITLE}}`       | Subtitle           |
| `{{STOCK_CODE}}`     | Stock code (default: 688802.SH) |
| `{{PAGE_TITLE}}`     | Page title         |
| `{{CHAPTER_TITLE}}`  | Chapter divider title |
| `{{PAGE_NUM}}`       | Page number        |
| `{{CONTENT}}`        | Main content area  |
| `{{CONTENT_LEFT}}`   | Left column content |
| `{{CONTENT_RIGHT}}`  | Right column content |
| `{{DISCLAIMER_TEXT}}` | Disclaimer body text |
| `{{THANK_YOU}}`      | Thank-you message (default: 感谢观看！) |
| `{{CONFIDENTIAL}}`   | Footer confidential text |
| `{{LOGO_LARGE}}`     | Large Logo filename (cover/ending) |
| `{{LOGO_HEADER}}`    | Header Logo filename (content pages) |
| `{{COVER_BG_IMAGE}}` | Cover/ending background image filename |
| `{{CIRCUIT_BG_IMAGE}}` | Chapter divider circuit board background |

---

## XI. Asset Inventory

| Asset | File | Dimensions | Usage |
| ----- | ---- | ---------- | ----- |
| Logo (large, with 沐曦) | image3.png | 4037×1690 | Cover page top-left |
| Logo (header, small with 沐曦) | image1.png | 265×64 | Content page top-right |
| Logo (header, tiny) | image5.png | 183×44 | Chapter divider top-right |
| Cover/ending background | image2.jpeg | 1920×1080 | Full-bleed sci-fi/tech background |
| Circuit board pattern | image4.png | 2000×1500 | Chapter divider decorative background |
| Logo (medium) | image6.png | 1513×367 | Alternative logo placement |
| Purple chevron icon | image7.png | 73×79 | Decorative bullet/accent arrow |

---

## XII. Theme Color Slots (OOXML)

| Slot     | Color Value | Usage |
| -------- | ----------- | ----- |
| dk1      | `#000000`   | Primary text |
| lt1      | `#FFFFFF`   | Light background |
| dk2      | `#1E8E94`   | Teal accent |
| lt2      | `#E5E6E9`   | Light gray background |
| accent1  | `#660874`   | Brand purple (primary accent) |
| accent2  | `#A4D487`   | Soft green |
| accent3  | `#48BB00`   | Bright green |
| accent4  | `#A894B1`   | Lavender gray |
| accent5  | `#1E8E94`   | Teal (same as dk2) |
| accent6  | `#E5E6E9`   | Light gray (same as lt2) |
| hlink    | `#48BB00`   | Hyperlink green |
| folHlink | `#660874`   | Followed hyperlink purple |

---

## XIII. Usage Notes

1. **Template Deployment**: Copy the `.potx` template to your project directory; rename to `.pptx` for direct editing.
2. **Asset Replacement**: Replace logo images in `ppt/media/` — large logo (4037×1690 PNG), header logo (265×64 PNG).
3. **Cover Background**: Replace `image2.jpeg` (1920×1080) with custom cover imagery; maintain dark sci-fi/tech aesthetic for brand consistency.
4. **Content Generation**: Select appropriate slide layouts (1–9) based on content needs, and populate content placeholders.
5. **Confidential Footer**: All content pages carry `| 沐曦内部资料 | MetaX Confidential | All Rights Reserved |` — update or remove as needed for external distribution.
6. **Disclaimer Page**: Include disclaimer page (Layout 9) for external-facing presentations.
7. **Color Usage**: Maintain `#660874` purple as dominant brand accent; use `#48BB00` green and `#1E8E94` teal sparingly for data visualization and highlights.
