# Page Blueprints for Rebuild.AI

Below are the complete Astro page route components.

## 1. `src/pages/index.astro`
```astro
---
import BaseHead from '../components/BaseHead.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import InteractiveAssessment from '../components/InteractiveAssessment.astro';
import FormattedDate from '../components/FormattedDate.astro';
import { getCollection } from 'astro:content';
import { SITE_TITLE, SITE_DESCRIPTION } from '../consts';

const posts = (await getCollection('blog'))
	.sort((a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf())
	.slice(0, 3);
---

<!doctype html>
<html lang="zh-CN">
	<head>
		<BaseHead title={`${SITE_TITLE} | Rebuild Yourself`} description={SITE_DESCRIPTION} />
	</head>
	<body>
		<Header />
		<main>
			<!-- Hero Section -->
			<section class="hero">
				<div class="hero-glow hero-glow-cyan"></div>
				<div class="hero-glow hero-glow-violet"></div>
				
				<div class="hero-content">
					<div class="badge">人机协同 | Human + AI</div>
					<h1 class="hero-title">如何在 AI 时代 <span class="gradient-text">重塑自我</span></h1>
					<p class="hero-lead">探索认知增强、智能体工作流与人类独特价值的共生之道，在软件主导的未来中确立自己的核心坐标。</p>
					
					<div class="hero-actions">
						<a href="/blog" class="btn btn-primary">开始阅读</a>
						<a href="#assessment" class="btn btn-secondary">参与评测</a>
					</div>
				</div>
			</section>

			<!-- Pillars Section -->
			<section class="pillars">
				<div class="section-header">
					<div class="badge">行动路线图 | The Roadmap</div>
					<h2 class="section-title">三大核心重塑支柱</h2>
					<p class="section-subtitle">我们专注于在机器智能时代实现个人转型的三大核心支柱。</p>
				</div>

				<div class="pillars-grid">
					<div class="pillar-card glass glass-hover">
						<div class="pillar-icon-wrapper cyan">
							<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
								<path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"></path>
								<polyline points="3.27 6.96 12 12.01 20.73 6.96"></polyline>
								<line x1="12" y1="22.08" x2="12" y2="12"></line>
							</svg>
						</div>
						<h3>认知增强 (Cognitive Augmentation)</h3>
						<p class="pillar-desc"><strong>认知增强</strong>: 学习将 AI 视作你的“第二大脑”——借助大语言模型 (LLM) 扩展思维边界，加速问题解决与知识内化。</p>
					</div>

					<div class="pillar-card glass glass-hover">
						<div class="pillar-icon-wrapper violet">
							<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
								<polyline points="22 12 18 12 15 21 9 3 6 12 2 12"></polyline>
							</svg>
						</div>
						<h3>智能体工作流 (Agentic Workflows)</h3>
						<p class="pillar-desc"><strong>智能体工作流</strong>: 从单纯的 Prompt 输入转向编排自主运行的循环。利用系统杠权，彻底重构日常工作与生产力流向。</p>
					</div>

					<div class="pillar-card glass glass-hover">
						<div class="pillar-icon-wrapper pink">
							<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
								<path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"></path>
							</svg>
						</div>
						<h3>人本核心韧性 (Human-Core Resilience)</h3>
						<p class="pillar-desc"><strong>人本韧性</strong>: 专注于批判性思考、深层品味 (Taste)、同理心与判断力——这些是 AI 无法取代的终极人类价值瓶颈。</p>
					</div>
				</div>
			</section>

			<!-- Interactive Assessment Section -->
			<section class="assessment-section" id="assessment">
				<div class="section-header">
					<div class="badge">诊断评测工具 | Diagnostic Tool</div>
					<h2 class="section-title">评估你的 AI 时代准备度</h2>
					<p class="section-subtitle">评估你在智能体时代 (Agentic Era) 应对变革与重塑自我的能力。</p>
				</div>
				<InteractiveAssessment />
			</section>

			<!-- Latest Insights Section -->
			<section class="latest-insights">
				<div class="section-header-row">
					<div>
						<div class="badge">最新洞察 | Insights</div>
						<h2 class="section-title">最新文章</h2>
					</div>
					<a href="/blog" class="view-all-link">查看全部文章 &rarr;</a>
				</div>

				<div class="posts-grid">
					{posts.map((post) => (
						<article class="post-card glass glass-hover">
							{post.data.heroImage && (
								<div class="post-img-wrapper">
									<img src={post.data.heroImage.src} alt="" class="post-img" />
									{post.data.video && (
										<div class="video-card-badge">
											<svg viewBox="0 0 24 24" width="12" height="12" fill="currentColor" style="display:inline;vertical-align:middle;margin-right:2px;">
												<path d="M17 10.5V7c0-.55-.45-1-1-1H4c-.55 0-1 .45-1 1v10c0 .55.45 1 1 1h12c.55 0 1-.45 1-1v-3.5l4 4v-11l-4 4z"/>
											</svg>
											<span>播客 | Podcast</span>
										</div>
									)}
								</div>
							)}
							<div class="post-content">
								<span class="post-date"><FormattedDate date={post.data.pubDate} /></span>
								<h3 class="post-title">
									<a href={`/blog/${post.id}/`}>{post.data.title}</a>
								</h3>
								<p class="post-desc">{post.data.description}</p>
								<a href={`/blog/${post.id}/`} class="read-more">阅读全文 &rarr;</a>
							</div>
						</article>
					))}
				</div>
			</section>
		</main>
		<Footer />
	</body>
</html>

<style>
	.hero {
		position: relative;
		padding: 6rem 0 5rem;
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
		overflow: visible;
	}

	.hero-glow {
		position: absolute;
		width: 350px;
		height: 350px;
		border-radius: 50%;
		filter: blur(100px);
		opacity: 0.15;
		z-index: -1;
		pointer-events: none;
	}
	html.light .hero-glow {
		opacity: 0.08;
	}
	.hero-glow-cyan {
		background: var(--accent-cyan);
		top: -50px;
		left: 10%;
	}
	.hero-glow-violet {
		background: var(--accent-violet);
		bottom: 0;
		right: 10%;
	}

	.hero-content {
		max-width: 800px;
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.hero-title {
		font-size: 4rem;
		font-weight: 850;
		line-height: 1.1;
		letter-spacing: -0.04em;
		margin-top: 1.5rem;
		margin-bottom: 1.5rem;
	}

	.hero-lead {
		font-size: 1.35rem;
		color: var(--text-secondary);
		line-height: 1.6;
		margin-bottom: 2.5rem;
		max-width: 650px;
	}

	.hero-actions {
		display: flex;
		gap: 1rem;
	}

	.pillars {
		padding: 5rem 0;
	}

	.section-header {
		text-align: center;
		margin-bottom: 3.5rem;
	}

	.section-title {
		font-size: 2.5rem;
		font-weight: 800;
		letter-spacing: -0.03em;
		margin-top: 0.75rem;
	}

	.section-subtitle {
		font-size: 1.15rem;
		color: var(--text-secondary);
		max-width: 600px;
		margin: 0 auto;
	}

	.pillars-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 1.5rem;
	}

	.pillar-card {
		padding: 2.25rem 2rem;
		border-radius: 20px;
		display: flex;
		flex-direction: column;
	}

	.pillar-icon-wrapper {
		width: 3.25rem;
		height: 3.25rem;
		border-radius: 12px;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-bottom: 1.5rem;
		border: 1px solid rgba(var(--accent-cyan-rgb), 0.2);
	}
	.pillar-icon-wrapper.cyan {
		background: rgba(var(--accent-cyan-rgb), 0.08);
		color: var(--accent-cyan);
		border-color: rgba(var(--accent-cyan-rgb), 0.2);
	}
	.pillar-icon-wrapper.violet {
		background: rgba(var(--accent-violet-rgb), 0.08);
		color: var(--accent-violet);
		border-color: rgba(var(--accent-violet-rgb), 0.2);
	}
	.pillar-icon-wrapper.pink {
		background: rgba(244, 63, 94, 0.08);
		color: #f43f5e;
		border-color: rgba(244, 63, 94, 0.2);
	}

	.pillar-card h3 {
		font-size: 1.25rem;
		margin-bottom: 1rem;
	}

	.pillar-desc {
		font-size: 0.95rem;
		color: var(--text-secondary);
		margin-bottom: 0;
		line-height: 1.6;
	}

	.assessment-section {
		padding: 4rem 0;
	}

	.latest-insights {
		padding: 5rem 0 2rem;
	}

	.section-header-row {
		display: flex;
		justify-content: space-between;
		align-items: flex-end;
		margin-bottom: 3.5rem;
	}

	.view-all-link {
		font-family: var(--font-display);
		font-weight: 600;
		color: var(--accent-cyan);
		font-size: 0.95rem;
		margin-bottom: 0.5rem;
	}

	.posts-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 1.5rem;
	}

	.post-card {
		border-radius: 20px;
		overflow: hidden;
		display: flex;
		flex-direction: column;
		height: 100%;
	}

	.post-img-wrapper {
		width: 100%;
		height: 180px;
		overflow: hidden;
		border-bottom: 1px solid var(--border-color);
		position: relative;
	}

	.video-card-badge {
		position: absolute;
		top: 0.75rem;
		left: 0.75rem;
		display: inline-flex;
		align-items: center;
		gap: 0.25rem;
		background: rgba(var(--accent-primary-rgb), 0.85);
		color: #ffffff;
		padding: 0.25rem 0.625rem;
		border-radius: 9999px;
		font-family: var(--font-display);
		font-weight: 700;
		font-size: 0.7rem;
		backdrop-filter: blur(4px);
		border: 1px solid rgba(255, 255, 255, 0.1);
		z-index: 2;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
	}

	.post-img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		transition: transform 0.5s ease;
	}

	.post-card:hover .post-img {
		transform: scale(1.05);
	}

	.post-content {
		padding: 1.75rem;
		display: flex;
		flex-direction: column;
		flex: 1;
	}

	.post-date {
		font-size: 0.8rem;
		color: var(--text-muted);
		font-weight: 500;
		margin-bottom: 0.75rem;
	}

	.post-title {
		font-size: 1.25rem;
		margin-bottom: 0.75rem;
		line-height: 1.35;
	}

	.post-title a {
		color: var(--text-primary);
	}
	.post-title a:hover {
		color: var(--accent-cyan);
	}

	.post-desc {
		font-size: 0.9rem;
		color: var(--text-secondary);
		line-height: 1.5;
		margin-bottom: 1.5rem;
		flex: 1;
	}

	.read-more {
		font-size: 0.875rem;
		font-weight: 600;
		color: var(--accent-cyan);
	}

	@media (max-width: 1024px) {
		.pillars-grid, .posts-grid {
			grid-template-columns: repeat(2, 1fr);
		}
		.pillars-grid > *:last-child {
			grid-column: span 2;
		}
	}

	@media (max-width: 768px) {
		.hero-title {
			font-size: 2.75rem;
		}
		.hero-lead {
			font-size: 1.15rem;
		}
		.section-header-row {
			flex-direction: column;
			align-items: flex-start;
			gap: 1rem;
		}
		.pillars-grid, .posts-grid {
			grid-template-columns: 1fr;
		}
		.pillars-grid > *:last-child {
			grid-column: span 1;
		}
	}
</style>
```

