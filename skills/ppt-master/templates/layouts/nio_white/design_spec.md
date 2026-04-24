# NIO Automotive-Tech Information Template (nio_white) - Design Specification

> NIO (蔚来) internal high-density template for Multi-Agent architecture decks, manufacturing-cost breakdowns, system comparison matrices, product-line review records, and cross-functional project-plan presentations.

---

## I. Template Overview

| Property       | Description                                                |
| -------------- | ---------------------------------------------------------- |
| **Template Name** | NIO 自动化科技信息模板 (nio_white)                        |
| **Use Cases**  | Multi-Agent architecture decks, IT/system overview, digital-transformation plans, review records, manufacturing-cost breakdowns, cross-functional project plans |
| **Design Tone** | Information-dense, modular, automotive-tech corporate style with structured grids, multi-column tables, comparison panels, and teal-accented numeric markers |
| **Theme Mode** | Light theme (white background + deep-navy emphasis + signature teal accent + pale-mint secondary panels) |
| **Brand Identity** | NIO Inc. / 蔚来汽车 / NYSE: NIO — corporate typeface "Blue Sky Standard"; footer watermark `NIO · Footnote` (right-hand word conventionally colored teal) |

---

## II. Canvas Specification

| Property       | Value                         |
| -------------- | ----------------------------- |
| **Format**     | Standard 16:9                 |
| **Dimensions** | 1280 x 720 px                |
| **viewBox**    | `0 0 1280 720`               |
| **Page Margins** | Left 40px, Right 40px, Top 39px, Bottom 39px |
| **Safe Area**  | x: 40-1240, y: 39-668        |

---

## III. Color Scheme

### Primary Brand Colors

| Role           | Color Value | Notes                            |
| -------------- | ----------- | -------------------------------- |
| **NIO Teal (Primary Accent)** | `#00BEBE` | Signature NIO accent — numeric markers, highlight word, key-metric callouts, table header strokes |
| **Deep Navy (Emphasis)** | `#00243B` | Cover / Section divider / Thank-you backgrounds, dark-panel fills on dense pages |
| **Near-Black** | `#000F17` | Primary text, chart axes, rule lines |
| **Dark Teal** | `#004B63` | Secondary dark — sub-emphasis blocks, nested headers on navy |
| **Background White** | `#FFFFFF` | Main page background |
| **Pale Mint** | `#B8E8EB` | Soft-panel fill for data cards, sub-section divider background, alternating-row tint |
| **Light Gray** | `#DCDCDC` | Dividers, table rules, subtle fills, timeline phase bars |
| **Charcoal Gray** | `#545859` | Secondary text, metadata, axis labels |

### Decorative / Callout Colors

| Role           | Color Value | Notes                            |
| -------------- | ----------- | -------------------------------- |
| **Pale Yellow** | `#FCF793` | Highlight background, annotation flags, "notice" markers |
| **Alert Red** | `#FF231E` | Confidentiality tag, project-plan risk/overrun block, critical data flag |

### Text Colors

| Role           | Color Value | Usage                  |
| -------------- | ----------- | ---------------------- |
| **Primary Text** | `#000F17` | Body text, headings on white |
| **White Text** | `#FFFFFF`  | Text on navy backgrounds (cover/divider/thank-you) |
| **Accent Text** | `#00BEBE` | Inline accent word, numeric markers |
| **Footer/Muted** | `#545859` (tinted, ~60% luminosity) | Page numbers, dates, confidential notice |

---

## IV. Typography System

### Font Stack

**Primary**: `"Blue Sky Standard Bold", "Blue Sky Standard Regular", "Blue Sky Standard Light", "Noto Sans CJK SC Bold", "Noto Sans CJK SC Light", "Source Han Sans CN", "PingFang SC", Arial, sans-serif`

> **Blue Sky Standard** is NIO's proprietary Latin corporate typeface (three weights: **Bold**, **Regular**, **Light**).
> **Noto Sans CJK SC** (Light / Bold) is the Chinese companion face paired with Blue Sky Standard in the source `.potx`.
> When embedding into PPTX/PDF exports, embed the font or rasterize titles to preserve layout.

### Embedded Font Files (extracted from `nio-template.potx`)

