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
| P     | Body content       | 18-24     | Light/Regular |
| Sub1  | Secondary content  | 21        | Light   |
| Sub2  | Annotations        | 19        | Light   |

> **Adaptive sizing**: P (body) defaults to 24px but may be reduced to 18px when a page contains both images and dense text. See §V-b for rules.
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

## V-b. Content Density & Space Utilization

> These rules ensure every page feels content-rich and professionally filled, without crossing into the telecom-style "high-information-density" territory (see `ai_ops` template for that). The visual language and margins of metax_white remain unchanged.

### 1. Image Sizing Rule

When a content page uses a **text + image split layout**, the image area must not exceed **1/2 of the corresponding text area**.

| Layout | Content area width | Max image width | Min text width |
| ------ | ------------------ | --------------- | -------------- |
| Left-right split | 1094px (x: 88-1182) | ~360px | ~700px |
| Top-bottom split | 1094px | Full width, but image height ≤ 1/2 of text block height | — |

- The image serves as a **visual supplement** to the text, not the focal point.
- If multiple images appear on one page, their **combined area** must still respect the 1/2 constraint.

### 2. Page Fill Principle

Content must fill the page. Bottom whitespace (between the lowest content element and the footer at y=686) should not exceed **~60px**.

- If the bottom of the page is visibly empty, the Executor must **add information**: extra data points, supplementary cards, deeper analysis, or supporting quotes.
- Cover, Chapter divider, Disclaimer, and Ending pages are **exempt** from this rule.

### 3. Adaptive Font Sizing

When a single page carries both an image and substantial text, the body font size (P level) may be **reduced from 24px down to 18px** to fit more content.

| Condition | Body font size | Notes |
| --------- | -------------- | ----- |
| Text-only page, low density | 24px | Default |
| Text + image, moderate density | 21px | Comfortable reading |
| Text + image, high density | 18px | Minimum for body text |
| Annotations / footnotes | 14px | Absolute minimum (readability floor) |

- **Never go below 14px** for any visible text (excluding page numbers at 12px Micro level).
- H2 (page heading) and H3 (section title) sizes remain fixed regardless of density.
- When font size is reduced, line spacing should stay at ≥ 1.3× font size for readability.

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

### 5. Disclaimer Page (`05_disclaimer.svg`) — ⚠️ MANDATORY

> **This page is MANDATORY.** Every presentation using this template MUST include a Disclaimer page as the **second-to-last page** (immediately before the Ending page). The Strategist MUST add it to the Content Outline (Section IX). The Executor MUST inherit the full page from the template SVG without modifying any text content.

- Uses Content Page layout (accent bar + connector line + logo + footer)
- Page title: `DISCLAIMER` (32px bold)
- Content area: single text block, no cards, no icons, no charts
- Body text: **fixed content — do NOT modify** (20px, `#000000`, line-height ~28px)
- Text block position: x=40, y=176, width=1208
- The fixed body text is:

```
This presentation may contain privileged and confidential information and is intended only for MetaX Integrated Circuits (Shanghai) Co., Ltd. and/or its affiliates (hereinafter collectively referred to as "MetaX").  The information presented in this presentation should be kept strictly confidential and should not be allowed to make available to others without written consent from MetaX.

The information presented in this presentation is for informational purposes only and may contain technical inaccuracies, omissions and typographical errors.  MetaX makes no representation or warranty of any kind, express or implied, regarding the reliability, availability or completeness of any information on the Slides.

MetaX is the owner of the presentation and all portions thereof.  Except as expressly provided, you are prohibited from copying, modifying, distributing, displaying or transmitting any of the contents of the Slides for any purposes.

MetaX, 沐曦, the MetaX logo and combinations thereof are trademarks of MetaX. Other product and company names used in this presentation are for identification purposes only and may be trademarks or trade names of their respective owners.

http://www.metax-tech.com/
```

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
| Icon-to-text gap (horizontal) | 12px |
| Icon-to-title gap (vertical, inside card) | 12px |
| Minimum icon-to-icon distance | ≥ 1.5× icon width |
| Footer margin from bottom | 14px |
| Minimum content fill ratio | ≥ 90% of content area height (content bottom ≥ y=620) |

---

## VII-b. Icon System

### Icon Design Intent

| Property | Value |
| -------- | ----- |
| **Design approach** | Icon-as-Label — functional semantic tags, subordinate to text |
| **Recommended source** | B: AI-generated (primary) + C: Built-in `chunk` library (fallback for generic concepts) |
| **Style anchor** (for AI-generated) | `flat vector icon, monochrome deep purple (#660874), single-weight line-art, white background, minimal detail, clean corporate infographic style, consistent with other icons in this set` |

### Icon Sizing Tiers (MetaX-adapted)

| Tier | Size | Use Case |
| ---- | ---- | -------- |
| Primary | 44×44 px | Card header icon (cards are 1094px content width ÷ 3 ≈ 348px per card — 44px fits better than 48px) |
| Secondary | 32×32 px | List item prefix, comparison row label |
| Tertiary | 24×24 px | Inline accent, metadata indicator |

### Icon Color Rules (MetaX palette)

| Context | Color | Notes |
| ------- | ----- | ----- |
| Default (on white bg) | `#000000` | Matches MetaX body text |
| Emphasis | `#660874` | Brand Purple — use for the single most important icon per page |
| On dark background | `#FFFFFF` | White icon on purple/dark card |
| Teal accent (data pages) | `#1E8E94` | Optional — for data-visualization related icons only |

---

## VIII. SVG Technical Constraints

1. viewBox: `0 0 1280 720`
2. Opacity via `fill-opacity` / `stroke-opacity`, no `rgba()`
3. Forbidden: `clipPath`, `mask`, `<style>`, `class`, `foreignObject`
4. Forbidden: `textPath`, `animate*`, `script`, `marker-end`
5. Inline styles only, no external CSS
6. No `<g opacity>` — set opacity on each child individually
7. Triangle flow arrows: use `<polygon>` with apex point at the **destination** side (e.g. right-pointing ▶: `points="Xleft,Ytop Xleft,Ybot Xright,Ymid"`) — see shared-standards.md §2

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

> **Note**: The Disclaimer page (`05_disclaimer.svg`) contains fixed text and uses no placeholders. Its content must not be modified.

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
