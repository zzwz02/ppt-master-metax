# MetaX White 模板配图生成 — 可复用 Prompt 规范

> 适用模板: `metax_white` | 使用工具: [image-generator.md](../../../references/image-generator.md)

---

Read [image-generator.md](../../../references/image-generator.md) for detailed tool usage instructions.

## 一、配图生成工作流 (Workflow)

### 输入要求
- **源文档**: Markdown 内容稿（确定配图主题和内容方向）
- **参考文稿**（可选）: PDF / PowerPoint / Word 等原始文档，用于提取可复用的图表/图示作为参考
  - PDF → PyMuPDF 按 xref 提取嵌入图片，或裁剪内容区域
  - PPTX → `python-pptx` 遍历 slide shapes 提取 image blobs
  - DOCX → `python-docx` 提取 inline/floating images
- **目标模板**: 指定 design_spec（决定配色、风格锚点）

### 分阶段执行流程

```
Phase 1  分析源文档 → 确定需要 N 张配图的关键内容点
         （N 由用户指定；若未指定则 Agent 根据内容量自行决定）
         → 定义每张图的主题与预期视觉内容
    ↓
Phase 2  从参考文稿提取图片素材
         （PDF 按 xref / PPTX 按 shape / DOCX 按 inline，非整页截图）
    ↓
Phase 3  Agent 自审提取结果，按三个维度分类：
         a. 质量检查 — 是否黑图/破损/过于简陋（尺寸、色彩丰富度、内容完整性）
         b. 主题匹配 — 图片内容是否符合 Phase 1 定义的主题
         c. 风格匹配 — 图片视觉风格是否符合目标模板（flat vector / 配色 / 线稿风格）
         → 分类结果：
           ✅ 质量+主题+风格均通过 → 直接使用（PDF提取）
           ⚠️ 质量+主题通过但风格不符 → 提取保留作为参考，AI 按模板风格重制（PDF提取 → AI重制）
           ❌ 质量不合格或主题不符 → 丢弃，纯 AI 生成
    ↓
Phase 4  输出 N 张配图计划表（主题 + 来源标注 + prompt 草案）→ 用户 Review
         ⚠️ 必须在此阶段让用户确认，不可跳过直接生成
    ↓
Phase 5  用户确认后，按 design_spec 的 Style Anchor 修订所有 prompt
         → 将通用风格锚点（§二）和通用负面提示词（§三）拼接到每条 prompt
    ↓
Phase 6  逐张生成 → 每张落盘后 Agent 立即自审（质量 + 主题匹配），不合格自行重做
         分批交付用户检查（先 2-3 张确认风格方向，通过后再生成剩余）
```

### 关键原则

1. **不截整页**: 从参考文稿提取时按嵌入对象抽取图片（PDF→xref, PPTX→shape, DOCX→inline），或裁剪内容区域（排除页眉页脚），绝不直接 render 整页
2. **Agent 先自审再交付**: 提取/生成的图片必须由 Agent 先检查质量（尺寸、色彩丰富度、内容完整性），不合格的自行重做，不要把质量判断推给用户
3. **先规划后执行**: 列出全部配图的主题、来源（PDF提取 / PDF提取→AI重制 / AI生成）、prompt 草案，经用户确认后再动手生成
4. **风格对齐模板**: 所有 prompt 必须从目标模板的 design_spec Icon System / Style Anchor 中提取风格关键词，确保配图与 PPT 整体视觉一致
5. **分批交付**: 先生成 2-3 张让用户确认风格方向，通过后再批量生成剩余图片
6. **封面/尾页不生成**: 封面和尾页背景**永远使用模板已有素材**（`cover_bg.jpeg`），不需要 AI 生成。配图计划中不要包含封面/尾页背景图

### Phase 4 计划表格式

每张配图需列出以下信息供用户 Review：

```markdown
### 图N：[图片主题名]
- **用途**：对应章节 / 页面位置
- **类型**：Background / Illustration / Diagram（见 §七）
- **来源**：PDF提取 / PDF提取→AI重制 / AI生成
- **参考素材**：[仅「PDF提取→AI重制」时填写：提取文件路径，供 Agent 编写 prompt 时参考构图和内容]
- **Prompt**：[完整 prompt 草案（「PDF提取」来源可留空）]
- **Negative Prompt**：[负面提示词]
```

### AI 重制工作流（PDF提取 → AI重制）

当参考文稿中的图片**内容/构图有参考价值，但视觉风格不符合目标模板**时（如：照片风、3D 渲染、彩色渐变、深色背景等），采用"提取 → 分析 → 重制"三步流程：

#### 步骤

1. **提取并保留原图**
   - 按 xref / shape / inline 提取到 `<project>/images/pdf_extracted/` 目录
   - 不丢弃、不跳过——即使风格不匹配，原图仍是 prompt 编写的重要视觉参考