All font files in `fonts/` are Embedded OpenType (`.eot`) archives as stored by PowerPoint's `embeddedFontLst`. Convert to `.ttf` / `.woff2` if downstream rendering (e.g. browser preview, HTML export) requires native support; the native PPTX export pipeline consumes them as-is.

| File | Typeface | Weight / Variant |
| ---- | -------- | ---------------- |
| `fonts/BlueSkyStandard-Light.eot` | Blue Sky Standard Light | Regular (default body / H1 / H2) |
| `fonts/BlueSkyStandard-Regular.eot` | Blue Sky Standard Regular | Regular (footer, metadata, page numbers) |
| `fonts/BlueSkyStandard-Bold.eot` | Blue Sky Standard Bold | Regular slot |
| `fonts/BlueSkyStandard-Bold-bold.eot` | Blue Sky Standard Bold | Bold slot (H3 emphasis, card titles) |
| `fonts/NotoSansCJKsc-Light.eot` | Noto Sans CJK SC | Light (CJK body) |
| `fonts/NotoSansCJKsc-Bold.eot` | Noto Sans CJK SC | Bold (CJK headings) |

| Level | Usage              | Size (px) | Weight  |
| ----- | ------------------ | --------- | ------- |
| H1    | Cover main title, Section divider title | 36        | Light   |
| H2    | Page heading       | 36        | Light   |
| H3    | In-content section header, card title | 27        | Bold    |
| P     | Body content       | 14-18     | Light/Regular |
| Sub1  | Secondary content, table cell | 14        | Light   |
| Sub2  | Annotations, captions | 12        | Light   |

> **Adaptive sizing**: P (body) defaults to 18px but may be reduced to 14px when a page carries a dense multi-column comparison, a full-page table, or an architecture diagram with many labels. See §V-b for rules.

| Micro | Page numbers, footer | 11        | Regular |

> **Design note on small titles**: The NIO aesthetic intentionally uses a modest H1/H2 (36px) — smaller than most corporate templates. This frees vertical real estate for dense content regions. Do not inflate titles to 48px+; the compactness is a feature.

---

## V. Page Structure

### Common Content Page Navigation

| Area       | Position/Size | Description                            |
| ---------- | ------------- | -------------------------------------- |
| **Title Area** | x=40, y=39, w=892, h=69 | Page title, 36px Light, left-aligned |
| **Content Area (default)** | x=40, y=174, w=1152, h=489 | Full-width body region, master-inherited |
| **Content Area (compact)** | x=40, y=134, w=892, h=529 | Denser variant used by Basic Text / Chart / Table / Two-Column layouts |
| **Footer Left** | x=40, y=681, w=600, h=13 | `NIO · [Project]` watermark, 11px Regular, tinted `#545859` |
| **Footer Date** | x=1069, y=681, w=127, h=13 | Auto-filled date, 11px Regular |
| **Page Number** | x=1196, y=681, w=43, h=13 | Slide number, 11px Regular |

> **No top accent bar, no header rule, no logo on content pages.** Unlike ribbon-heavy corporate templates, the NIO page chrome is intentionally invisible — the brand is carried by the footer watermark (with its teal-colored terminal word) and the teal accent applied inside content. All vertical budget goes to content.

---

## V-b. Content Density & Space Utilization

> These rules place nio_white in the **high-information-density** tier (alongside `ai_ops`). Unlike the lighter-touch `metax_white`, nio_white is designed to carry comparison matrices, architecture diagrams with 10+ labeled nodes, multi-column tables, and RASIC-style grids without feeling cramped.

### 1. Image Sizing Rule

When a content page uses a **text + image split layout**, the image occupies **1/3 of the page dimension** and text occupies **2/3**.

| Layout | Image (MANDATORY) | Text |
| ------ | ----- | ---- |
| Left-right split | ≤ 400px wide (1/3 × 1200) | ≥ 800px wide (2/3 × 1200) |
| Top-bottom split | ≤ 210px tall (1/3 × 630) | ≥ 420px tall (2/3 × 630) |

- On dense analytical pages, the image serves as a **visual supplement** to the text/table, not the focal point.
- If multiple images appear on one page, their **combined area** must still respect the 1/3 constraint.

### 2. Page Fill Principle