## 2. `src/pages/blog/index.astro`
```astro
---
import { getCollection } from 'astro:content';
import BaseHead from '../../components/BaseHead.astro';
import Header from '../../components/Header.astro';
import Footer from '../../components/Footer.astro';
import FormattedDate from '../../components/FormattedDate.astro';
import { SITE_TITLE, SITE_DESCRIPTION } from '../../consts';

const posts = (await getCollection('blog')).sort(
	(a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf(),
);

// Collect all unique tags
const allTags = Array.from(
	new Set(posts.flatMap((post) => post.data.tags || []))
).sort();

const tagTranslations: Record<string, string> = {
	workflows: '智能工作流',
	augmentation: '认知增强',
	resilience: '心智重构'
};
---

<!doctype html>
<html lang="en">
	<head>
		<BaseHead title={`文库 | ${SITE_TITLE}`} description={SITE_DESCRIPTION} />
	</head>
	<body>
		<Header />
		<main>
			<!-- Archive Header -->
			<div class="archive-header">
				<span class="badge">洞察归档</span>
				<h1 class="archive-title">重构者文库</h1>
				<p class="archive-subtitle">在人工智能时代，关于提升人类效能、技术框架以及自我重塑的深度探索与随笔。</p>
			</div>

			<!-- Search & Filter Bar -->
			<div class="search-filter-bar glass">
				<div class="search-wrapper">
					<svg class="search-icon" viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
						<circle cx="11" cy="11" r="8"></circle>
						<line x1="21" y1="21" x2="16.65" y2="16.65"></line>
					</svg>
					<input type="text" id="search-input" placeholder="搜索文章..." aria-label="搜索文章" />
				</div>
				<div class="filter-tags">
					<button class="filter-btn active" data-tag="all">全部主题</button>
					{allTags.map(tag => (
						<button class="filter-btn" data-tag={tag}>
							{tagTranslations[tag] || tag.charAt(0).toUpperCase() + tag.slice(1)}
						</button>
					))}
				</div>
			</div>

			<!-- Articles Grid -->
			<section class="posts-section">
				<div class="posts-grid" id="posts-grid">
					{posts.map((post) => (
						<article 
							class="post-card glass glass-hover" 
							data-tags={(post.data.tags || []).join(',')} 
							data-title={post.data.title.toLowerCase()}
							data-desc={post.data.description.toLowerCase()}
						>
							{post.data.heroImage && (
								<div class="post-img-wrapper">
									<img src={post.data.heroImage.src} alt="" class="post-img" />
									{post.data.video && (
										<div class="video-card-badge">
											<svg viewBox="0 0 24 24" width="12" height="12" fill="currentColor" style="display:inline;vertical-align:middle;margin-right:2px;">
												<path d="M17 10.5V7c0-.55-.45-1-1-1H4c-.55 0-1 .45-1 1v10c0 .55.45 1 1 1h12c.55 0 1-.45 1-1v-3.5l4 4v-11l-4 4z"/>
											</svg>
											<span>播客 | Podcast</span>
										</div>
									)}
								</div>
							)}
							<div class="post-content">
								<div class="post-meta">
									<span class="post-date"><FormattedDate date={post.data.pubDate} /></span>
									{post.data.tags && post.data.tags.map(tag => (
										<span class="post-tag-badge">#{tag}</span>
									))}
								</div>
								
								<h3 class="post-title">
									<a href={`/blog/${post.id}/`}>{post.data.title}</a>
								</h3>
								<p class="post-desc">{post.data.description}</p>
								<a href={`/blog/${post.id}/`} class="read-more">阅读全文 &rarr;</a>
							</div>
						</article>
					))}
				</div>

				<!-- Pagination Wrapper -->
				<div class="pagination-wrapper">
					<div class="pagination-container glass" id="pagination-container"></div>
				</div>
				
				<div class="no-results" id="no-results">
					<svg viewBox="0 0 24 24" width="48" height="48" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
						<circle cx="12" cy="12" r="10"></circle>
						<line x1="8" y1="12" x2="16" y2="12"></line>
					</svg>
					<h3>未找到相关文章</h3>
					<p>请尝试换个搜索关键词，或选择其他主题分类。</p>
				</div>
			</section>
		</main>
		<Footer />
	</body>
</html>

<style>
	.archive-header {
		text-align: center;
		padding: 4rem 0 3rem;
	}

	.archive-title {
		font-size: 3.25rem;
		font-weight: 850;
		letter-spacing: -0.04em;
		margin-top: 1rem;
		margin-bottom: 1rem;
	}

	.archive-subtitle {
		font-size: 1.25rem;
		color: var(--text-secondary);
		max-width: 650px;
		margin: 0 auto;
		line-height: 1.6;
	}

	.search-filter-bar {
		border-radius: 24px;
		padding: 1rem;
		display: flex;
		gap: 1.5rem;
		align-items: center;
		margin-bottom: 3rem;
	}

	.search-wrapper {
		position: relative;
		flex: 1;
		display: flex;
		align-items: center;
	}

	.search-icon {
		position: absolute;
		left: 1.25rem;
		color: var(--text-muted);
		pointer-events: none;
	}

	#search-input {
		width: 100%;
		padding-left: 3rem;
		border-radius: 9999px;
		height: 3.25rem;
		font-size: 1rem;
	}

	.filter-tags {
		display: flex;
		gap: 0.5rem;
		flex-wrap: wrap;
	}

	.filter-btn {
		background: transparent;
		border: 1px solid var(--border-color);
		color: var(--text-secondary);
		padding: 0.5rem 1.25rem;
		border-radius: 9999px;
		font-family: var(--font-display);
		font-weight: 600;
		font-size: 0.9rem;
		cursor: pointer;
		transition: all var(--transition-speed) ease;
	}

	.filter-btn:hover {
		color: var(--text-primary);
		border-color: var(--text-secondary);
		background: rgba(var(--accent-cyan-rgb), 0.04);
	}

	.filter-btn.active {
		background: var(--accent-gradient);
		color: #ffffff;
		border-color: transparent;
		box-shadow: 0 4px 12px rgba(var(--accent-cyan-rgb), 0.25);
	}

	.posts-section {
		position: relative;
		min-height: 300px;
	}

	.posts-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 1.5rem;
		transition: opacity 0.3s ease;
	}

	.post-card {
		border-radius: 20px;
		overflow: hidden;
		display: flex;
		flex-direction: column;
		height: 100%;
		transition: opacity var(--transition-speed) ease, transform 0.3s ease;
	}

	.post-card.hidden {
		display: none;
	}

	.post-img-wrapper {
		width: 100%;
		height: 180px;
		overflow: hidden;
		border-bottom: 1px solid var(--border-color);
		position: relative;
	}

	.video-card-badge {
		position: absolute;
		top: 0.75rem;
		left: 0.75rem;
		display: inline-flex;
		align-items: center;
		gap: 0.25rem;
		background: rgba(var(--accent-primary-rgb), 0.85);
		color: #ffffff;
		padding: 0.25rem 0.625rem;
		border-radius: 9999px;
		font-family: var(--font-display);
		font-weight: 700;
		font-size: 0.7rem;
		backdrop-filter: blur(4px);
		border: 1px solid rgba(255, 255, 255, 0.1);
		z-index: 2;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
	}

	.post-img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		transition: transform 0.5s ease;
	}

	.post-card:hover .post-img {
		transform: scale(1.05);
	}

	.post-content {
		padding: 1.75rem;
		display: flex;
		flex-direction: column;
		flex: 1;
	}

	.post-meta {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		margin-bottom: 0.75rem;
		flex-wrap: wrap;
	}

	.post-date {
		font-size: 0.8rem;
		color: var(--text-muted);
		font-weight: 500;
	}

	.post-tag-badge {
		font-size: 0.75rem;
		font-weight: 600;
		color: var(--accent-cyan);
		text-transform: lowercase;
	}

	.post-title {
		font-size: 1.25rem;
		margin-bottom: 0.75rem;
		line-height: 1.35;
	}

	.post-title a {
		color: var(--text-primary);
	}
	.post-title a:hover {
		color: var(--accent-cyan);
	}

	.post-desc {
		font-size: 0.9rem;
		color: var(--text-secondary);
		line-height: 1.5;
		margin-bottom: 1.5rem;
		flex: 1;
	}

	.read-more {
		font-size: 0.875rem;
		font-weight: 600;
		color: var(--accent-cyan);
	}

	.no-results {
		display: none;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		text-align: center;
		padding: 4rem 2rem;
		border-radius: 20px;
		color: var(--text-secondary);
	}

	.no-results svg {
		color: var(--text-muted);
		margin-bottom: 1rem;
	}

	.no-results h3 {
		font-size: 1.5rem;
		margin-bottom: 0.5rem;
	}

	@media (max-width: 1024px) {
		.posts-grid {
			grid-template-columns: repeat(2, 1fr);
		}
		.search-filter-bar {
			flex-direction: column;
			align-items: stretch;
		}
	}

	@media (max-width: 768px) {
		.archive-title {
			font-size: 2.5rem;
		}
		.archive-subtitle {
			font-size: 1.1rem;
		}
		.posts-grid {
			grid-template-columns: 1fr;
		}
	}
</style>

<style is:global>
	.pagination-wrapper {
		display: flex;
		justify-content: center;
		margin-top: 4rem;
	}

	.pagination-container {
		display: inline-flex;
		align-items: center;
		gap: 0.625rem;
		padding: 0.5rem 1rem;
		border-radius: 9999px;
		box-shadow: var(--card-shadow);
	}

	.page-btn {
		-webkit-appearance: none;
		appearance: none;
		min-width: 2.75rem;
		height: 2.75rem;
		border-radius: 12px;
		border: none;
		background: transparent;
		background-color: transparent;
		color: var(--text-secondary);
		font-family: var(--font-display);
		font-weight: 700;
		font-size: 1.05rem;
		cursor: pointer;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 0;
		transition: all var(--transition-speed) ease;
		flex-shrink: 0;
	}

	.page-btn:hover:not(:disabled) {
		color: var(--accent-cyan);
		background: rgba(var(--accent-cyan-rgb), 0.08);
	}

	.page-btn.active {
		background: var(--accent-gradient);
		color: #ffffff;
		box-shadow: 0 4px 14px rgba(var(--accent-cyan-rgb), 0.4);
	}
	html.light .page-btn.active {
		box-shadow: 0 4px 14px rgba(var(--accent-cyan-rgb), 0.2);
	}

	.page-btn:disabled {
		opacity: 0.25;
		cursor: not-allowed;
	}

	.page-btn svg {
		transition: transform 0.2s ease;
	}

	.page-btn:hover:not(:disabled) svg.prev-arrow {
		transform: translateX(-2px);
	}

	.page-btn:hover:not(:disabled) svg.next-arrow {
		transform: translateX(2px);
	}
</style>

<script>
	const setupFiltersAndPagination = () => {
		const searchInput = document.getElementById('search-input') as HTMLInputElement | null;
		const filterButtons = document.querySelectorAll('.filter-btn');
		const postCards = document.querySelectorAll('.post-card');
		const noResults = document.getElementById('no-results');
		const paginationContainer = document.getElementById('pagination-container');

		const postsPerPage = 6;
		let currentPage = 1;
		let activeTag = 'all';
		let searchQuery = '';

		const filterPosts = () => {
			let visibleCount = 0;
			const matchingCards: HTMLElement[] = [];
			
			postCards.forEach(card => {
				const htmlCard = card as HTMLElement;
				const title = htmlCard.dataset.title || '';
				const desc = htmlCard.dataset.desc || '';
				const tags = htmlCard.dataset.tags ? htmlCard.dataset.tags.split(',') : [];

				const matchesSearch = title.includes(searchQuery) || desc.includes(searchQuery);
				const matchesTag = activeTag === 'all' || tags.includes(activeTag);

				if (matchesSearch && matchesTag) {
					matchingCards.push(htmlCard);
				} else {
					htmlCard.classList.add('hidden');
				}
			});

			const totalPages = Math.ceil(matchingCards.length / postsPerPage);
			
			if (currentPage > totalPages) {
				currentPage = Math.max(1, totalPages);
			}

			matchingCards.forEach((card, index) => {
				const startIndex = (currentPage - 1) * postsPerPage;
				const endIndex = startIndex + postsPerPage;
				
				if (index >= startIndex && index < endIndex) {
					card.classList.remove('hidden');
					visibleCount++;
				} else {
					card.classList.add('hidden');
				}
			});

			if (noResults) {
				noResults.style.display = visibleCount === 0 ? 'flex' : 'none';
			}

			renderPagination(totalPages);
		};

		const renderPagination = (totalPages: number) => {
			if (!paginationContainer) return;
			paginationContainer.innerHTML = '';

			if (totalPages <= 1) {
				paginationContainer.style.display = 'none';
				return;
			}

			paginationContainer.style.display = 'flex';

			const prevBtn = document.createElement('button');
			prevBtn.className = 'page-btn';
			prevBtn.setAttribute('aria-label', '上一页');
			prevBtn.innerHTML = `
				<svg class="prev-arrow" viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
					<polyline points="15 18 9 12 15 6"></polyline>
				</svg>
			`;
			prevBtn.disabled = currentPage === 1;
			prevBtn.addEventListener('click', () => {
				currentPage--;
				filterPosts();
				window.scrollTo({ top: 300, behavior: 'smooth' });
			});
			paginationContainer.appendChild(prevBtn);

			for (let i = 1; i <= totalPages; i++) {
				const btn = document.createElement('button');
				btn.className = `page-btn ${currentPage === i ? 'active' : ''}`;
				btn.textContent = i.toString();
				btn.setAttribute('aria-label', `第 ${i} 页`);
				btn.addEventListener('click', () => {
					currentPage = i;
					filterPosts();
					window.scrollTo({ top: 300, behavior: 'smooth' });
				});
				paginationContainer.appendChild(btn);
			}

			const nextBtn = document.createElement('button');
			nextBtn.className = 'page-btn';
			nextBtn.setAttribute('aria-label', '下一页');
			nextBtn.innerHTML = `
				<svg class="next-arrow" viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
					<polyline points="9 18 15 12 9 6"></polyline>
				</svg>
			`;
			nextBtn.disabled = currentPage === totalPages;
			nextBtn.addEventListener('click', () => {
				currentPage++;
				filterPosts();
				window.scrollTo({ top: 300, behavior: 'smooth' });
			});
			paginationContainer.appendChild(nextBtn);
		};

		searchInput?.addEventListener('input', (e) => {
			searchQuery = (e.target as HTMLInputElement).value.toLowerCase().trim();
			currentPage = 1;
			filterPosts();
		});

		filterButtons.forEach(btn => {
			btn.addEventListener('click', () => {
				filterButtons.forEach(b => b.classList.remove('active'));
				btn.classList.add('active');
				activeTag = (btn as HTMLElement).dataset.tag || 'all';
				currentPage = 1;
				filterPosts();
			});
		});

		filterPosts();
	};

	setupFiltersAndPagination();
	document.addEventListener('astro:after-swap', setupFiltersAndPagination);
</script>
```