2. **Agent 分析原图内容**
   - 观察原图的**核心构图**（元素数量、空间布局、连接关系）
   - 识别原图的**关键视觉元素**（图标类型、图表形态、数据结构）
   - 提取原图表达的**信息层次**（主体 vs 辅助 vs 装饰）

3. **编写风格重制 Prompt**
   - 将分析结果翻译为 §六 对应图类型的 prompt 模板
   - **保留原图的构图逻辑和信息结构**，替换视觉风格为目标模板的 Style Anchor
   - 在计划表的 `参考素材` 字段记录原图路径，便于追溯

#### Prompt 编写要点

| 从原图继承的 | 替换为模板风格的 |
|-------------|----------------|
| 构图布局（左右分栏/环形/层叠） | 保留 |
| 元素数量与层级关系 | 保留 |
| 信息内容（图表数据方向、流程步骤） | 保留 |
| 配色方案 | → `#660874` 深紫 + `#1E8E94` 青色 + `#000000` 线稿 |
| 渲染风格（3D/照片/渐变） | → `flat vector, single-weight line-art, minimal detail` |
| 背景色（深色/彩色/纹理） | → `white background` |
| 复杂装饰/阴影/光晕 | → `clean corporate infographic style, no gradients, no shadows` |

#### 示例

**原图**：PDF 中提取的一张 3D 渲染芯片架构图（深蓝背景、彩色模块、光晕效果）

**分析**：中心为处理器 die，周围 4 个 HBM 显存模块，底部 PCIe 总线，顶部互联接口

**重制 Prompt**：
```
Flat vector network diagram of GPGPU chip architecture,
one central large square chip die in deep purple (#660874)
containing a 3x3 grid of small square processing units,
surrounded by four tall rectangular HBM memory stacks on left and right in black (#000000) outline,
PCIe bus as a wide horizontal bar at bottom,
high-speed interconnect ports as small rectangles at top,
teal (#1E8E94) signal pulses on connecting lines between components,
White background, single-weight line-art, minimal detail,
clean corporate semiconductor diagram, 16:9 aspect ratio
```

> 核心思路：**原图提供"画什么"，模板规范提供"怎么画"**。

---

## 二、通用风格锚点 (Style Anchor)

所有配图 prompt 必须包含以下风格约束（直接拼接在 prompt 末尾）：

```
Monochrome deep purple (#660874) as primary color with teal (#1E8E94) accent.
White background, single-weight line-art, minimal detail,
clean corporate infographic style, no gradients, no shadows,
professional semiconductor presentation illustration, 16:9 aspect ratio
```

> ⚠️ 封面/尾页背景**不生成**，直接使用模板素材 `cover_bg.jpeg`。

## 三、通用负面提示词 (Negative Prompt)

```
text, labels, numbers, words, photorealistic, 3D render, colorful rainbow,
busy details, watermark, dark background, gradients, shadows, glow effects, photo
```

## 四、通用参数

```bash
--aspect_ratio 16:9 --image_size 1K
```

输出分辨率: 1376×768 (16:9)

## 五、配色规则

| 角色 | 色值 | 用途 |
|------|------|------|
| 主色 | `#660874` (深紫) | 所有主要图形元素、边框、填充 |
| 强调色 | `#1E8E94` (青色) | 连接线、高亮、次要元素 |
| 亮绿 | `#48BB00` | 仅用于"增长/成功"语义的点缀（如投资增长箭头） |
| 辅助灰 | `#E5E6E9` | 模块背景填充、微弱边界、网格线 |
| 薰衣草灰 | `#A894B1` | 第三层级装饰 |
| 黑色线稿 | `#000000` | 图标默认描边色（与 MetaX body text 一致） |
| 背景 | `#FFFFFF` (白) | 内容页必须白底 |

### 配色使用优先级

1. **主要结构/强调元素** → `#660874` 深紫（每张图的视觉焦点）
2. **次要连接/高亮** → `#1E8E94` 青色（箭头、连接线、信号脉冲）
3. **普通图标描边** → `#000000` 黑色（非重点图标的默认线稿色）
4. **背景填充/网格** → `#E5E6E9` 浅灰（辅助结构，不抢视觉）
5. **第三层级装饰** → `#A894B1` 薰衣草灰（极少使用）

> ⚠️ 严格限制每张图只用 2-3 种色值。不要在一张图中使用全部色值。

## 六、Prompt 结构模板

### 通用结构

```
Flat vector [图类型] of [主题描述].
[具体视觉元素: 图标名称、形状、数量].
[布局: 元素的空间位置关系，使用 left/right/top/bottom/center].
[连接关系: 箭头/线条/流向].
[配色指定: deep purple (#660874) for [主要元素], teal (#1E8E94) for [次要元素], black (#000000) for [线稿]].
White background, single-weight line-art, minimal detail,
clean corporate infographic style, 16:9 aspect ratio
```