Content must reach at least **y=640**, leaving ≤ 45px above the footer (y=681). Pages that fall short must add content — KPI callouts, table rows, process steps, or annotations. Cover, divider, and ending pages are exempt.

> This rule triggers the **Vertical Fill Budget** (pre-generation) and **Post-generation Self-check** steps defined in `executor-base.md`.

### 3. Image-Frame Fit Rule

When a content page contains images, the image frame must be sized to **match the image's native aspect ratio**. Visible padding / letterboxing inside a frame is **forbidden**. nio_white enforces an **Image-Frame Fit Rule** with the following parameters:

| Parameter | Value | Description |
| --------- | ----- | ----------- |
| `max_image_width` | 400px | Left-right split: max image frame width (1/3 × 1200) |
| `content_area_height` | 529px | Available column height (compact variant, y: 134–663) |
| `min_column_fill` | 80% | Single-image column must fill ≥ 80% of column height |
| `anti_waste_remedy` | companion content | Add KPI strip, agent-layer annotation, or mini-summary card below the image |

- The frame `<rect>` and `<image>` element must use the **same** W×H, calculated from the image's native aspect ratio: `H = W × (img_height / img_width)`.
- If the aspect-ratio-fitted image frame alone fills < 80% of the column height, the Executor **must** add companion content (KPI callout strip, data annotation, source attribution, or mini-summary card) until the column reaches ≥ 80% fill.
- Using a fixed/hardcoded frame size for all images regardless of aspect ratio is **prohibited**.

> This rule triggers the **Image-Frame Fit Checklist** steps defined in `executor-base.md`.

### 4. Adaptive Font Sizing

When a single page carries a dense diagram, a multi-column table, or an image plus substantial text, the body font size (P level) may be **reduced from 18px down to 14px** to fit more content.

| Condition | Body font size | Notes |
| --------- | -------------- | ----- |
| Text-only page, moderate density | 18px | Default |
| Text + image OR multi-column layout | 16px | Comfortable reading |
| Dense table / architecture diagram with many labels | 14px | Minimum for body text |
| Annotations / footnotes / axis labels | 12px | Absolute minimum (readability floor) |

- **Never go below 12px** for any visible text (excluding the 11px Micro footer/page-number level).
- H1 (36px) and H3 (27px) sizes remain fixed regardless of density.
- When font size is reduced, line spacing should stay at ≥ 1.35× font size for readability.

---

## VI. Page Types

### 1. Cover Page (`01_cover.svg`) — `Titelfolie` layout

- Full-bleed deep-navy background `#00243B`
- **NIO logo** (`nio_logo.png`, white-on-transparent): top-left at (38, 32) 51×47, `preserveAspectRatio="xMidYMid meet"`
- Topic line 1 + line 2: white, 36px Light, top-left anchored at (116, 38)
- Day / Month date stamp + department/author: white, 8pt, adjacent to topic at (333, 38)
- **Confidentiality tag**: red `#FF231E`, 8pt, bottom-left at (41, 663)
- Legal disclaimer strip: white 8pt at (41, 683) 1029×13
- No accent bar, no footer watermark

### 2. Section Divider Page (`02_section.svg`) — `Section Divider` layout

- Full-bleed deep-navy background `#00243B`
- Section number `#`: teal `#00BEBE`, 27px, centered at (526, 273) 230×32
- Section title: white, 36px Light, centered at (376, 321) 528×43
- Footer present (muted gray tint on navy)

### 2b. Sub-Section Divider Page — `Sub-Section Divider` layout

- Pale mint background `#B8E8EB`
- Sub-section number `#`: teal `#00BEBE`, 27px at (526, 273)
- Sub-section title: dark `#000F17`, 36px Light at (376, 321)
- Used for nested breaks within a major section

### 3. Content Page (`03_content.svg`) — multiple layouts

White background. Primary layout variants used for high-density content:

