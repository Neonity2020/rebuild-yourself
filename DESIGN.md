# Rebuild.AI 视觉设计规范与设计系统 (Design System)

本文件定义了 **Rebuild.AI**（如何在 AI 时代重塑自我）的品牌视觉风格、UI 组件规范、微交互逻辑以及 AI 图像艺术指导，作为项目视觉设计与重塑的唯一事实来源。

---

## 1. 品牌概念与设计哲学 (Design Philosophy)

*   **设计隐喻：淬炼与重塑**
    Rebuild.AI 的核心理念是人类在人工智能时代的自我迭代与重构。设计上采用“熔炉与火光”的物理隐喻——在黑暗冷冽的计算世界中，人类独特的思想如同燃烧的凤凰余烬，经受淬炼并爆发出生机。
*   **三大设计核心**：
    1.  **极高对比度（High Contrast）**：纯黑背景与亮白文字、亮橙渐变形成高视觉张力。
    2.  **玻璃拟态（Glassmorphism）**：通过磨砂玻璃面板增加界面的通透感和层级深度。
    3.  **微光律动（Micro-glow & Animations）**：运用平滑的过渡与发光效果为静态内容注入生命力。

---

## 2. 配色系统 (Color Palette - "Phoenix Ember")

配色旨在营造高端、深邃的技术沙龙质感，杜绝千篇一律的 AI 科技蓝紫色调。

### 2.1 核心颜色令牌 (Theme Tokens)

| Token 变量名 | 色值 (HEX / RGB) | 视觉用途 |
| :--- | :--- | :--- |
| `--accent-primary` | `#f97316` / `rgb(249, 115, 22)` | 赤陶暖铜（全站第一主色，用于高亮、主按钮） |
| `--accent-secondary`| `#d97706` / `rgb(217, 119, 6)` | 凤凰琥珀金（辅色，渐变终止色） |
| `--accent-cyan` | `#06b6d4` / `rgb(6, 182, 212)` | 极客荧光青（用于中立高亮、进度条、评测标签） |
| `--accent-gradient` | `linear-gradient(135deg, #f97316 0%, #d97706 100%)` | 渐变火光（用于大字标题、主操作按键背景） |

### 2.2 模式主题色值对比 (Dark & Light Theme)

#### 暗色模式 (Dark Theme - 默认)
*   **全站背景底色** (`--bg-primary`): `#09090b` (纯净黑曜石碳黑)
*   **卡片/容器背景** (`--bg-secondary`): `rgba(24, 24, 27, 0.65)` (炭黑磨砂玻璃)
*   **深色填充/输入框** (`--bg-tertiary`): `#27272a`
*   **文字主色** (`--text-primary`): `#fafafa` (亮白，提供极高对比度)
*   **文字辅色** (`--text-secondary`): `#a1a1aa` (温和浅灰，降低长时间阅读疲劳)
*   **文字暗色/辅助** (`--text-muted`): `#71717a` (低饱和度灰色)
*   **边框颜色** (`--border-color`): `rgba(255, 255, 255, 0.08)` (微弱玻璃板边缘)

#### 亮色模式 (Light Theme)
*   **全站背景底色** (`--bg-primary`): `#f4f4f5` (温润浅灰白)
*   **卡片/容器背景** (`--bg-secondary`): `rgba(255, 255, 255, 0.75)` (亮白磨砂玻璃)
*   **深色填充/输入框** (`--bg-tertiary`): `#e4e4e7`
*   **文字主色** (`--text-primary`): `#09090b` (黑曜石黑)
*   **文字辅色** (`--text-secondary`): `#3f3f46` (深灰)
*   **文字暗色/辅助** (`--text-muted`): `#71717a` (中灰)
*   **边框颜色** (`--border-color`): `rgba(9, 9, 11, 0.08)`

### 2.3 代码区块高对比度规范 (Code Contrast Specification)

为了保证代码片段在浅色和深色主题下都具备优异的易读性（符合 WCAG AA 级以上无障碍标准），设计系统采用了以下特定处理：

*   **行内代码 (`code`)**：
    - **深色模式**：背景采用 `rgba(var(--accent-cyan-rgb), 0.1)`，文字采用 `--accent-cyan`（亮橙色），对比度符合深底高亮标准。
    - **浅色模式**：为了避免亮橙色在白色背景上过度洗白（对比度不足），行内代码文字统一变更为深红铜色 (`#9a3412`)，背景加深至 `rgba(234, 88, 12, 0.08)`，对比度达到 **AAA 级** 极佳可读范围。