### 按图类型的模板

#### A. 概念对比图 (Comparison)

```
Flat vector split comparison infographic,
left side: [A概念的图标/视觉] in black (#000000),
right side: [B概念的图标/视觉] in deep purple (#660874)
with [连接/展开元素],
teal (#1E8E94) accent [点缀位置],
White background, single-weight line-art, minimal detail,
clean corporate infographic style, 16:9 aspect ratio
```

#### B. 时间线/里程碑 (Timeline)

```
Flat vector horizontal timeline infographic from [起始] to [终止],
[N] milestone nodes along a purple (#660874) horizontal line:
[节点1图标], [节点2图标], [节点3图标], ...,
each node is a simple circle with icon inside,
teal (#1E8E94) accent dots between nodes,
black (#000000) thin connecting lines,
White background, single-weight line-art, minimal detail,
clean corporate presentation diagram, 16:9 aspect ratio
```

#### C. 流程/管线图 (Pipeline)

```
Flat vector pipeline diagram of [主题],
left-to-right flow: [起点图标] → [中间阶段图标] → [终点图标],
connected by single-weight purple (#660874) directional arrows,
[装饰元素] running along [位置] as decorative element,
teal (#1E8E94) [高亮元素],
black (#000000) icons,
White background, single-weight line-art, minimal detail,
clean corporate infographic style, 16:9 aspect ratio
```

#### D. 闭环/循环图 (Loop)

```
Flat vector illustration of [主题] closed-loop system,
[N] stations arranged in a circular flow:
[站点1图标], [站点2图标], [站点3图标], [站点4图标],
connected by single-weight purple (#660874) directional arrows forming a continuous loop,
each station as a simple icon in black (#000000) outline
with one purple emphasis element,
teal (#1E8E94) flow indicators on arrows,
White background, minimal detail, clean corporate infographic style, 16:9 aspect ratio
```

#### E. 网络/生态图 (Network)

```
Flat vector network diagram of [主题],
one central [核心节点描述] in deep purple (#660874)
surrounded by [N] [外围节点描述] in black (#000000) outlines
arranged in a [hexagonal/circular/radial] pattern,
connected by single-weight purple line-art,
teal (#1E8E94) signal pulses on connecting lines,
each node is a simple circle with minimal icon inside,
White background, minimal detail, clean corporate infographic style, 16:9 aspect ratio
```

#### F. 数据/增长图 (Chart)

```
Flat vector [chart type] infographic,
[N] bars/elements [排列方式],
bars in deep purple (#660874) with the [关键元素] highlighted,
a rising trend arrow in teal (#1E8E94) [位置],
small [主题图标] at [位置] in black (#000000) single-weight line-art,
subtle grid lines in light gray (#E5E6E9),
White background, minimal detail, clean corporate investment presentation graphic,
16:9 aspect ratio
```

#### G. 科学概念图 (Scientific Concept)

```
Flat vector illustration of [科学概念],
a simplified [核心视觉: protein ribbon / globe / molecule] in deep purple (#660874)
single-weight line-art,
surrounded by teal (#1E8E94) [辅助元素: data scan lines / streams / connections],
[补充元素] in black (#000000) outlines,
White background, minimal detail, clean scientific infographic style,
16:9 aspect ratio
```

#### H. 天平/权衡图 (Balance)

```
Flat vector balance scale illustration,
left pan holds [正面元素图标] lifted higher,
right pan holds [负面元素图标] weighing slightly down,
fulcrum shaped as [中心意象] in deep purple (#660874),
scale structure in black (#000000) single-weight line-art,
teal (#1E8E94) accents on [强调侧],
White background, minimal detail, clean corporate infographic style,
16:9 aspect ratio
```

#### I. 层栈/架构图 (Layered Stack)

```
Flat vector layered architecture diagram of [主题],
[N] horizontal rectangular layers stacked vertically:
bottom layer: [底层名称] with [图标] icon,
middle layers: [各层名称 + 图标],
top layer: [顶层名称] with [图标] icon,
each layer in deep purple (#660874) with decreasing opacity from bottom to top,
teal (#1E8E94) vertical connectors linking adjacent layers,
black (#000000) single-weight outlines,
White background, minimal detail, clean corporate architecture diagram,
16:9 aspect ratio
```

#### J. 矩阵/分类格 (Matrix / Grid)