| Variant | Source Layout | Typical Use |
| ------- | ------------- | ----------- |
| **Basic Text** | `Basic Text Slide` | Dense prose, context-setting, review notes |
| **Two Column** | `Two Column text` | Parallel analysis, side-by-side specs |
| **Horizontal Split** | `Text / Image Horizontal Split 01-04` | Text top + image band bottom |
| **Vertical Split** | `Text / Image Vertical Split 01-04` | 50/50 text + image |
| **Comparison** | `Comparison 01-04` | Two 591×509 panels, mirrored — A/B matrix |
| **Project Plan** | `Project Plan` | Wide timeline band 1200×391 + 4 swim-lane callouts |
| **Chart Slide** | `Chart Slide` | Left context 444×196 + right chart 674×528 |
| **Table Slide** | `Table Slide` | Left context 292×218 + right table 832×529 |
| **Data Classification** | `Data Classification Info` | Four-tier left pyramid + four matching right descriptions |
| **TOC Grid** | `Table Of Content 01-04` | 3×3 grid of numbered section tiles (teal `#` + dark title) |

All content-page variants share:
- Standard title area at (40, 39) 892×69, 36px Light
- Standard footer strip at y=681 (watermark + date + page number)
- No top accent bar, no logo chrome

### 4. Ending Page (`04_ending.svg`) — `Thank You Slide` layout

- Full-bleed deep-navy background `#00243B` (matches cover)
- **NIO logo** (`nio_logo.png`): top-left at (38, 32) 51×47, same slot and placement as cover
- `Thank you` / `感谢观看！`: white, 36px Light, top-left at (116, 43)
- Confidentiality tag + disclaimer strip at bottom (same positions as cover)
- No page number

---

## VII. Spacing Guidelines

| Element        | Value  |
| -------------- | ------ |
| Title to content gap | 26–66px (varies by variant: compact 26px, default 66px) |
| Card gap       | 16px   |
| Content block gap | 24px |
| Card padding   | 16px   |
| Card border radius | 0px (strictly flat-rectilinear — no rounded corners anywhere) |
| Icon-to-text gap (horizontal) | 8px |
| Icon-to-title gap (vertical, inside card) | 8px |
| Minimum icon-to-icon distance | ≥ 1.5× icon width |
| Two-column gutter | 40px |
| Footer margin from bottom | 13px |
| Minimum content fill ratio | ≥ 92% of content area height (content bottom ≥ y=640) |

> **No rounded corners, no drop shadows, no gradients, no glow effects.** The template is strictly flat and rectilinear. These effects break the automotive-tech aesthetic.

---

## VII-b. Icon System

### Icon Design Intent

| Property | Value |
| -------- | ----- |
| **Design approach** | Icon-as-Label — functional semantic tags, subordinate to text. The NIO aesthetic leans on **teal numeric markers** (large `#` numerals) as the primary visual-rhythm device; icons play a supporting role |
| **Recommended source** | B: AI-generated (primary) + C: Built-in `chunk` library (fallback for generic concepts). Never mix icon styles within a deck |
| **Style anchor** (for AI-generated) | `flat vector icon, monochrome near-black (#000F17), single-weight line-art (1.5px stroke, rounded joins), white background, minimal detail, automotive-tech aesthetic, consistent with other icons in this set` |

### Numeric Marker System (Signature Device)

The NIO template's signature visual device is the **large teal numeral** — used on TOC tiles (86px tall) and as section-divider marks (27px). When ordering content, **prefer numbered markers over icons**.

### Icon Sizing Tiers (NIO-adapted)

| Tier | Size | Use Case |
| ---- | ---- | -------- |
| Primary | 32×32 px | Card header icon, comparison-panel label, data-classification tier mark |
| Secondary | 24×24 px | List item prefix, table row label |
| Tertiary | 16×16 px | Inline accent, metadata indicator |

### Icon Color Rules (NIO palette)

| Context | Color | Notes |
| ------- | ----- | ----- |
| Default (on white bg) | `#000F17` | Matches NIO body text |
| Emphasis | `#00BEBE` | NIO Teal — reserved for the single most important icon per page (respect teal accent budget: ≤3 teal elements per content slide) |
| On navy background | `#FFFFFF` | White icon on cover/divider/thank-you |
| On pale mint background | `#000F17` | Black icon on sub-section divider |
| Alert / critical | `#FF231E` | Alert red — confidentiality, risk callout only |

---

## VIII. SVG Technical Constraints

