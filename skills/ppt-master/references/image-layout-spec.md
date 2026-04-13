> See shared-standards.md for common technical constraints.

# Image Layout Specification (Mandatory)

Layout rules for pages containing images. Both the Strategist planning phase and Executor generation phase must follow these rules.

**Core principle: Calculate layout based on the image's original aspect ratio, ensuring the image is displayed completely without excess whitespace or cropping.**

---

## Layout Decision Flow

```
1. Get image original dimensions → Calculate ratio (width/height)
2. Select layout type based on ratio
3. Calculate maximum display size for the image
4. Allocate remaining space for text area
5. Fill results into the Design Specification's image resource list
```

**When to execute**: If the image approach includes "B) User-provided", after the Strategist completes the Eight Confirmations and before content analysis and outlining, the scan must be run and the image resource list populated.

---

## Layout Type Selection (Mandatory)

| Image Ratio | Layout Type | Image Position | Description |
|-------------|-------------|----------------|-------------|
| > 2.0 (ultra-wide) | Top-bottom split | Top full-width | Image spans canvas width, height proportional |
| 1.5-2.0 (wide) | Top-bottom split | Top | Image width = content area width, height proportional |
| 1.2-1.5 (standard) | Left-right split | Left | Image height-first fit, width proportional |
| 0.8-1.2 (square) | Left-right split | Left | Image takes content area height, width proportional |
| < 0.8 (portrait) | Left-right split | Left | Image height = content area height, width proportional |

> Edge cases: When ratio is at a boundary (e.g., 1.5), decide based on text volume. More text → left-right; less text → top-bottom.

---

## Dimension Calculation Formulas

### PPT 16:9 (1280x720) Canvas Parameters

```
Canvas: 1280 x 720 px
Content area: 1160 x 640 px (left/right margin 60px, top/bottom margin 40px)
Title area height: 60 px
Content start: y = 80 px (title + spacing)
```

### Top-Bottom Layout Calculation

```
Image width = W = 1160 px
Image height = W / R = 1160 / R px
Text area height = H - image height - gap(20px)

Validation: Text area height >= 150px (at least 3-4 lines of text)
If not satisfied → Switch to left-right layout
```

### Left-Right Layout Calculation

**Method 1 (height-first, suitable for portrait images)**:
```
Image height = H = 600 px
Image width = H x R = 600 x R px
Text area width = W - image width - gap(20px)
```

**Method 2 (width-constrained, for wide images converted to left-right)**:
```
Image width = W x 0.7 = 812 px
Image height = image width / R
Text area width = W - image width - gap(20px)
```

**Validation**: Text area width >= 280px; otherwise reduce image area width.

---

## Layout Examples

### Ultra-wide Image (ratio 2.45)

```
Original: 1960x800, R=2.45 → Top-bottom split
Image: 1160x473, Text area: 1160x147 → 7:3 top-bottom
```

### Standard Landscape (ratio 1.38)

```
Original: 1614x1171, R=1.38 → Left-right split
Image: 773x560 (left), Text area: 367x560 (right) → 7:3 left-right
```

### Wide Image Edge Case (ratio 1.75)

```
Original: 1820x1040, R=1.75
Try top-bottom: image height=663, text area=-43 ❌
Switch to left-right: image 780x446 (left), text area 360x600 (right) → 7:3 left-right
```

---

## Template Override: Image Sizing Rule

The dimension formulas above calculate the **maximum possible** display area for the image. However, some templates impose stricter constraints in their `design_spec.md` (e.g., §V-b "Image Sizing Rule": image area ≤ 1/2 of text area).

**When a template Image Sizing Rule exists, it OVERRIDES the formulas above.**

The Executor must use `min(formula_result, template_max)` as the final image dimension.

| Step | Action |
|------|--------|
| 1 | Calculate image dimensions using the formulas above |
| 2 | Read `<project_path>/templates/design_spec.md` for Image Sizing Rule |
| 3 | If template max is smaller, use template max instead |

---

## Template Override: Image-Frame Fit Rule (Anti-Letterboxing)

Some templates define an **Image-Frame Fit Rule** in their `design_spec.md` (typically in §V-b). This rule requires that image frames match each image's native aspect ratio — no letterboxing — and that single-image columns fill a minimum percentage of the available column height.

**When a template Image-Frame Fit Rule exists**, the Frame Sizing Workflow and Single-Image Column Anti-Waste Rule below become **mandatory**. When no such rule exists in the template, these steps are recommended best practices but not enforced.