```
Flat vector [M×N] matrix grid layout of [主题],
[M] rows × [N] columns of rounded rectangular cells,
each cell contains a simple icon: [逐个列出图标],
[行/列标题方向] grouped by [分类维度],
cell borders in black (#000000) single-weight line-art,
[重点单元格] highlighted with deep purple (#660874) fill,
other cells in light gray (#E5E6E9) fill,
teal (#1E8E94) accent on [特殊标记/对角线/关键格],
White background, minimal detail, clean corporate classification diagram,
16:9 aspect ratio
```

#### K. 漏斗/筛选图 (Funnel)

```
Flat vector funnel diagram of [主题],
[N] horizontal bands narrowing from top to bottom:
top (widest): [初始阶段] with [图标],
middle bands: [中间筛选阶段 + 图标],
bottom (narrowest): [最终产出] with [图标],
funnel outline in deep purple (#660874) single-weight line-art,
each band slightly lighter shade using fill-opacity,
teal (#1E8E94) downward arrows between bands,
small filter/sieve icons on the right side at each transition,
black (#000000) icons inside each band,
White background, minimal detail, clean corporate infographic style,
16:9 aspect ratio
```

#### L. 维恩/交集图 (Venn / Intersection)

```
Flat vector Venn diagram of [主题],
[2-3] overlapping circles:
circle 1 ([概念A]): [图标] in deep purple (#660874) outline,
circle 2 ([概念B]): [图标] in teal (#1E8E94) outline,
[circle 3 ([概念C]): [图标] in black (#000000) outline,]
overlap region contains [交集概念图标] with subtle purple fill,
each circle drawn with single-weight line-art,
icons placed at each circle's center and overlap center,
White background, minimal detail, clean corporate infographic style,
16:9 aspect ratio
```

#### M. 金字塔/层级图 (Pyramid / Hierarchy)

```
Flat vector pyramid diagram of [主题],
[N] horizontal tiers from base to apex:
base (widest): [基础层] with [图标],
middle tiers: [各层名称 + 图标],
apex (smallest): [顶层/终极目标] with [图标],
pyramid outline in deep purple (#660874) single-weight line-art,
each tier separated by thin black (#000000) lines,
apex tier highlighted with deep purple (#660874) fill,
teal (#1E8E94) upward arrow along the left side indicating progression,
White background, minimal detail, clean corporate strategy diagram,
16:9 aspect ratio
```

#### N. 齿轮/协同图 (Gears / Mechanism)

```
Flat vector interlocking gears diagram of [主题],
[N] meshing gears of varying sizes:
large central gear: [核心机制] with [图标] inside, in deep purple (#660874),
smaller surrounding gears: [协同要素 + 图标] in black (#000000) outline,
gears visually interlocked with teeth meshing,
teal (#1E8E94) motion arc indicators showing rotation direction,
each gear has a simple icon at its center,
White background, single-weight line-art, minimal detail,
clean corporate mechanism diagram, 16:9 aspect ratio
```

#### O. 靶心/目标图 (Target / Bullseye)

```
Flat vector target bullseye diagram of [主题],
[N] concentric rings from outer to inner:
outer ring: [外围/基础层] in light gray (#E5E6E9),
middle rings: [中间层级 + 图标] in black (#000000) outline,
bullseye center: [核心目标/关键指标] with [图标] in deep purple (#660874) fill,
an arrow hitting the bullseye from [方向] in teal (#1E8E94),
small icons placed within each ring representing [各层含义],
White background, single-weight line-art, minimal detail,
clean corporate strategy diagram, 16:9 aspect ratio
```

#### P. 盾牌/安全图 (Shield / Security)

```
Flat vector shield diagram of [主题],
a large shield silhouette in deep purple (#660874) outline at center,
inside the shield: [核心保护对象图标] in deep purple fill,
surrounding the shield: [N] threat/challenge icons in black (#000000)
arranged around the perimeter ([具体威胁图标]),
teal (#1E8E94) defense lines radiating outward from shield edges,
small checkmark or lock icons at shield corners,
White background, single-weight line-art, minimal detail,
clean corporate security infographic style, 16:9 aspect ratio
```

#### Q. 路线图/路径图 (Roadmap / Path)

```
Flat vector winding roadmap diagram of [主题],
a curved S-shaped path from bottom-left to top-right,
[N] waypoint markers along the path:
waypoint 1: [起点阶段] with [图标],
waypoint 2: [阶段名称] with [图标],
waypoint 3: [阶段名称] with [图标],
waypoint N: [目标终点] with [图标] and a flag icon,
path line in deep purple (#660874) single-weight stroke with dashed segments,
waypoint circles in black (#000000) outline with icons inside,
teal (#1E8E94) directional chevrons along the path indicating direction,
a small compass icon in the corner,
White background, single-weight line-art, minimal detail,
clean corporate strategy roadmap style, 16:9 aspect ratio
```

#### R. 桥梁/连接图 (Bridge / Connection)