1. viewBox: `0 0 1280 720`
2. Opacity via `fill-opacity` / `stroke-opacity`, no `rgba()`
3. Forbidden: `clipPath`, `mask`, `<style>`, `class`, `foreignObject`
4. Forbidden: `textPath`, `animate*`, `script`, `marker-end`
5. Inline styles only, no external CSS
6. No `<g opacity>` — set opacity on each child individually
7. Triangle flow arrows: use `<polygon>` with apex point at the **destination** side (e.g. right-pointing ▶: `points="Xleft,Ytop Xleft,Ybot Xright,Ymid"`) — see shared-standards.md §2
8. All decorative geometry is rectilinear (`<rect>` with no `rx`/`ry`). No rounded corners, no gradients, no shadows.
9. Colors must resolve to the defined palette. Introducing colors outside §III breaks the brand.

---

## IX. Placeholder Specification

| Placeholder          | Description                    | Page Type |
| -------------------- | ------------------------------ | --------- |
| `{{TITLE}}`          | Cover topic (line 1)           | Cover     |
| `{{SUBTITLE}}`       | Cover topic (line 2) / subtitle | Cover     |
| `{{DATE}}`           | Date (day / month)             | Cover / Footer |
| `{{CONFIDENTIALITY}}`| Red confidentiality tag         | Cover / Ending |
| `{{DISCLAIMER}}`     | Legal / unauthorized-use note   | Cover / Ending |
| `{{SECTION_NUM}}`    | Large teal `#` numeral          | Section / Sub-Section / TOC tile |
| `{{SECTION_TITLE}}`  | Section divider title           | Section / Sub-Section |
| `{{PAGE_TITLE}}`     | Page title                      | Content   |
| `{{CONTENT_AREA}}`   | Main content area               | Content   |
| `{{COLUMN_1}}` / `{{COLUMN_2}}` | Parallel column body | Two Column / Comparison |
| `{{IMAGE}}`          | Picture placeholder             | Split / Full-bleed / Landscape |
| `{{CHART}}`          | Native chart placeholder        | Chart Slide |
| `{{TABLE}}`          | Native table placeholder        | Table Slide |
| `{{TIMELINE}}`       | Project-plan band               | Project Plan |
| `{{FOOTER_LEFT}}`    | Watermark text (default `NIO · Footnote`) | All content pages |
| `{{PAGE_NUM}}`       | Page number                     | Content / Ending |
| `{{THANK_YOU}}`      | Thank-you message               | Ending    |

---

## X. Asset Inventory

All assets in this section were extracted from `nio-template.potx` and are the canonical source for the `nio_white` template package.

### Logo / Imagery

| Asset | Filename | Format | Placement | Usage |
| ----- | -------- | ------ | --------- | ----- |
| NIO logo (raster, white on transparent) | `nio_logo.png` | PNG RGBA, 197×182 | Top-left slot (38, 32), rendered at native PPTX size 51×47 | Referenced by `01_cover.svg` and `04_ending.svg` via `<image href="nio_logo.png"/>`. Native aspect 197/182 ≈ slot 51/47; safe to render with `preserveAspectRatio="xMidYMid meet"`. The white glyph is designed for the deep-navy cover / thank-you background. |
| NIO logo (vector archival) | `nio_logo.emf` | EMF, ~36KB | Same slot as above | Canonical vector copy extracted from `ppt/media/image2.emf`. Not referenced by the SVG templates (SVG renderers and `svg_to_pptx` only accept PNG/JPG/SVG images), but kept in the package for downstream pipelines that can consume EMF directly. |

### Fonts

Stored under `fonts/`. See §IV for per-file typeface mapping.

| File | Typeface | Role |
| ---- | -------- | ---- |
| `fonts/BlueSkyStandard-Light.eot` | Blue Sky Standard Light | Default body / H1 / H2 |
| `fonts/BlueSkyStandard-Regular.eot` | Blue Sky Standard Regular | Footer, metadata, page numbers |
| `fonts/BlueSkyStandard-Bold.eot` | Blue Sky Standard Bold (regular slot) | H3 emphasis, card titles |
| `fonts/BlueSkyStandard-Bold-bold.eot` | Blue Sky Standard Bold (bold slot) | H3 emphasis, card titles |
| `fonts/NotoSansCJKsc-Light.eot` | Noto Sans CJK SC Light | Chinese body / H1 / H2 |
| `fonts/NotoSansCJKsc-Bold.eot` | Noto Sans CJK SC Bold | Chinese H3 / emphasis |