## 3. `src/pages/blog/[...slug].astro`
```astro
---
import { type CollectionEntry, getCollection, render } from 'astro:content';
import BlogPost from '../../layouts/BlogPost.astro';

export async function getStaticPaths() {
	const posts = await getCollection('blog');
	return posts.map((post) => ({
		params: { slug: post.id },
		props: post,
	}));
}
type Props = CollectionEntry<'blog'>;

const post = Astro.props;
const { Content, headings } = await render(post);
---

<BlogPost {...post.data} headings={headings}>
	<Content />
</BlogPost>
```

## 4. `src/pages/about.astro`
```astro
---
import BaseHead from '../components/BaseHead.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import { SITE_TITLE, SITE_DESCRIPTION } from '../consts';
---

<!doctype html>
<html lang="en">
	<head>
		<BaseHead title={`关于我 | ${SITE_TITLE}`} description={SITE_DESCRIPTION} />
	</head>
	<body>
		<Header />
		<main>
			<!-- About Header -->
			<section class="about-header">
				<span class="badge">重构哲学</span>
				<h1 class="about-title">自我重构宣言</h1>
				<p class="about-subtitle">关于本站的初心、作者的探索，以及我们如何在机器速度与人类审美之间寻找完美的交汇点。</p>
			</section>

			<!-- Intro Section -->
			<section class="intro-grid">
				<div class="intro-content">
					<h2>核心前提</h2>
					<p>我们正处于人类历史上最剧烈的技术转型期。随着人工智能从静态模型演变为自主运行的智能体（Agent），常规性的脑力劳动价值正在迅速归零。</p>
					<p>本博客 <strong>Rebuild.AI</strong> 是一座鲜活的实验室。它记录了我的实践与思考，并提供了一套系统性框架，帮助我们从传统建设者转型为<strong>智能编排者（AI Orchestrator）</strong>——即通过指挥智能系统，而非徒手进行低效执行的人。</p>
					<p>我相信，未来既不单单属于 AI，也不属于未被扩容的纯粹人类。它属于那些掌握“人机共生艺术”的人：利用机器的速度扩展个人能力的上限，同时加倍强化人类独有的判断力、审美力与同理心。</p>
				</div>
				
				<div class="stats-panel glass">
					<h3>能力扩容画像</h3>
					<div class="stats-grid">
						<div class="stat-card">
							<span class="stat-num">85%</span>
							<span class="stat-label">AI 辅助编码</span>
						</div>
						<div class="stat-card">
							<span class="stat-num">5x</span>
							<span class="stat-label">学习速率倍增</span>
						</div>
						<div class="stat-card">
							<span class="stat-num">0</span>
							<span class="stat-label">自动化焦虑感</span>
						</div>
						<div class="stat-card">
							<span class="stat-num">100%</span>
							<span class="stat-label">人类独特审美</span>
						</div>
					</div>
					<div class="stats-footer">
						<p>由 Claude、GPT-4 和本地定制智能体提供认知扩容。</p>
					</div>
				</div>
			</section>

			<!-- Timeline Section -->
			<section class="timeline-section">
				<div class="section-title-wrapper">
					<div class="badge">重构之路</div>
					<h2>工作流演进</h2>
					<p>我个人工作流如何从“手动低级执行”蜕变到“智能体协同编排”。</p>
				</div>

				<div class="timeline">
					<div class="timeline-item">
						<div class="timeline-marker"></div>
						<div class="timeline-content glass glass-hover">
							<span class="timeline-date">2023</span>
							<h3>认知觉醒</h3>
							<p>GPT-4 的发布击碎了我对技能长寿的幻想。我意识到编写纯语法代码和起草标准化文案已不再是核心竞争优势。我开始把重心转移到 Prompt 工程以及核心认知模型的研究中。</p>
						</div>
					</div>

					<div class="timeline-item">
						<div class="timeline-marker"></div>
						<div class="timeline-content glass glass-hover">
							<span class="timeline-date">2024</span>
							<h3>认知扩容</h3>
							<p>我将自己所有的工作流切换为“双脑协作”模式。不再逐行手写代码，而是学习去编写精确的技术 Spec，与模型建立反馈循环，在架构层级进行审阅与决策。这让我的产出效率提升了 300%。</p>
						</div>
					</div>

					<div class="timeline-item">
						<div class="timeline-marker"></div>
						<div class="timeline-content glass glass-hover">
							<span class="timeline-date">2025</span>
							<h3>智能体编排</h3>
							<p>工作流从简单的 Chat 对话演化为自主 Agent 循环（MCP 服务、多智能体团队协作），自动执行深度调研、数据清洗以及自动化测试。我自己的角色彻底转向系统设计与价值过滤。</p>
						</div>
					</div>

					<div class="timeline-item">
						<div class="timeline-marker"></div>
						<div class="timeline-content glass glass-hover">
							<span class="timeline-date">2026</span>
							<h3>重建与分享</h3>
							<p>创立 Rebuild.AI，分享用于自我升级、超级学习和保持心智韧性且经过实战打磨的系统框架。重点聚焦于深化人类品味、驾驭智能 Agent 团队，以及开发高附加值的卓越产品。</p>
						</div>
					</div>
				</div>
			</section>

			<!-- Core Beliefs Callout -->
			<section class="beliefs-section glass">
				<h2 class="beliefs-title">核心底层原则</h2>
				<div class="beliefs-grid">
					<div class="belief-card">
						<h4>01 / 方向重于速度</h4>
						<p>当执行速度趋于无穷且边际成本归零时，唯一的瓶颈变成了做决策：去往“哪里”，以及创造“什么”才是有价值的。</p>
					</div>
					<div class="belief-card">
						<h4>02 / AI 只是放大器</h4>
						<p>AI 不会直接替代你，它只是你的乘数。如果你自身的专业能力或批判性思维是 0，0 乘以任何数依然是 0。请先升级你的认知内核。</p>
					</div>
					<div class="belief-card">
						<h4>03 / 深化人类品味</h4>
						<p>品味是人类最顶级的过滤器，是识别美感、品质和市场契合度的综合直觉。这才是我们最终的护城河。</p>
					</div>
				</div>
			</section>
		</main>
		<Footer />
	</body>
</html>

<style>
	.about-header {
		text-align: center;
		padding: 4rem 0 3rem;
	}

	.about-title {
		font-size: 3.25rem;
		font-weight: 850;
		letter-spacing: -0.04em;
		margin-top: 1rem;
		margin-bottom: 1rem;
	}

	.about-subtitle {
		font-size: 1.25rem;
		color: var(--text-secondary);
		max-width: 650px;
		margin: 0 auto;
		line-height: 1.6;
	}

	.intro-grid {
		display: grid;
		grid-template-columns: 1.2fr 0.8fr;
		gap: 3rem;
		margin-bottom: 5rem;
		align-items: start;
	}

	.intro-content h2 {
		font-size: 1.75rem;
		margin-bottom: 1.25rem;
	}

	.intro-content p {
		font-size: 1.05rem;
		line-height: 1.75;
		color: var(--text-secondary);
		margin-bottom: 1.5rem;
	}

	.stats-panel {
		padding: 2rem;
		border-radius: 24px;
	}

	.stats-panel h3 {
		font-size: 1.25rem;
		margin-bottom: 1.5rem;
		border-bottom: 1px solid var(--border-color);
		padding-bottom: 0.5rem;
	}

	.stats-grid {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 1.25rem;
		margin-bottom: 1.5rem;
	}

	.stat-card {
		display: flex;
		flex-direction: column;
	}

	.stat-num {
		font-size: 2rem;
		font-weight: 800;
		color: var(--accent-cyan);
		font-family: var(--font-display);
	}

	.stat-label {
		font-size: 0.8rem;
		color: var(--text-secondary);
		font-weight: 500;
	}

	.stats-footer {
		font-size: 0.8rem;
		color: var(--text-muted);
		border-top: 1px solid var(--border-color);
		padding-top: 1rem;
		text-align: center;
	}

	.timeline-section {
		padding: 3rem 0 5rem;
	}

	.section-title-wrapper {
		text-align: center;
		margin-bottom: 3.5rem;
	}

	.section-title-wrapper h2 {
		font-size: 2.25rem;
		margin-top: 0.5rem;
	}

	.timeline {
		position: relative;
		max-width: 800px;
		margin: 0 auto;
		padding-left: 2rem;
	}

	.timeline::before {
		content: '';
		position: absolute;
		left: 7px;
		top: 0.5rem;
		bottom: 0;
		width: 2px;
		background: var(--border-color);
	}

	.timeline-item {
		position: relative;
		margin-bottom: 3rem;
	}

	.timeline-item:last-child {
		margin-bottom: 0;
	}

	.timeline-marker {
		position: absolute;
		left: -2rem;
		top: 1.25rem;
		width: 16px;
		height: 16px;
		border-radius: 50%;
		background: var(--bg-primary);
		border: 3px solid var(--accent-cyan);
		z-index: 1;
		transition: all 0.3s ease;
	}

	.timeline-item:hover .timeline-marker {
		background: var(--accent-cyan);
		box-shadow: 0 0 10px rgba(var(--accent-cyan-rgb), 0.6);
	}

	.timeline-content {
		padding: 1.75rem 2rem;
		border-radius: 20px;
	}

	.timeline-date {
		font-family: var(--font-display);
		font-size: 0.85rem;
		font-weight: 700;
		color: var(--accent-cyan);
		text-transform: uppercase;
		letter-spacing: 0.05em;
		display: block;
		margin-bottom: 0.5rem;
	}

	.timeline-content h3 {
		font-size: 1.35rem;
		margin-bottom: 0.75rem;
	}

	.timeline-content p {
		font-size: 0.95rem;
		color: var(--text-secondary);
		margin-bottom: 0;
		line-height: 1.6;
	}

	.beliefs-section {
		padding: 3rem;
		border-radius: 28px;
		margin: 3rem 0;
	}

	.beliefs-title {
		font-size: 1.75rem;
		margin-bottom: 2rem;
		text-align: center;
	}

	.beliefs-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 2rem;
	}

	.belief-card h4 {
		font-size: 1.1rem;
		font-weight: 700;
		margin-bottom: 0.75rem;
		color: var(--accent-cyan);
	}

	.belief-card p {
		font-size: 0.925rem;
		line-height: 1.6;
		margin-bottom: 0;
	}

	@media (max-width: 868px) {
		.intro-grid {
			grid-template-columns: 1fr;
			gap: 2rem;
		}
		.beliefs-grid {
			grid-template-columns: 1fr;
			gap: 1.5rem;
		}
		.beliefs-section {
			padding: 2rem;
		}
	}
</style>
```