```
Flat vector bridge diagram of [主题],
left platform: [概念A / 现状] with [图标群] in black (#000000),
right platform: [概念B / 目标] with [图标群] in deep purple (#660874),
an arched bridge structure connecting both platforms in deep purple (#660874) outline,
bridge surface contains [N] small icons representing [连接要素/手段],
teal (#1E8E94) arrows flowing across the bridge from left to right,
a gap/chasm below the bridge suggesting the divide being bridged,
subtle grid lines in light gray (#E5E6E9) on platforms,
White background, single-weight line-art, minimal detail,
clean corporate infographic style, 16:9 aspect ratio
```

#### S. 拼图/整合图 (Puzzle / Integration)

```
Flat vector jigsaw puzzle diagram of [主题],
[N] interlocking puzzle pieces arranged in a [2×2 / 2×3 / irregular] layout:
piece 1: [要素A] with [图标] in deep purple (#660874) fill,
piece 2: [要素B] with [图标] in black (#000000) outline,
piece 3: [要素C] with [图标] in black (#000000) outline,
piece N: [要素D] with [图标] in teal (#1E8E94) outline,
pieces visually interlocked with classic jigsaw connector shapes,
one piece slightly offset or floating to suggest assembly in progress,
each piece contains a simple centered icon,
White background, single-weight line-art, minimal detail,
clean corporate integration diagram style, 16:9 aspect ratio
```

#### T. 雷达/能力图 (Radar / Capability)

```
Flat vector radar chart diagram of [主题],
a regular [N]-sided polygon wireframe with [N] axes radiating from center:
axis 1: [维度A], axis 2: [维度B], axis 3: [维度C], ...,
[2] concentric polygon outlines in light gray (#E5E6E9) as reference grid,
data polygon filled area in deep purple (#660874) with fill-opacity 0.15,
data polygon outline in deep purple (#660874) single-weight line-art,
[optional second data polygon in teal (#1E8E94) outline for comparison],
vertex dots at each data point in deep purple (#660874),
small icons at each axis endpoint in black (#000000),
White background, minimal detail, clean corporate assessment diagram,
16:9 aspect ratio
```

#### U. 树状/分支图 (Tree / Branch)

```
Flat vector tree diagram of [主题],
root node at [top/left]: [根节点/起点] with [图标] in deep purple (#660874),
first branch level: [N] child nodes ([子节点名称 + 图标]),
second branch level: [M] leaf nodes ([叶节点名称 + 图标]),
all nodes as simple circles or rounded rectangles in black (#000000) outline,
root node highlighted with deep purple (#660874) fill,
connecting lines as single-weight black branches with right-angle or curved paths,
teal (#1E8E94) accent on [关键路径/重点分支],
nodes decrease in size from root to leaf,
White background, single-weight line-art, minimal detail,
clean corporate decision tree / org chart style, 16:9 aspect ratio
```

#### V. 沙漏/转化图 (Hourglass / Transformation)

```
Flat vector hourglass diagram of [主题],
top chamber: [输入/初始状态] with [图标群] in black (#000000) outline,
narrow neck: [转化过程/瓶颈] with [过程图标] in deep purple (#660874),
bottom chamber: [输出/最终状态] with [图标群] in deep purple (#660874) fill,
hourglass frame in black (#000000) single-weight line-art,
teal (#1E8E94) particle dots flowing from top to bottom through the neck,
small decorative clock or timer icon beside the hourglass,
White background, single-weight line-art, minimal detail,
clean corporate transformation infographic style, 16:9 aspect ratio
```

#### W. 灯塔/引领图 (Lighthouse / Guidance)

```
Flat vector lighthouse diagram of [主题],
a simplified lighthouse tower at center in deep purple (#660874) outline,
light beam radiating from top in teal (#1E8E94) fan shape toward the right,
[N] small icons within the light beam representing [被引领的方向/目标]:
[图标1], [图标2], [图标3],
base of lighthouse surrounded by [基础要素图标] in black (#000000),
stylized wave lines at the bottom in light gray (#E5E6E9),
the lighthouse lens/lamp area highlighted with deep purple (#660874) fill,
White background, single-weight line-art, minimal detail,
clean corporate vision and leadership diagram style, 16:9 aspect ratio
```

#### X. 地图/定位图 (Map / Positioning)

```
Flat vector positioning map diagram of [主题],
a large quadrant grid with [X轴维度] horizontal axis and [Y轴维度] vertical axis,
axis lines in black (#000000) single-weight line-art,
subtle grid cells in light gray (#E5E6E9),
[N] positioning markers as circles of varying sizes:
[主体A] large circle in deep purple (#660874) in [象限位置],
[主体B] medium circle in teal (#1E8E94) in [象限位置],
[其他主体] small circles in black (#000000) outline scattered in other positions,
each marker contains a simple icon representing the entity,
arrow showing movement trajectory from current to target position,
White background, single-weight line-art, minimal detail,
clean corporate strategy positioning chart style, 16:9 aspect ratio
```

