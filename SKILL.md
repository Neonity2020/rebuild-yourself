# Rebuild.AI 项目开发与排版规范

本文件记录了本项目的核心排版规范、技术架构要点与样式开发约定，以便未来的开发者或 AI 协同助手高效维护。

---

## 1. Markdown / MDX 排版与加粗规范

在撰写 `.md` 或 `.mdx` 文章时，中文加粗有特定的规范：

> [!IMPORTANT]
> **中文加粗格式约定**：
> 由于 Markdown 解析引擎（Remark）遵循 CommonMark 规范，在解析紧贴中文字符或中文全角标点（如 `“`、`”`、`《`、`》`）的双星号加粗标记时，正则边界判断会失效，导致页面上直接渲染出 `**` 字符（例如 `**“认知劳动”**` 无法正确加粗）。
> 
> **通用解决规范**：
> - 在中文文章中，需要对紧挨着中文全角符号或汉字的词语进行加粗时，**必须直接使用 HTML 的 `<strong>` 标签**来进行包覆。
> - **示例**：
>   - *不推荐（可能解析失败）*：`我称之为**“认知外包”**。`
>   - *推荐（100% 成功解析）*：`我称之为<strong>“认知外包”</strong>。`

---

## 2. 配色与设计规范 (Design Specification)

为了保持高水准的现代视觉质感，本项目建立了完整的视觉与交互设计系统。关于全站的核心配色代号、暗/亮色主题色值、字体排版比例、玻璃拟态类（`.glass`）、胶囊徽章（`.badge`）的样式约定以及 AI 配图艺术风格，**请直接参阅详尽的 [DESIGN.md](file:///Users/andi/Documents/Projects/rebuild-yourself/DESIGN.md) 文档**。

---

## 3. 动态元素样式规范 (Global Style Scoping)

*   **JS 动态节点的样式继承**：
    Astro 的 `<style>` 标签默认是 Scoped（局部作用域）的。这意味着像分页器（Pagination）、弹出卡片等**在客户端通过 JS 动态创建的 DOM 节点**（`document.createElement`），在编译时无法获得 Astro 的 Scope 标识属性（例如 `data-astro-cid-xxxx`），导致局部样式失效。
*   **开发约定**：
    凡是涉及到客户端 JS 动态生成的 HTML 元素样式，**必须将其书写在独立的 `<style is:global>` 标签中**，以确保浏览器能正确应用全局 CSS。

---

## 4. 外部链接跳转规范 (Rehype External Links)

*   **配置规则**：
    为了优化 SEO 权重并防止跳转流失，博客所有 Markdown/MDX 正文中的外部链接均需在**新标签页中打开**。
*   **实现机制**：
    我们在 [`astro.config.mjs`](file:///Users/andi/Documents/Projects/rebuild-yourself/astro.config.mjs) 中配置了自定义 Rehype 插件 `rehypeExternalLinks`。它会自动拦截所有以 `http://` 或 `https://` 开头的 `<a>` 标签，并注入 `target="_blank"` 与 `rel="noopener noreferrer"` 属性。
*   **注意点**：
    Astro 的 MDX 插件会自动继承 Markdown 渲染器的处理器配置，因此无需在 `mdx()` 集成中重复声明。

---

## 5. 图片资产创作与优化工作流 (Image Workflow)

为了提供 WOW 的视觉体验并兼顾加载性能，所有文章配图和 UI 插图必须遵循以下流水线：

1.  **AI 概念图创作**：
    *   避免低级的扁平插画。应使用极具现代艺术感、3D 渲染质感、或高科技抽象感的视觉设计。
    *   色彩上必须严格呼应 “Phoenix Ember” 的暖色调（黑曜石暗背景 + 暖铜/琥珀金能量线），可点缀少许荧光青色（Cyan）或板岩灰（Slate Gray）作为科技感对比。
2.  **格式转换优化 (PNG $\rightarrow$ JPEG)**：
    *   AI 绘图工具生成的原始图片通常为无损 PNG 格式，体积巨大。
    *   必须在命令行中通过 macOS 内置的 `sips` 图像处理工具将其转换为高压缩比的 JPEG 格式：
        ```bash
        sips -s format jpeg input.png --out output.jpg
        ```
    *   完成转换后，务必清理（删除）原 PNG 文件以保持代码库整洁。
3.  **Astro 图像管线编译 (WebP)**：
    *   在 Markdown/MDX 的 frontmatter 中引用最终的 `.jpg` 路径。
    *   Astro 构建管线（Vite & Sharp）会在编译阶段自动将其输出为经过高保真压缩的 WebP 资源，大幅减少网络传输开销。

---

## 6. 中文化与技术名词保留规范 (Localization & Terms)

当对主页、评测模块、或导航栏等 UI 页面进行中文化本地化时，必须遵循以下双语保留准则：

1.  **专有名词保留英文**：
    *   凡是业界通用的前沿技术名词、专有名词，**一律直接保持英文书写**（或使用括号中英对照形式），避免因生硬翻译而显得业余。
    *   **重点保留名词清单**：
        - `AI` (不翻译为“人工智能”)
        - `LLM` / `Agents` / `Prompt` / `Prompt Engineering`
        - `Agentic Workflows` (智能体工作流)
        - `DAG` (有向无环图) / `State Machine` (状态机)
        - `Human-in-the-Loop` (半自动人机协同)
        - `Taste` (品味/审美) / `Edge` (竞争壁垒)
        - `Ground Truth` (地面真理/底层事实)
2.  **中文排版空格习惯**：
    *   在中文与英文字符、数字、或代码标签之间，**需保持留有一个半角空格的良好书写习惯**，使页面呈现更加美观。
    *   *示例*：`你处于 AI 适应度的第一阶段，建议每天花 10 分钟使用 AI 辅助日常工作。`
