---
name: rebuild-yourself-dev
description: Guidelines for designing, building, formatting, and replicating the Rebuild.AI Astro website, including design systems, markdown rules, image pipelines, and codebase blueprints.
---

# Rebuild.AI Design, Development & Replication Guide

This guide outlines the brand identity, styling rules, typography, markdown formatting guidelines, and replication blueprints for building and maintaining the Rebuild.AI Astro codebase.

---

## 1. Project Replication Guide (Bootstrapping Pipeline)

To recreate this Astro blog from scratch in a new folder, follow these steps:

### 1.1 Project Structure Setup
Create a new directory and build the following structure:
```text
rebuild-yourself/
├── public/
│   ├── favicon.svg
│   └── favicon.ico
├── src/
│   ├── assets/
│   │   └── blog-placeholder-1.jpg
│   ├── components/
│   │   ├── BaseHead.astro
│   │   ├── Footer.astro
│   │   ├── FormattedDate.astro
│   │   ├── Header.astro
│   │   ├── HeaderLink.astro
│   │   └── InteractiveAssessment.astro
│   ├── layouts/
│   │   └── BlogPost.astro
│   ├── pages/
│   │   ├── index.astro
│   │   ├── about.astro
│   │   ├── rss.xml.js
│   │   └── blog/
│   │       ├── index.astro
│   │       └── [...slug].astro
│   ├── styles/
│   │   └── global.css
│   ├── content/
│   │   └── blog/
│   ├── consts.ts
│   └── content.config.ts
├── astro.config.mjs
├── tsconfig.json
└── package.json
```

### 1.2 Configuration Setup
1. Copy the contents of `package.json`, `tsconfig.json`, `astro.config.mjs`, `src/content.config.ts`, and `src/consts.ts` from [configs.md](file:///Users/andi/Documents/Projects/rebuild-yourself/.agents/skills/rebuild-yourself-dev/references/configs.md).
2. Run `npm install` to install Astro and its dependencies.

### 1.3 Styles, Layouts & Components Setup
1. Create `src/styles/global.css` and paste the full stylesheet from [styles.md](file:///Users/andi/Documents/Projects/rebuild-yourself/.agents/skills/rebuild-yourself-dev/references/styles.md).
2. Implement layouts and components by copying their code from [layouts_and_components.md](file:///Users/andi/Documents/Projects/rebuild-yourself/.agents/skills/rebuild-yourself-dev/references/layouts_and_components.md).

### 1.4 Pages & Content Setup
1. Copy the code for page routes (`index.astro`, `blog/index.astro`, `blog/[...slug].astro`, and `about.astro`) from [pages.md](file:///Users/andi/Documents/Projects/rebuild-yourself/.agents/skills/rebuild-yourself-dev/references/pages.md).
2. Add Markdown/MDX posts under `src/content/blog/` following the writing rules in Section 2.
3. Start the dev environment using `npm run dev`.

---

## 2. Markdown & MDX Writing Rules

### 2.1 Chinese Bolding Formatting
Due to Remark regex boundary issues in markdown parsing of Chinese characters next to full-width punctuation, double stars (`**`) can fail to render.
- **Rule**: When bolding text in a Chinese context that is adjacent to full-width punctuation or Chinese characters, always use the HTML `<strong>` tag.
- **Good**: `我称之为<strong>“认知外包”</strong>。`
- **Bad**: `我称之为**“认知外包”**。`

### 2.2 Localization & Technical Terms
Keep industry-standard technical terms in English (or use English with Chinese translation in parentheses).
- **Keywords**: `AI`, `LLM`, `Agents`, `Prompt`, `Prompt Engineering`, `Agentic Workflows`, `DAG`, `State Machine`, `Human-in-the-Loop`, `Taste`, `Edge`, `Ground Truth`.
- **Spacing**: Add exactly one half-width space between Chinese characters and English words, numbers, or code blocks.
  - *Example*: `你处于 AI 适应度的第一阶段，建议每天花 10 分钟使用 AI 辅助日常工作。`

---

## 3. Design System & Styling (Phoenix Ember Theme)

### 3.1 Theme Tokens

| Variable | Value | Purpose |
| :--- | :--- | :--- |
| `--accent-primary` | `#f97316` / `rgb(249, 115, 22)` | Primary warm copper/terracotta accent |
| `--accent-secondary`| `#d97706` / `rgb(217, 119, 6)` | Phoenix amber accent |
| `--accent-cyan` | `#06b6d4` / `rgb(6, 182, 212)` | Tech cyan accent |
| `--accent-gradient` | `linear-gradient(135deg, #f97316 0%, #d97706 100%)` | Heading / Button gradient |

### 3.2 Code Block Contrast (WCAG AA Accessibility)
- **Inline Code (`code`)**:
  - **Dark Mode**: Background `rgba(var(--accent-cyan-rgb), 0.1)`, Text `--accent-cyan`.
  - **Light Mode**: Text `#9a3412` (deep copper), Background `rgba(234, 88, 12, 0.08)` (ensures AAA-level contrast).
- **Block Code (`pre`)**:
  - **Dark Mode**: Dark background (Shiki `github-dark`), light syntax highlight.
  - **Light Mode**: White background (`#ffffff`), dark syntax highlight (Shiki `github-light`).

### 3.3 Key Utility Classes
- **Frosted Glass (`.glass`)**:
  ```css
  .glass {
    background: var(--bg-secondary);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    border: 1px solid var(--border-color);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.2);
  }
  ```
- **Hover Micro-interaction (`.glass-hover`)**: Adds translation and warm copper glow on hover.
- **Pill Badge (`.badge`)**: Capsule badge with uppercase spacing and accent background.

---

## 4. Astro Layout & Style Scoping Rules

### 4.1 Dynamic Elements Style Scoping
Astro `<style>` tags are scoped at build-time.
- **Rule**: Put styles for JavaScript-generated dynamic DOM nodes (e.g., in `InteractiveAssessment.astro`) in a `<style is:global>` block inside the component so client-side created elements inherit them correctly.

### 4.2 External Link Routing
- **Rule**: All external links in blog posts (starting with `http://` or `https://`) should open in a new tab with `rel="noopener noreferrer"`. Handled globally via `rehypeExternalLinks` in `astro.config.mjs`.

---

## 5. Image Optimization and Generation Workflow

### 5.1 Art Direction Prompts for AI-Generated Assets
Always construct prompts using these rules:
- **Style Tags**: `premium cinematic illustration`, `cybernetic / high-tech abstract aesthetic`, `octane render style`, `dark obsidian background`.
- **Composition**:
  - **Center**: Glowing warm orange/gold organic/geometric structure (human thoughts/original taste/Ground Truth).
  - **Edges**: Cold gray, dark, flat repetitive block/matrix structures (AI statistical averages).
- **Aspect Ratio**: Always `16:9`.

### 5.2 Image Compression Pipeline
1. Save the generated PNG in the target asset directory.
2. Run `sips` in macOS terminal to convert it to high-compression JPEG:
   ```bash
   sips -s format jpeg input.png --out output.jpg
   ```
3. Delete the original PNG. Astro automatically builds optimized `.jpg` into WebP.