## 七、图片类型速查与选型指南

根据内容语义选择图类型：

| 内容语义 | 推荐图类型 | 模板 | 典型用途 |
|----------|-----------|------|----------|
| 封面/尾页 | **不生成** | — | 使用模板素材 `cover_bg.jpeg` |
| A vs B 对比 | Comparison | §六-A | 概念区分、新旧对比 |
| 演进/历程 | Timeline | §六-B | 技术发展、系统迭代 |
| 流程/管线 | Pipeline | §六-C | 药物研发、数据处理 |
| 闭环/循环 | Loop | §六-D | 实验循环、反馈系统 |
| 生态/架构 | Network | §六-E | 多Agent协作、技术栈 |
| 数据/增长 | Chart | §六-F | 投资、市场规模 |
| 科学概念 | Scientific | §六-G | 蛋白质、天气、分子 |
| 权衡/取舍 | Balance | §六-H | 风险vs机遇、利弊分析 |
| 分层/技术栈 | Layered Stack | §六-I | 软件架构栈、协议层、能力层级 |
| 产品/特性矩阵 | Matrix | §六-J | 产品组合、功能对标、分类对照 |
| 筛选/转化 | Funnel | §六-K | 候选药物筛选、用户转化、需求收敛 |
| 概念交叉/融合 | Venn | §六-L | HPC+AI 融合、跨学科交叉 |
| 战略层级/重要性 | Pyramid | §六-M | 能力金字塔、战略优先级 |
| 多方协同/联动 | Gears | §六-N | 软硬件协同、部门协作 |
| 聚焦/目标 | Target | §六-O | 核心KPI、精准定位、目标市场 |
| 安全/合规/防护 | Shield | §六-P | 数据安全、系统防护、合规体系 |
| 发展路径/阶段规划 | Roadmap | §六-Q | 产品路线图、技术演进路径、战略规划 |
| 跨越鸿沟/衔接 | Bridge | §六-R | 技术落地、产学研转化、跨领域衔接 |
| 要素整合/模块拼装 | Puzzle | §六-S | 解决方案组合、能力拼图、团队互补 |
| 多维评估/能力画像 | Radar | §六-T | 技术成熟度、竞品能力对比、人才画像 |
| 层级分类/决策分支 | Tree | §六-U | 组织架构、决策树、技术分类体系 |
| 转化/瓶颈/时间压力 | Hourglass | §六-V | 数据转化、资源瓶颈、研发周期压缩 |
| 愿景/引领/方向 | Lighthouse | §六-W | 技术愿景、行业引领、战略灯塔 |
| 竞争格局/市场定位 | Map | §六-X | 竞品定位、市场象限、战略位置分析 |

## 八、实战案例库

以下为经过验证的完整 prompt，可直接复用或微调。

