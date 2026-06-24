# Rebuild.AI - 如何在 AI 时代重塑自我

> **Rebuild.AI** 是一个面向 AI 时代的个人成长、认知增强与人机协同博客。它不仅是一个内容发布平台，更是一个倡导“智能编排（AI Orchestration）”的自我重构实验室。

---

## ✨ 项目特色 (Features)

*   🎨 **Phoenix Ember 独创主题**：以黑曜石深黑（Obsidian Dark）为基底，融合暖铜与凤凰琥珀金的渐变微光，打破千篇一律的科技蓝色调，带来极高视觉张力与沉浸式阅读质感。
*   📊 **交互式自我评估系统**：内置一分钟自我评测组件（Interactive Assessment），根据用户作答动态计算 **AI 时代复原力指数 (AI Era Resilience Index)** 并给出品味与重塑行动建议。
*   ⚙️ **暗/亮主题随心切换**：支持亮色（Warm Slate）与暗色模式的无缝瞬时切换，并对代码区块、行内高亮做了针对性的可读性优化（符合 WCAG AA 级对比度无障碍标准）。
*   📖 **极致的阅读排版系统**：
    *   **阅读进度指示器**：顶部平滑律动的渐变进度条。
    *   **目录监听（Scroll-spy TOC）**：自动提取 Markdown 文章 Heading 并在侧边栏动态高亮当前段落。
    *   **Katex 公式支持**：原生加载 LaTeX 数学公式渲染引擎，保障学术/公式类内容完美呈现。
*   🤖 **Agent 友好（Custom Workspace Skill）**：内置符合 Claude Code 范式的自定义 Workspace 技能库（位于 `.agents/skills/`），使未来的 AI 协同助手或开发者能以 100% 的规范度一键复现或扩展此博客。

---

## 🛠️ 项目目录结构 (Directory Structure)

```text
rebuild-yourself/
├── .agents/                    # Workspace Customizations (Agent 技能库)
│   └── skills/
│       └── rebuild-yourself-dev/
│           ├── SKILL.md        # AI 开发设计规范与复现引导
│           └── references/      # 复现全站文件的源码蓝图模版
├── src/
│   ├── assets/                 # 静态图片资源
│   ├── components/             # 核心组件库
│   │   ├── BaseHead.astro      # 公共 Meta 与 SEO 头
│   │   ├── Header.astro        # 包含主题切换、自适应菜单的导航栏
│   │   ├── Footer.astro        # 页脚
│   │   └── InteractiveAssessment.astro # 自我评测交互组件
│   ├── content/                # 博客内容集合 (MD/MDX)
│   ├── layouts/
│   │   └── BlogPost.astro      # 博客文章详情排版布局
│   ├── pages/                  # 页面路由
│   │   ├── index.astro         # 响应式极简主页
│   │   ├── about.astro         # 自我重构宣言页
│   │   └── blog/               # 博客列表与详情渲染路由
│   └── styles/
│       └── global.css          # “Phoenix Ember” 核心设计系统样式
├── astro.config.mjs            # Astro 配置文件 (集成了自定义外链 Rehype 插件)
├── tsconfig.json               # TypeScript 配置
└── package.json
```

---

## 🚀 快速开始 (Getting Started)

### 环境要求
*   Node.js >= `22.12.0`
*   Package Manager: `npm`

### 安装与启动
1.  **克隆或复制项目到本地**
2.  **安装项目依赖**：
    ```bash
    npm install
    ```
3.  **启动本地开发服务器**：
    ```bash
    npm run dev
    ```
    服务器将在 [http://localhost:4321](http://localhost:4321) 运行。

### 编译构建
*   **生产打包**：
    ```bash
    npm run build
    ```
    生成的高保真静态站点将输出至 `./dist/` 目录。
*   **本地预览生产包**：
    ```bash
    npm run preview
    ```

---

## 📝 编写与排版规范

如果您（或您的 AI 协同助手）正在为 Rebuild.AI 撰写文章或调整代码，请遵循以下核心规范（详见 [.agents/skills/rebuild-yourself-dev/SKILL.md](file:///Users/andi/Documents/Projects/rebuild-yourself/.agents/skills/rebuild-yourself-dev/SKILL.md)）：

1.  **中文加粗**：由于 Remark 解析器边界问题，中文句式中紧贴全角标点符号（如 `“`、`《`）的词语加粗**必须使用 HTML 标签 `<strong>`**，不要使用 `**`。
2.  **空格习惯**：中文字符与英文单词、数字、代码块之间需留有**一个半角空格**。
3.  **技术名词**：专业技术名词（如 `AI`、`LLM`、`Agents`、`Prompt`）**一律保持英文书写**，不进行生硬汉化。
4.  **外链拦截**：博客详情页面的所有 Markdown 外部链接将由 Rehype 插件自动拦截并注入 `target="_blank"` 与 `rel="noopener noreferrer"`。