> The image frame (the `<rect>` + `<image>` container) must be sized to **match the actual image's aspect ratio**. Visible padding / letterboxing inside a frame is **forbidden**.

### Frame Sizing Workflow

Both Strategist and Executor must follow these steps:

1. **Know the image dimensions before layout.** The Strategist must record each image's pixel width × height in the Design Spec image resource list. The Executor must read these dimensions before drawing the frame.
2. **Calculate frame dimensions from the image's native aspect ratio.** Given a target width `W`, set frame height `H = W × (img_height / img_width)`. The frame `<rect>` and `<image>` element must use the **same** `W × H` — no oversized container.
3. **Cap check.** After calculating `H`, verify: (a) the frame does not violate template-level image sizing constraints (e.g. §V-b), and (b) the frame bottom stays within the safe area. If either fails, reduce `W` until both pass.

### Single-Image Column Anti-Waste Rule

When a column (e.g. right side of a left-right split) contains **only one image**, the Strategist / Executor must ensure the column's total content fills ≥ the template's `min_column_fill` (default 80%) of the available column height. If the aspect-ratio-fitted image frame alone does not reach the target, apply one of these remedies:

| Remedy | Description |
| ------ | ----------- |
| **A. Add companion content** | Place data callouts, key quotes, source annotations, or a mini-summary card above or below the image in the same column. |
| **B. Stack multiple images** | If additional relevant images exist, stack 2–3 images vertically in the column (each fitted to its own aspect ratio, with 16px gap between frames). |
| **C. Change layout** | Abandon left-right split; embed the image inline within the text flow (e.g. between cards), or use a bottom-strip layout with full-width image below the text block. |

- Whichever remedy is chosen, the **combined content** in the column must reach ≥ 80% of the available column height.

Example (metax_white, left-right split, content area = 1094px):
- Formula says: image width = 680px (height-first fit for ratio 1.79)
- Template says: max image width ≈ 360px (image ≤ 1/2 of text area)
- **Final: image width = 360px**, text width ≈ 700px

Example (metax_white, top-bottom split, content area height = 536px):
- Formula says: image height = 400px
- Template says: image height ≤ 1/2 of text block height
- **Final: image height ≤ 178px** (text block ≈ 356px, image ≤ 178px)

---

## Prohibited Practices

| Prohibited | Correct Approach |
|-----------|-----------------|
| Fixed 50:50 or arbitrary ratios | Dynamic calculation based on image ratio |
| Forcing wide image into square container | Use top-bottom layout or increase image area width |
| Placing portrait image in narrow horizontal strip | Use left-right layout, image on left |
| Image whitespace exceeding 10% | Recalculate layout or choose alternative approach |
| Cropping key image content | Use `preserveAspectRatio="xMidYMid meet"` |
| Text area too small to read | Ensure text area >= 150px (top-bottom) or >= 280px (left-right) |
| Ignoring template Image Sizing Rule | Always apply `min(formula_result, template_max)` |

---

## SVG Image Embedding Code

### Complete Display (recommended for data charts)

```xml
<image href="../images/xxx.png"
       x="60" y="80" width="780" height="446"
       preserveAspectRatio="xMidYMid meet"/>
```

### Crop-to-Fill (backgrounds only)

```xml
<image href="../images/bg.png"
       x="0" y="0" width="1280" height="720"
       preserveAspectRatio="xMidYMid slice"/>
```

---

## Image Resource List Template

In the Design Specification & Content Outline, the image resource list must include:

| Field | Description | Example |
|-------|-------------|---------|
| Filename | Image filename | `p12_0.png` |
| Original dimensions | Width x Height | 1524x968 |
| Ratio | Width / Height | 1.57 |
| Page | Usage page number | Page 5 |
| Type | Visual type | Background / Photography / Illustration / Diagram / Decorative |
| Layout plan | Top-bottom/Left-right + split ratio | Top-bottom 6:4 or Left-right 7:3 |
| Image area | Image display dimensions | 1160x420 or 780x446 |
| Text area | Remaining space dimensions | 1160x200 or 360x600 |

**The Type field is used by Image_Generator to select the appropriate prompt strategy.**

---

## Automation Tool

```bash
python3 scripts/analyze_images.py <project_path>/images
```

Output includes dimensions, ratios, and layout recommendations (Markdown table), which can be directly used to populate the image resource list.

---

## Role Responsibilities

| Role | Responsibility |
|------|---------------|
| **Strategist** | Run analyze_images.py, calculate layout per this spec, populate image resource list |
| **Executor** | Strictly follow the layout plan and dimensions in the image resource list when generating SVGs |