### 案例1：概念对比（Agent vs 工具）

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Split comparison infographic, flat vector style: left side a single static gear icon in black (#000000) labeled area representing a passive tool, right side a glowing brain hub in deep purple (#660874) with radiating single-weight line-art connections to 5 surrounding tool icons (microscope, code bracket, robotic arm, database cylinder, document), arrows forming an iterative observe-orient-decide-act loop on the right side, teal (#1E8E94) arrow accents, white background, minimal detail, clean corporate infographic style, 16:9 aspect ratio" \
  -n "realistic, photography, 3D render, complex textures, watermark, dark background, gradient fills" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_agent_vs_model
```

**效果**：左侧静态齿轮（工具）vs 右侧辐射脑部（Agent）+ 迭代环路

### 案例2：时间线里程碑

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Horizontal timeline infographic from 2023 to 2026, flat vector style with single-weight line-art, five milestone nodes along a purple (#660874) horizontal line: chemistry flask icon, robotic arm icon, research paper icon, multi-node constellation icon, laboratory building icon, each node is a simple circle with icon inside, teal (#1E8E94) accent dots between nodes, black (#000000) thin connecting lines, white background, minimal detail, clean corporate presentation diagram, 16:9 aspect ratio" \
  -n "realistic, photography, 3D, complex textures, dark background, text labels, watermark, colorful" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_milestone_timeline
```

### 案例3：闭环系统（自驱实验室）

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Flat vector illustration of autonomous laboratory closed-loop system, four stations arranged in a circular flow: AI brain monitor, robotic arm with test tube, analysis instrument (X-ray icon), data chart display, connected by single-weight purple (#660874) directional arrows forming a continuous loop, each station as a simple icon in black (#000000) outline with one purple emphasis element, teal (#1E8E94) flow indicators on arrows, white background, minimal detail, clean corporate infographic style, 16:9 aspect ratio" \
  -n "realistic, photography, people, dark background, cluttered, watermark, 3D, gradient fills" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_self_driving_lab
```

### 案例4：流程管线（药物发现）

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Flat vector pipeline diagram of AI drug discovery, left-to-right flow: neural network brain icon generating molecular structures, flowing through a funnel/filter stage, then to a pill capsule with checkmark icon on the right, a simplified DNA strand running along the bottom as a decorative element, all in single-weight line-art, deep purple (#660874) pipeline structure, teal (#1E8E94) molecule accents, black (#000000) icons, white background, minimal detail, clean pharmaceutical infographic style, 16:9 aspect ratio" \
  -n "realistic, photography, human faces, dark background, text, watermark, cluttered, 3D render, colorful" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_drug_discovery
```

### 案例5：网络生态图（多Agent系统）

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Flat vector network diagram of multi-agent AI ecosystem, one central orchestrator node in deep purple (#660874) surrounded by 6 specialized agent nodes in black (#000000) outlines arranged in a hexagonal pattern (researcher, reviewer, critic, experimentalist, analyst, writer icons), connected by single-weight purple line-art, teal (#1E8E94) signal pulses on connecting lines, each node is a simple circle with minimal icon inside, white background, minimal detail, clean corporate infographic style, 16:9 aspect ratio" \
  -n "realistic, photography, human faces, dark background, cluttered, 3D render, watermark, gradient, colorful" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_multi_agent
```

### 案例6：科学概念（蛋白质结构）

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Flat vector illustration of protein structure prediction concept, a simplified protein ribbon (alpha helix and beta sheet shapes) in deep purple (#660874) single-weight line-art, surrounded by teal (#1E8E94) data scan lines revealing the 3D folding pattern, small molecular dots and amino acid chain segments in black (#000000) outlines, abstract AI analysis rays emanating from corner, white background, minimal detail, clean scientific infographic style, 16:9 aspect ratio" \
  -n "realistic photo, 3D render, dark background, text, watermark, cartoon characters, busy patterns, gradient" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_protein_structure
```

### 案例7：科学概念（天气/地球）

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Flat vector illustration of AI weather prediction, a simplified globe outline in black (#000000) single-weight line-art, overlaid with stylized wind current lines and cloud icons, an AI chip icon in deep purple (#660874) positioned beside the globe sending teal (#1E8E94) data streams wrapping around the atmosphere layer, small grid pattern overlay suggesting computational prediction, white background, minimal detail, clean scientific infographic style, 16:9 aspect ratio" \
  -n "realistic photo, text, watermark, cartoon characters, busy details, dark background, 3D render, gradient" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_weather_climate
```

### 案例8：数据增长图（投资）

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Flat vector financial growth infographic, ascending bar chart with 5 bars increasing in height from left to right, bars in deep purple (#660874) with the tallest bar highlighted, a rising trend arrow in teal (#1E8E94) arcing above the bars, small AI chip and molecule icons at the base of key bars in black (#000000) single-weight line-art, subtle grid lines in light gray (#E5E6E9), white background, minimal detail, clean corporate investment presentation graphic, 16:9 aspect ratio" \
  -n "realistic, photography, human faces, dark background, cluttered, text labels, watermark, 3D, colorful" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_investment
```

### 案例9：天平权衡图（风险 vs 机遇）

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Flat vector balance scale illustration, left pan holds opportunity icons (light bulb, DNA strand, upward arrow) lifted higher, right pan holds risk icons (warning triangle, broken link chain) weighing slightly down, fulcrum shaped as a simplified AI chip in deep purple (#660874), scale structure in black (#000000) single-weight line-art, teal (#1E8E94) accents on opportunity side, a thin sunrise horizon line in the background suggesting future outlook, white background, minimal detail, clean corporate infographic style, 16:9 aspect ratio" \
  -n "realistic, photography, human faces, text, watermark, busy patterns, dark background, 3D render, colorful" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_risk_future
```

### 案例10：软件生态层栈

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Flat vector infographic of a five-layer software ecosystem stack for GPU computing. Five horizontal rectangular layers stacked vertically with slight 3D isometric perspective: bottom layer is hardware silicon wafer, second layer is driver and firmware gears, third layer is computing libraries with mathematical symbols, fourth layer is frameworks with interconnected nodes, top layer is applications with domain icons (molecule, cloud, brain). Each layer in deep purple (#660874) with varying opacity (darkest at bottom, lighter at top). Teal (#1E8E94) vertical connectors linking all layers. White background, single-weight line-art, minimal detail, clean corporate infographic style, professional semiconductor presentation, 16:9 aspect ratio" \
  -n "text, labels, words, photorealistic, 3D render, colorful, busy, watermark, dark background, gradients, shadows" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_software_ecosystem
```

### 案例11：全景生态地图

**命令**：
```bash
python3 skills/ppt-master/scripts/image_gen.py \
  "Flat vector panoramic ecosystem infographic showing the complete HPC plus AI convergence landscape. Center: a large hexagonal hub icon representing the unified computing platform. Six satellite clusters radiate outward connected by single-weight lines: top-left cluster has molecule and crystal icons (materials science), top-right has cloud and wind icons (weather and climate), right has flask and DNA helix icons (drug discovery), bottom-right has car and turbine icons (engineering CFD), bottom-left has factory and robot arm icons (industrial AI), left has server rack and network icons (data center). Each cluster enclosed in a subtle circular boundary. Deep purple (#660874) for all icons and structures, teal (#1E8E94) for connection lines and the central hub accent. White background, single-weight line-art, minimal detail, clean corporate ecosystem map style, 16:9 aspect ratio" \
  -n "text, labels, words, photorealistic, 3D, colorful, busy, watermark, dark background, gradients, shadows" \
  --aspect_ratio 16:9 --image_size 1K \
  -o <project>/images -f img_ecosystem_panorama
```

## 九、经验总结

### Prompt 编写要诀

1. **具体物件优于抽象概念**：用 `server rack, brain-circuit, flask, crystal lattice, robotic arm, pill capsule` 而不是 `computing, intelligence, science, medicine`。抽象概念导致 AI 生成空洞或偏题的图
2. **必须禁止文字**：negative prompt 中必须包含 `text, labels, numbers, words`，否则 Gemini 会在图中生成乱码/拼错的文字
3. **色值必须写 HEX**：明确写 `deep purple (#660874)` 和 `teal (#1E8E94)`，不能只说 "purple and teal"，否则 AI 会选择偏离品牌色的颜色
4. **方位词定布局**：使用 `left/right/top/bottom/center/surrounding/radiating` 明确元素位置关系，避免 AI 随意排布
5. **风格尾缀必不可少**：每条 prompt 必须以 `single-weight line-art, minimal detail, clean corporate infographic style` 结尾，确保系列图片风格一致
6. **内容丰富度平衡**：prompt 不能过于简单（"几个方框"→ 空洞），也不能堆砌过多元素（→ 混乱）。一张图描述 3-6 个核心视觉元素为宜

### 配色陷阱

7. **严控色彩数量**：每张图实际使用 **2-3 种色值**（主色 + 强调色 + 线稿色）。把五六种颜色全塞进一张图 → 花哨杂乱
8. **黑色是默认线稿色**：非重点图标用 `black (#000000)` 描边，只把 `#660874` 留给需要强调的核心元素
9. **浅灰只做背景网格**：`#E5E6E9` 仅用于 `subtle grid lines` 或 `background fill`，不要用作图标色

### 生成与自审

10. **逐张生成不并行**：一次只调一条 `image_gen.py` 命令，等文件落盘确认后再生成下一张
11. **落盘后立即自审**：检查文件大小是否合理（>50KB）、分辨率是否正确（1376×768）。过小的文件（<30KB）通常意味着生成失败或内容过于简单
12. **分批交付**：先生成 2-3 张给用户确认风格方向，通过后再批量生成剩余图片。避免全部生成后再返工
13. **不合格即重做**：如果自审发现图片过于空洞（只有几个方框、缺乏图标细节），应立即用更丰富的 prompt 重新生成，不要交给用户判断。典型失败模式：prompt 只描述抽象几何形状而没有具体图标/物件
14. **PDF 提取的矢量图常为黑图**：PDF 中嵌入的矢量图（非位图）用 PyMuPDF xref 提取后经常全黑。遇到此情况改用 `page.get_pixmap(matrix, clip)` 裁剪内容区域渲染，或标记为 AI 重制

### 提取 → AI 重制

15. **风格不匹配不等于无用**：参考文稿中的图片即使是照片风/3D 渲染/深色背景/彩色渐变，只要内容和构图有参考价值就应提取保留，用于指导 AI 重制 prompt 的编写
16. **继承构图、替换风格**：重制时保留原图的元素数量、空间布局和信息层次，仅替换配色（→ 深紫+青色+黑色线稿）、渲染风格（→ flat vector）、背景（→ 白色）
17. **提取目录与最终目录分离**：原图提取到 `images/pdf_extracted/`，AI 重制的成品输出到 `images/`，避免混淆。提取目录仅作 Agent 编写 prompt 时的视觉参考，不在 PPT 中使用
18. **计划表中标注参考来源**：`来源` 标记为 `PDF提取 → AI重制`，`参考素材` 字段填写提取文件路径，便于用户 Review 时理解重制依据