*   **独立代码块 (`pre`)**：
    - **深色模式**（默认）：代码块保持深色背景（使用 Shiki `github-dark` 的背景色），代码文本显示亮色语法高亮颜色。
    - **浅色模式**（有 `.light` 类时）：代码块转为纯白色背景 (`#ffffff`），其内部的代码文本和语法高亮 Token 自动调整为深色语法高亮系统（使用 Shiki `github-light` 的深色语法字色），保证在白底背景下依然清晰好读，完美契合浅色阅读氛围。

---

## 3. 字体系统 (Typography)

全站字体分为三个层级，兼顾展示度与长文本阅读舒适感。

*   **标题与展示字体 (Header / Display)**:
    - 采用 **Outfit** 字体（粗度范围：`700` 至 `850`）。
    - 视觉特色：字形几何感强，字母饱满圆润，拥有极佳的科技时尚感与视觉重量感。
*   **正文阅读字体 (Body Copy)**:
    - 采用 **Inter** 字体（粗度范围：`400` 至 `600`）搭配标准无衬线系统字体。
    - 视觉特色：高字高设计，字符间距紧凑适中，专为屏幕排版阅读优化。
*   **代码与数据字体 (Monospace)**:
    - 采用 **Fira Code** 字体。
    - 视觉特色：自带代码连字（Ligatures）效果，逻辑结构清晰。

---

## 4. 视觉修饰与质感组件 (Visual Utilities)

### 4.1 玻璃拟态卡片 (.glass)
为了创造半通透的悬浮物理质感，统一采用以下 CSS 类属性定义：
```css
.glass {
	background: var(--bg-secondary);
	backdrop-filter: blur(12px);
	-webkit-backdrop-filter: blur(12px);
	border: 1px solid var(--border-color);
	box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.2);
}
```

### 4.2 发光悬浮动效 (.glass-hover)
鼠标悬停在卡片或按钮上时，通过边界颜色亮化与弥散阴影来增加微光互动：
```css
.glass-hover {
	transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1),
	            border-color 0.3s ease,
	            box-shadow 0.3s ease;
}
.glass-hover:hover {
	transform: translateY(-4px);
	border-color: rgba(249, 115, 22, 0.3); /* 暖铜微发光 */
	box-shadow: 0 12px 40px 0 rgba(249, 115, 22, 0.08);
}
```

### 4.3 胶囊徽章 (.badge)
主要用于分类标识、路线图卡片标头。其排版必须是等距的等宽风格：
```css
.badge {
	display: inline-block;
	padding: 0.25rem 0.75rem;
	border-radius: 9999px;
	font-family: var(--font-display);
	font-size: 0.75rem;
	font-weight: 700;
	text-transform: uppercase;
	letter-spacing: 0.05em;
	background: rgba(249, 115, 22, 0.1);
	color: var(--accent-primary);
	border: 1px solid rgba(249, 115, 22, 0.2);
}
```

---

## 5. 交互界面与微动画规范 (Micro-interactions)

1.  **全局缓动函数**：
    凡是涉及到位置、大小或阴影改变的过渡，统一使用 `cubic-bezier(0.4, 0, 0.2, 1)`（平滑加速再减速）以确保符合物理运动直觉，持续时间设定在 `250ms` 到 `350ms` 之间。
2.  **圆形评分图表 (Radial Gauge)**：
    评测结果仪表盘采用 SVG 构建。前景色通过 `stroke-dashoffset` 随得分比例动态改变。动画执行应在渲染 100ms 后触发，并通过数字由 0 开始累加的动画营造“加载校验中”的紧迫技术感。
3.  **阅读进度条**：
    文章详情页的顶部阅读进度条采用 `--accent-gradient` 渐变，并添加微弱的发光阴影（`box-shadow: 0 0 8px rgba(var(--accent-cyan-rgb), 0.5)`），给读者提供精确、平滑的内容定位锚点。

---

## 6. AI 图像艺术指导规范 (AI Art Direction)

在为文章或模块生成配图时，为了保证全站图像的美学一致性，**必须**严格遵循以下 Prompt 风格指南：

*   **核心关键词组**：
    - `premium cinematic illustration` (电影质感数码艺术)
    - `cybernetic / high-tech abstract aesthetic` (高科技抽象美学)
    - `octane render style` (高质感光影折射渲染)
    - `dark obsidian background` (深邃黑曜石背景)
*   **构图冲突对比 (Contrast Logic)**：
    - **中心温暖区**：画面中央应为亮眼的、充满生机的、高密度的发光橙色/金色的有机结构（如大脑脉络、几何结晶、发光能量线），代表**人类独特的思考、品味与 Ground Truth**。
    - **四周冰冷区**：画面的边缘或对比面应为冷灰、暗色、甚至像积木一样扁平的、规则重复的数字矩阵或正态分布曲线，代表**AI 的统计概率平均值与平庸同化力**。
*   **输出控制**：
    - 宽高比固定为 `16:9`。
    - 生成 PNG 后，必须调用系统工具将其转化为 JPEG 并保持中低体积，以兼顾网站运行速度。
