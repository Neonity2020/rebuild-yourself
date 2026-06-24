# Layouts and Components Blueprints for Rebuild.AI

Below are the complete Astro templates for layouts and components.

## 1. `src/layouts/BlogPost.astro`
```astro
---
import type { CollectionEntry } from 'astro:content';
import BaseHead from '../components/BaseHead.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import FormattedDate from '../components/FormattedDate.astro';

type Props = CollectionEntry<'blog'>['data'] & {
	headings?: Array<{ depth: number; slug: string; text: string }>;
};

const { title, description, pubDate, updatedDate, heroImage, tags, headings = [], video, videoType = 'local' } = Astro.props;

// Filter headings for the Table of Contents (show h2 and h3)
const tocHeadings = headings.filter((h) => h.depth === 2 || h.depth === 3);
---

<html lang="en">
	<head>
		<BaseHead title={title} description={description} />
		<!-- KaTeX for LaTeX formula rendering -->
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css">
		<script is:inline src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js" defer></script>
		<script is:inline src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js" defer onload="window.dispatchEvent(new Event('katex-loaded'))"></script>
	</head>

	<body>
		<Header />
		
		<!-- Reading Progress Bar -->
		<div class="reading-progress-bar" id="reading-progress"></div>

		<main class="post-main">
			<article class="post-article">
				<!-- Header Section -->
				<header class="post-header">
					<a href="/blog" class="back-link">
						<svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
							<line x1="19" y1="12" x2="5" y2="12"></line>
							<polyline points="12 19 5 12 12 5"></polyline>
						</svg>
						Back to Library
					</a>
					<div class="post-meta">
						<span class="post-date"><FormattedDate date={pubDate} /></span>
						{tags && tags.map(tag => (
							<span class="tag-badge">#{tag}</span>
						))}
					</div>
					<h1 class="post-title">{title}</h1>
					<p class="post-lead">{description}</p>
					{
						updatedDate && (
							<div class="last-updated-on">
								Last updated on <FormattedDate date={updatedDate} />
							</div>
						)
					}
				</header>

				<!-- Hero Image or Video Podcast -->
				{video ? (
					<div class="video-player-container glass">
						<div class="video-badge">
							<svg viewBox="0 0 24 24" width="16" height="16" fill="currentColor" style="display:inline;vertical-align:middle;margin-right:4px;">
								<path d="M17 10.5V7c0-.55-.45-1-1-1H4c-.55 0-1 .45-1 1v10c0 .55.45 1 1 1h12c.55 0 1-.45 1-1v-3.5l4 4v-11l-4 4z"/>
							</svg>
							<span>视频播客 | Video Podcast</span>
						</div>
						{videoType === 'local' ? (
							<video 
								src={video} 
								controls 
								playsinline 
								poster={heroImage ? heroImage.src : undefined}
								class="video-element"
							></video>
						) : (
							<div class="iframe-wrapper">
								<iframe 
									src={video} 
									scrolling="no" 
									border="0" 
									frameborder="no" 
									framespacing="0" 
									allowfullscreen="true"
									allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
									class="video-iframe"
								></iframe>
							</div>
						)}
					</div>
				) : (
					heroImage && (
						<div class="hero-image-wrapper glass">
							<img src={heroImage.src} alt="" class="hero-image" />
						</div>
					)
				)}

				<!-- Post Body & Table of Contents Container -->
				<div class="post-layout-container">
					<div class="prose" id="post-content">
						<slot />
					</div>

					{tocHeadings.length > 0 && (
						<aside class="toc-sidebar">
							<div class="toc-card glass">
								<h4 class="toc-title">On This Page</h4>
								<nav class="toc-nav">
									<ul>
										{tocHeadings.map(h => (
											<li class={`depth-${h.depth}`}>
												<a href={`#${h.slug}`} class="toc-link" data-slug={h.slug}>
													{h.text}
												</a>
											</li>
										))}
									</ul>
								</nav>
							</div>
						</aside>
					)}
				</div>
			</article>
		</main>
		<Footer />
	</body>
</html>

<style>
	/* Reading Progress */
	.reading-progress-bar {
		position: fixed;
		top: 0;
		left: 0;
		height: 3px;
		width: 0%;
		background: var(--accent-gradient);
		z-index: 999;
		transition: width 0.1s ease-out;
		box-shadow: 0 0 8px rgba(var(--accent-cyan-rgb), 0.5);
	}

	.post-main {
		width: 1200px;
		max-width: calc(100% - 2.5rem);
		margin: 0 auto;
		padding: 3rem 1.25rem;
	}

	.post-article {
		max-width: 960px;
		margin: 0 auto;
	}

	/* Post Header */
	.post-header {
		margin-bottom: 2.5rem;
	}

	.back-link {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		font-family: var(--font-display);
		font-weight: 600;
		font-size: 0.9rem;
		color: var(--text-secondary);
		margin-bottom: 1.5rem;
		transition: color var(--transition-speed) ease, transform var(--transition-speed) ease;
	}

	.back-link:hover {
		color: var(--accent-cyan);
		transform: translateX(-4px);
	}

	.post-meta {
		display: flex;
		align-items: center;
		gap: 1rem;
		margin-bottom: 1rem;
	}

	.post-date {
		font-size: 0.85rem;
		font-weight: 500;
		color: var(--text-muted);
	}

	.tag-badge {
		font-size: 0.8rem;
		font-weight: 600;
		color: var(--accent-cyan);
		text-transform: lowercase;
	}

	.post-title {
		font-size: 3.5rem;
		font-weight: 850;
		line-height: 1.15;
		letter-spacing: -0.04em;
		margin-bottom: 1rem;
	}

	.post-lead {
		font-size: 1.25rem;
		color: var(--text-secondary);
		line-height: 1.6;
		margin-bottom: 0;
	}

	.last-updated-on {
		font-size: 0.85rem;
		color: var(--text-muted);
		font-style: italic;
		margin-top: 1rem;
	}

	/* Hero Image */
	.hero-image-wrapper {
		border-radius: 24px;
		overflow: hidden;
		margin-bottom: 3.5rem;
		aspect-ratio: 2 / 1;
		width: 100%;
	}

	.hero-image {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	/* Video Player Container */
	.video-player-container {
		border-radius: 24px;
		overflow: hidden;
		margin-bottom: 3.5rem;
		width: 100%;
		position: relative;
		border: 1px solid var(--border-color);
		box-shadow: var(--card-shadow);
		background: var(--glass-bg);
	}

	.video-badge {
		position: absolute;
		top: 1rem;
		left: 1rem;
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		background: rgba(var(--accent-primary-rgb), 0.85);
		color: #ffffff;
		padding: 0.5rem 1rem;
		border-radius: 9999px;
		font-family: var(--font-display);
		font-weight: 700;
		font-size: 0.8rem;
		z-index: 10;
		backdrop-filter: blur(8px);
		border: 1px solid rgba(255, 255, 255, 0.1);
		box-shadow: 0 4px 12px rgba(var(--accent-primary-rgb), 0.3);
		text-transform: uppercase;
		letter-spacing: 0.05em;
	}

	.video-element {
		width: 100%;
		aspect-ratio: 16 / 9;
		display: block;
		object-fit: contain;
		background: #000;
	}

	.iframe-wrapper {
		width: 100%;
		position: relative;
		padding-bottom: 56.25%; /* 16:9 Aspect Ratio */
		height: 0;
	}

	.video-iframe {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		border: 0;
		background: #000;
	}

	/* Layout Container */
	.post-layout-container {
		display: flex;
		gap: 3.5rem;
		position: relative;
		align-items: flex-start;
	}

	.prose {
		flex: 1;
		max-width: 680px;
	}

	/* TOC Sidebar */
	.toc-sidebar {
		width: 250px;
		flex-shrink: 0;
		position: sticky;
		top: 6rem;
	}

	.toc-card {
		padding: 1.5rem;
		border-radius: 20px;
	}

	.toc-title {
		font-size: 0.95rem;
		font-weight: 700;
		text-transform: uppercase;
		letter-spacing: 0.05em;
		color: var(--text-primary);
		margin-bottom: 1rem;
		border-bottom: 1px solid var(--border-color);
		padding-bottom: 0.5rem;
	}

	.toc-nav ul {
		list-style: none;
		padding: 0;
		margin: 0;
	}

	.toc-nav li {
		margin-bottom: 0.625rem;
	}

	.toc-nav li.depth-3 {
		padding-left: 1rem;
	}

	.toc-link {
		font-size: 0.875rem;
		color: var(--text-secondary);
		transition: all 0.2s ease;
		display: inline-block;
		line-height: 1.4;
	}

	.toc-link:hover {
		color: var(--accent-cyan);
		transform: translateX(2px);
	}

	.toc-link.active {
		color: var(--accent-cyan);
		font-weight: 600;
		border-left: 2px solid var(--accent-cyan);
		padding-left: 6px;
		margin-left: -8px;
	}

	@media (max-width: 992px) {
		.toc-sidebar {
			display: none;
		}
		.post-layout-container {
			gap: 0;
		}
		.prose {
			max-width: 100%;
		}
	}

	@media (max-width: 768px) {
		.post-title {
			font-size: 2.5rem;
		}
		.hero-image-wrapper {
			margin-bottom: 2rem;
			border-radius: 16px;
		}
	}
</style>

<script>
	const setupPostInteractions = () => {
		// 1. Reading Progress Bar
		const progressBar = document.getElementById('reading-progress');
		
		const updateProgress = () => {
			if (!progressBar) return;
			const totalHeight = document.documentElement.scrollHeight - window.innerHeight;
			if (totalHeight > 0) {
				const percentage = (window.scrollY / totalHeight) * 100;
				progressBar.style.width = `${percentage}%`;
			}
		};

		window.addEventListener('scroll', updateProgress);
		updateProgress(); // Run once initially

		// 2. Scroll-Spy Table of Contents Active Highlighting
		const tocLinks = document.querySelectorAll('.toc-link');
		const articleContent = document.getElementById('post-content');
		
		if (!articleContent || tocLinks.length === 0) return;

		// Extract headings
		const headings = Array.from(articleContent.querySelectorAll('h2, h3'));
		
		const observerOptions = {
			root: null,
			rootMargin: '-10% 0px -70% 0px',
			threshold: 0
		};

		const observer = new IntersectionObserver((entries) => {
			entries.forEach(entry => {
				if (entry.isIntersecting) {
					const id = entry.target.getAttribute('id');
					tocLinks.forEach(link => {
						const anchorLink = link as HTMLAnchorElement;
						if (anchorLink.getAttribute('href') === `#${id}`) {
							link.classList.add('active');
						} else {
							link.classList.remove('active');
						}
					});
				}
			});
		}, observerOptions);

		headings.forEach(heading => observer.observe(heading));

		// 3. LaTeX Math Rendering
		const renderLaTeX = () => {
			const contentElement = document.getElementById('post-content');
			if (contentElement && typeof renderMathInElement === 'function') {
				renderMathInElement(contentElement, {
					delimiters: [
						{left: '$$', right: '$$', display: true},
						{left: '$', right: '$', display: false},
						{left: '\\(', right: '\\)', display: false},
						{left: '\\[', right: '\\]', display: true}
					],
					throwOnError : false
				});
			}
		};

		if (typeof renderMathInElement === 'function') {
			renderLaTeX();
		} else {
			window.addEventListener('katex-loaded', renderLaTeX, { once: true });
		}
	};

	// Initialize
	setupPostInteractions();
	document.addEventListener('astro:after-swap', setupPostInteractions);
</script>
```

## 2. `src/components/BaseHead.astro`
```astro
---
import '../styles/global.css';
import type { ImageMetadata } from 'astro';
import FallbackImage from '../assets/blog-placeholder-1.jpg';
import { SITE_TITLE } from '../consts';
import { Font } from 'astro:assets';

interface Props {
	title: string;
	description: string;
	image?: ImageMetadata;
}

const canonicalURL = new URL(Astro.url.pathname, Astro.site);
const { title, description, image = FallbackImage } = Astro.props;
---

<!-- Global Metadata -->
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
<link rel="icon" href="/favicon.ico" />

<script is:inline>
	const theme = (() => {
		if (typeof localStorage !== 'undefined' && localStorage.getItem('theme')) {
			return localStorage.getItem('theme');
		}
		if (window.matchMedia('(prefers-color-scheme: light)').matches) {
			return 'light';
		}
		return 'dark';
	})();
	
	if (theme === 'light') {
		document.documentElement.classList.add('light');
	} else {
		document.documentElement.classList.remove('light');
	}
</script>
<link rel="sitemap" href="/sitemap-index.xml" />
<link
	rel="alternate"
	type="application/rss+xml"
	title={SITE_TITLE}
	href={new URL('rss.xml', Astro.site)}
/>
<meta name="generator" content={Astro.generator} />

<Font cssVariable="--font-atkinson" preload />

<!-- Canonical URL -->
<link rel="canonical" href={canonicalURL} />

<!-- Primary Meta Tags -->
<title>{title}</title>
<meta name="title" content={title} />
<meta name="description" content={description} />

<!-- Open Graph / Facebook -->
<meta property="og:type" content="website" />
<meta property="og:url" content={Astro.url} />
<meta property="og:title" content={title} />
<meta property="og:description" content={description} />
<meta property="og:image" content={new URL(image.src, Astro.url)} />

<!-- Twitter -->
<meta property="twitter:card" content="summary_large_image" />
<meta property="twitter:url" content={Astro.url} />
<meta property="twitter:title" content={title} />
<meta property="twitter:description" content={description} />
<meta property="twitter:image" content={new URL(image.src, Astro.url)} />
```

## 3. `src/components/Header.astro`
```astro
---
import { SITE_TITLE } from '../consts';
import HeaderLink from './HeaderLink.astro';
---

<header class="glass">
	<nav class="nav-container">
		<a href="/" class="brand">
			<svg class="logo-icon" viewBox="0 0 24 24" width="28" height="28" fill="none" xmlns="http://www.w3.org/2000/svg">
				<path d="M12 2L2 7L12 12L22 7L12 2Z" stroke="url(#logo-grad)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
				<path d="M2 17L12 22L22 17" stroke="url(#logo-grad)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
				<path d="M2 12L12 17L22 12" stroke="url(#logo-grad)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
				<defs>
					<linearGradient id="logo-grad" x1="2" y1="2" x2="22" y2="22" gradientUnits="userSpaceOnUse">
						<stop stop-color="var(--accent-cyan)" />
						<stop offset="1" stop-color="var(--accent-violet)" />
					</linearGradient>
				</defs>
			</svg>
			<span class="brand-text gradient-text">{SITE_TITLE}</span>
		</a>

		<div class="nav-menu" id="nav-menu">
			<HeaderLink href="/">Home</HeaderLink>
			<HeaderLink href="/blog">Blog</HeaderLink>
			<HeaderLink href="/about">About</HeaderLink>
			<a href="/blog" class="btn btn-primary btn-mobile-only">Start Reading</a>
		</div>

		<div class="actions">
			<button id="theme-toggle" class="action-btn theme-toggle-btn" aria-label="Toggle theme">
				<svg class="sun-icon" viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
					<circle cx="12" cy="12" r="5"></circle>
					<line x1="12" y1="1" x2="12" y2="3"></line>
					<line x1="12" y1="21" x2="12" y2="23"></line>
					<line x1="4.22" y1="4.22" x2="5.64" y2="5.64"></line>
					<line x1="18.36" y1="18.36" x2="19.78" y2="19.78"></line>
					<line x1="1" y1="12" x2="3" y2="12"></line>
					<line x1="21" y1="12" x2="23" y2="12"></line>
					<line x1="4.22" y1="19.78" x2="5.64" y2="18.36"></line>
					<line x1="18.36" y1="5.64" x2="19.78" y2="4.22"></line>
				</svg>
				<svg class="moon-icon" viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
					<path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"></path>
				</svg>
			</button>

			<a href="/blog" class="btn btn-primary btn-nav-desktop">Get Started</a>

			<button id="menu-toggle" class="action-btn menu-toggle-btn" aria-label="Toggle menu">
				<svg class="hamburger" viewBox="0 0 24 24" width="22" height="22" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
					<line class="line line-top" x1="4" y1="6" x2="20" y2="6"></line>
					<line class="line line-mid" x1="4" y1="12" x2="20" y2="12"></line>
					<line class="line line-bot" x1="4" y1="18" x2="20" y2="18"></line>
				</svg>
			</button>
		</div>
	</nav>
</header>

<style>
	header {
		position: sticky;
		top: 1.25rem;
		z-index: 100;
		max-width: 1100px;
		margin: 1.25rem auto 0;
		width: calc(100% - 2.5rem);
		border-radius: 9999px;
		padding: 0.5rem 1.5rem;
		transition: background-color var(--transition-speed) ease, border-color var(--transition-speed) ease, box-shadow var(--transition-speed) ease;
	}
	.nav-container {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	.brand {
		display: flex;
		align-items: center;
		gap: 0.625rem;
		text-decoration: none;
	}
	.brand-text {
		font-family: var(--font-display);
		font-weight: 800;
		font-size: 1.35rem;
		letter-spacing: -0.03em;
	}
	.logo-icon {
		filter: drop-shadow(0 0 8px rgba(var(--accent-cyan-rgb), 0.3));
		transition: filter 0.3s ease;
	}
	.brand:hover .logo-icon {
		filter: drop-shadow(0 0 12px rgba(var(--accent-violet-rgb), 0.5));
	}
	.nav-menu {
		display: flex;
		align-items: center;
		gap: 0.5rem;
	}
	.actions {
		display: flex;
		align-items: center;
		gap: 0.75rem;
	}
	.action-btn {
		background: transparent;
		border: 1px solid var(--border-color);
		color: var(--text-primary);
		width: 2.5rem;
		height: 2.5rem;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		transition: all var(--transition-speed) ease;
	}
	.action-btn:hover {
		background: rgba(var(--accent-cyan-rgb), 0.08);
		border-color: var(--accent-cyan);
		color: var(--accent-cyan);
	}
	.btn-nav-desktop {
		padding: 0.5rem 1.25rem;
		font-size: 0.875rem;
	}
	.btn-mobile-only {
		display: none;
	}
	.menu-toggle-btn {
		display: none;
	}

	:global(html:not(.light)) .moon-icon {
		display: none;
	}
	:global(html.light) .sun-icon {
		display: none;
	}

	.hamburger .line {
		transition: transform 0.3s ease, opacity 0.3s ease;
		transform-origin: center;
	}
	.action-btn.active .line-top {
		transform: translateY(6px) rotate(45deg);
	}
	.action-btn.active .line-mid {
		opacity: 0;
	}
	.action-btn.active .line-bot {
		transform: translateY(-6px) rotate(-45deg);
	}

	@media (max-width: 768px) {
		header {
			top: 1rem;
			width: calc(100% - 2rem);
			border-radius: 24px;
			padding: 0.75rem 1.25rem;
		}
		.nav-menu {
			position: absolute;
			top: calc(100% + 0.75rem);
			left: 0;
			right: 0;
			background: var(--glass-bg);
			backdrop-filter: blur(20px);
			-webkit-backdrop-filter: blur(20px);
			border: 1px solid var(--glass-border);
			border-radius: 20px;
			flex-direction: column;
			padding: 1.5rem;
			gap: 0.75rem;
			display: none;
			box-shadow: var(--card-shadow);
			align-items: stretch;
		}
		.nav-menu.open {
			display: flex;
			animation: slideDown 0.3s cubic-bezier(0.16, 1, 0.3, 1);
		}
		.btn-nav-desktop {
			display: none;
		}
		.btn-mobile-only {
			display: inline-flex;
			width: 100%;
			margin-top: 0.5rem;
		}
		.menu-toggle-btn {
			display: flex;
		}
	}

	@keyframes slideDown {
		from {
			opacity: 0;
			transform: translateY(-10px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}
</style>

<script>
	const setupThemeToggle = () => {
		const toggleBtn = document.getElementById('theme-toggle');
		toggleBtn?.addEventListener('click', () => {
			const isLight = document.documentElement.classList.contains('light');
			if (isLight) {
				document.documentElement.classList.remove('light');
				localStorage.setItem('theme', 'dark');
			} else {
				document.documentElement.classList.add('light');
				localStorage.setItem('theme', 'light');
			}
		});
	};

	const setupMobileMenu = () => {
		const menuToggle = document.getElementById('menu-toggle');
		const navMenu = document.getElementById('nav-menu');
		
		menuToggle?.addEventListener('click', () => {
			const isOpen = navMenu?.classList.toggle('open');
			menuToggle?.classList.toggle('active', isOpen);
		});

		const navLinks = navMenu?.querySelectorAll('a');
		navLinks?.forEach(link => {
			link.addEventListener('click', () => {
				navMenu?.classList.remove('open');
				menuToggle?.classList.remove('active');
			});
		});
	};

	setupThemeToggle();
	setupMobileMenu();
	document.addEventListener('astro:after-swap', () => {
		setupThemeToggle();
		setupMobileMenu();
	});
</script>
```

## 4. `src/components/Footer.astro`
```astro
---
const today = new Date();
---

<footer class="glass">
	<div class="footer-container">
		<div class="footer-brand">
			<a href="/" class="brand-link">
				<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
					<path d="M12 2L2 7L12 12L22 7L12 2Z"></path>
					<path d="M2 17L12 22L22 17"></path>
					<path d="M2 12L12 17L22 12"></path>
				</svg>
				<span class="brand-name">Rebuild.AI</span>
			</a>
			<p class="brand-tagline">如何在 AI 时代重塑自我：探索人类智能与人工智能的最佳协同路径。</p>
		</div>

		<div class="footer-links-group">
			<h4>Navigation</h4>
			<ul>
				<li><a href="/">Home</a></li>
				<li><a href="/blog">Blog Library</a></li>
				<li><a href="/about">Manifesto</a></li>
			</ul>
		</div>

		<div class="footer-links-group">
			<h4>Connect</h4>
			<div class="social-links">
				<a href="https://github.com" target="_blank" aria-label="GitHub">
					<svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
						<path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"></path>
					</svg>
				</a>
				<a href="https://twitter.com" target="_blank" aria-label="Twitter">
					<svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
						<path d="M23 3a10.9 10.9 0 0 1-3.14 1.53 4.48 4.48 0 0 0-7.86 3v1A10.66 10.66 0 0 1 3 4s-4 9 5 13a11.64 11.64 0 0 1-7 2c9 5 20 0 20-11.5a4.5 4.5 0 0 0-.08-.83A7.72 7.72 0 0 0 23 3z"></path>
					</svg>
				</a>
				<a href="/rss.xml" aria-label="RSS Feed">
					<svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
						<path d="M4 11a9 9 0 0 1 9 9"></path>
						<path d="M4 4a16 16 0 0 1 16 16"></path>
						<circle cx="5" cy="19" r="1"></circle>
					</svg>
				</a>
			</div>
		</div>
	</div>
	<div class="footer-bottom">
		<p>&copy; {today.getFullYear()} Rebuild.AI. All rights reserved.</p>
	</div>
</footer>

<style>
	footer {
		margin-top: 5rem;
		border-radius: 32px 32px 0 0;
		border-bottom: none;
		border-left: none;
		border-right: none;
		padding: 4rem 2rem 2.5rem;
	}

	.footer-container {
		max-width: 1100px;
		margin: 0 auto;
		display: grid;
		grid-template-columns: 2fr 1fr 1fr;
		gap: 4rem;
		margin-bottom: 3rem;
	}

	.footer-brand {
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.brand-link {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		text-decoration: none;
		color: var(--text-primary);
		font-family: var(--font-display);
		font-weight: 800;
		font-size: 1.25rem;
	}

	.brand-tagline {
		font-size: 0.9rem;
		color: var(--text-secondary);
		line-height: 1.6;
		max-width: 320px;
	}

	.footer-links-group h4 {
		font-size: 0.95rem;
		font-weight: 700;
		text-transform: uppercase;
		letter-spacing: 0.05em;
		margin-bottom: 1.25rem;
		color: var(--text-primary);
	}

	.footer-links-group ul {
		list-style: none;
		padding: 0;
		margin: 0;
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}

	.footer-links-group a {
		color: var(--text-secondary);
		font-size: 0.9rem;
		transition: color var(--transition-speed) ease;
	}

	.footer-links-group a:hover {
		color: var(--accent-cyan);
	}

	.social-links {
		display: flex;
		gap: 1rem;
	}

	.social-links a {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 2.25rem;
		height: 2.25rem;
		border-radius: 50%;
		border: 1px solid var(--border-color);
		color: var(--text-secondary);
		transition: all var(--transition-speed) ease;
	}

	.social-links a:hover {
		color: var(--accent-cyan);
		border-color: var(--accent-cyan);
		background: rgba(var(--accent-cyan-rgb), 0.08);
		transform: translateY(-2px);
	}

	.footer-bottom {
		max-width: 1100px;
		margin: 0 auto;
		border-top: 1px solid var(--border-color);
		padding-top: 1.5rem;
		text-align: center;
		font-size: 0.8rem;
		color: var(--text-muted);
	}

	@media (max-width: 768px) {
		.footer-container {
			grid-template-columns: 1fr;
			gap: 2rem;
		}
		footer {
			padding: 3rem 1.5rem 2rem;
		}
	}
</style>
```

## 5. `src/components/FormattedDate.astro`
```astro
---
interface Props {
	date: Date;
}

const { date } = Astro.props;
---

<time datetime={date.toISOString()}>
	{
		date.toLocaleDateString('zh-cn', {
			year: 'numeric',
			month: 'long',
			day: 'numeric',
		})
	}
</time>
```

## 6. `src/components/HeaderLink.astro`
```astro
---
import type { HTMLAttributes } from 'astro/types';

type Props = HTMLAttributes<'a'>;

const { href, class: className, ...props } = Astro.props;
const pathname = Astro.url.pathname.replace(import.meta.env.BASE_URL, '');
const subpath = pathname.match(/[^\/]+/g);
const isActive = href === pathname || href === '/' + (subpath?.[0] || '');
---

<a href={href} class:list={[className, { active: isActive }]} {...props}>
	<slot />
</a>
<style>
	a {
		display: inline-block;
		text-decoration: none;
		color: var(--text-secondary);
		font-family: var(--font-display);
		font-weight: 500;
		font-size: 0.95rem;
		padding: 0.5rem 1rem;
		border-radius: 9999px;
		transition: all var(--transition-speed) ease;
	}
	a:hover {
		color: var(--text-primary);
		background: rgba(var(--accent-cyan-rgb), 0.08);
	}
	a.active {
		color: var(--accent-cyan);
		font-weight: 600;
		background: rgba(var(--accent-cyan-rgb), 0.1);
	}
</style>
```

## 7. `src/components/InteractiveAssessment.astro`
```astro
---
---

<div class="assessment-card glass" id="assessment-container">
	<!-- Start View -->
	<div class="view active" id="start-view">
		<div class="badge">自我评测 | Assessment</div>
		<h2 class="view-title">你准备好迎接 AI 时代了吗？</h2>
		<p class="view-desc">花 1 分钟完成这篇自我诊断评测，获取你的 <strong>AI 时代复原力指数 (AI Era Resilience Index)</strong>，并获得量身定制的自我重塑行动建议。</p>
		<button class="btn btn-primary" id="start-btn">开始评测</button>
	</div>

	<!-- Quiz View -->
	<div class="view" id="quiz-view">
		<div class="progress-container">
			<div class="progress-bar" id="progress-bar"></div>
		</div>
		<div class="question-header">
			<span class="question-number" id="question-number">第 1 题 / 共 4 题</span>
		</div>
		<h3 class="question-text" id="question-text">问题加载中...</h3>
		
		<div class="options-grid" id="options-container">
			<!-- Options dynamically populated by JS -->
		</div>
	</div>

	<!-- Results View -->
	<div class="view" id="results-view">
		<div class="badge">评测报告 | Report</div>
		<h2 class="view-title">复原力画像 (Resilience Profile)</h2>
		
		<div class="result-score-section">
			<div class="chart-container">
				<svg class="radial-svg" viewBox="0 0 120 120">
					<circle class="bg-circle" cx="60" cy="60" r="50"></circle>
					<circle class="fg-circle" cx="60" cy="60" r="50" id="result-chart-circle"></circle>
				</svg>
				<div class="score-display">
					<span class="score-val" id="score-val">0</span>
					<span class="score-pct">%</span>
				</div>
			</div>
			
			<div class="profile-info">
				<h3 class="profile-title" id="profile-title">AI Orchestrator</h3>
				<p class="profile-desc" id="profile-desc">你具备极强的适应力，能够广泛利用 AI 工具增强自己的智能。</p>
			</div>
		</div>
		
		<div class="recommendations-box">
			<h4>量身定制的重塑建议 (Recommendations)：</h4>
			<ul id="recommendations-list">
				<!-- Populated dynamically -->
			</ul>
		</div>

		<div class="results-actions">
			<button class="btn btn-secondary" id="restart-btn">重新测试</button>
			<a href="/blog" class="btn btn-primary" id="read-rec-btn">阅读推荐文章</a>
		</div>
	</div>
</div>

<style is:global>
	.assessment-card {
		padding: 2.5rem;
		border-radius: 24px;
		margin: 3rem 0;
		position: relative;
		overflow: hidden;
		min-height: 350px;
		display: flex;
		flex-direction: column;
		justify-content: center;
	}

	.view {
		display: none;
		flex-direction: column;
		align-items: center;
		text-align: center;
		animation: fadeIn 0.4s ease-out forwards;
	}

	.view.active {
		display: flex;
	}

	.view-title {
		margin-top: 1rem;
		font-size: 2.25rem;
		font-weight: 800;
		letter-spacing: -0.03em;
	}

	.view-desc {
		font-size: 1.1rem;
		max-width: 600px;
		margin-bottom: 2rem;
	}

	#quiz-view {
		text-align: left;
		align-items: stretch;
	}

	.progress-container {
		width: 100%;
		height: 6px;
		background: var(--bg-tertiary);
		border-radius: 9999px;
		margin-bottom: 1.5rem;
		overflow: hidden;
	}

	.progress-bar {
		height: 100%;
		width: 0%;
		background: var(--accent-gradient);
		border-radius: 9999px;
		transition: width 0.3s ease;
	}

	.question-header {
		display: flex;
		justify-content: space-between;
		font-size: 0.875rem;
		font-weight: 600;
		color: var(--accent-cyan);
		text-transform: uppercase;
		letter-spacing: 0.05em;
		margin-bottom: 0.5rem;
	}

	.question-text {
		font-size: 1.5rem;
		font-weight: 700;
		margin-bottom: 2rem;
		line-height: 1.35;
	}

	.options-grid {
		display: grid;
		grid-template-columns: 1fr;
		gap: 1rem;
	}

	.option-btn {
		background: var(--bg-secondary);
		border: 1px solid var(--border-color);
		color: var(--text-primary);
		padding: 1.25rem 1.5rem;
		border-radius: 16px;
		text-align: left;
		cursor: pointer;
		font-family: var(--font-sans);
		font-size: 1.05rem;
		transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
		display: flex;
		align-items: center;
		gap: 1rem;
	}

	.option-btn:hover {
		background: rgba(var(--accent-cyan-rgb), 0.04);
		border-color: rgba(var(--accent-cyan-rgb), 0.4);
		transform: translateX(4px);
	}

	.option-indicator {
		width: 24px;
		height: 24px;
		border-radius: 50%;
		border: 2px solid var(--border-color);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 0.8rem;
		font-weight: 700;
		flex-shrink: 0;
		transition: all 0.2s ease;
	}

	.option-btn:hover .option-indicator {
		border-color: var(--accent-cyan);
		background: rgba(var(--accent-cyan-rgb), 0.1);
	}

	.result-score-section {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 3rem;
		margin: 2rem 0;
		width: 100%;
		max-width: 700px;
	}

	.chart-container {
		position: relative;
		width: 120px;
		height: 120px;
		flex-shrink: 0;
	}

	.radial-svg {
		transform: rotate(-90deg);
		width: 100%;
		height: 100%;
	}

	.bg-circle, .fg-circle {
		fill: none;
		stroke-width: 10px;
	}

	.bg-circle {
		stroke: var(--bg-tertiary);
	}

	.fg-circle {
		stroke: var(--accent-cyan);
		stroke-linecap: round;
		stroke-dasharray: 314;
		stroke-dashoffset: 314;
		transition: stroke-dashoffset 1s cubic-bezier(0.4, 0, 0.2, 1);
	}

	.score-display {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		display: flex;
		align-items: baseline;
		font-family: var(--font-display);
	}

	.score-val {
		font-size: 2.25rem;
		font-weight: 800;
		color: var(--text-primary);
	}

	.score-pct {
		font-size: 1rem;
		font-weight: 600;
		color: var(--text-secondary);
	}

	.profile-info {
		text-align: left;
		flex: 1;
	}

	.profile-title {
		font-size: 1.75rem;
		font-weight: 800;
		margin-bottom: 0.5rem;
		background: var(--accent-gradient);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.profile-desc {
		font-size: 1.05rem;
		color: var(--text-secondary);
		margin-bottom: 0;
	}

	.recommendations-box {
		width: 100%;
		background: rgba(255, 255, 255, 0.02);
		border: 1px solid var(--border-color);
		border-radius: 16px;
		padding: 1.5rem;
		margin-bottom: 2rem;
		text-align: left;
	}

	.recommendations-box h4 {
		font-size: 1.1rem;
		margin-bottom: 0.75rem;
		font-weight: 700;
	}

	.recommendations-box ul {
		margin-left: 1.25rem;
		color: var(--text-secondary);
	}

	.recommendations-box li {
		margin-bottom: 0.5rem;
	}

	.results-actions {
		display: flex;
		gap: 1rem;
	}

	@media (max-width: 640px) {
		.assessment-card {
			padding: 1.5rem;
		}
		.result-score-section {
			flex-direction: column;
			gap: 1.5rem;
			text-align: center;
		}
		.profile-info {
			text-align: center;
		}
		.results-actions {
			flex-direction: column;
			width: 100%;
			gap: 0.75rem;
		}
		.results-actions .btn {
			width: 100%;
		}
	}

	@keyframes fadeIn {
		from {
			opacity: 0;
			transform: translateY(8px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.rec-link {
		color: var(--accent-cyan);
		text-decoration: none;
		font-weight: 600;
		border-bottom: 1px dashed rgba(var(--accent-cyan-rgb), 0.4);
		transition: all 0.2s ease;
	}

	.rec-link:hover {
		color: var(--text-primary);
		border-bottom-color: var(--accent-cyan);
		background: rgba(var(--accent-cyan-rgb), 0.08);
		border-radius: 4px;
		padding: 0 4px;
		margin: 0 -4px;
	}
</style>

<script>
	const questions = [
		{
			question: "你在工作或学习中集成生成式 AI 工具（如 ChatGPT、Claude 或 Copilot 等）的频率如何？",
			options: [
				{ text: "深度融入，每日使用——它们几乎增强了我所有的日常工作与学习项目。", score: 25 },
				{ text: "偶尔使用——我只用它们来做头脑风暴或文章总结等特定任务。", score: 15 },
				{ text: "极少或从不使用——我更倾向于传统的搜索引擎或纯手动工作流。", score: 5 }
			]
		},
		{
			question: "面对复杂问题时，你通常如何与 AI 协同工作？",
			options: [
				{ text: "作为思维伙伴——构建复杂的 Prompt 工作流，并批判性地评估其输出。", score: 25 },
				{ text: "快速查询答案——直接复制粘贴结果，很少进行二次审阅或深度修改。", score: 10 },
				{ text: "自主解决——尽量完全靠自己弄清楚，避免引入 AI 干扰思路。", score: 5 }
			]
		},
		{
			question: "在学习跨领域的全新技术/技能时，你的掌握速度如何？",
			options: [
				{ text: "极快——我利用 AI 快速建立学习路线图，解答疑难概念，并进行交互式探索。", score: 25 },
				{ text: "中等——我通过观看在线视频课程、阅读官方文档，并手写一些测试代码。", score: 15 },
				{ text: "较慢——我主要依赖传统培训或实体书籍，通常需要耗费数月时间才能上手。", score: 5 }
			]
		},
		{
			question: "对于 AI 自动化可能取代当前岗位的趋势，你的心态是怎样的？",
			options: [
				{ text: "视作机遇——这是摆脱机械化事务、升级个人策略与 Taste 的绝佳契机。", score: 25 },
				{ text: "焦虑但积极——虽然有危机感，但我正在主动学习相关技术进行自我重塑。", score: 15 },
				{ text: "焦虑且排斥——我认为这是一种威胁，尽量避免去接触和了解它。", score: 5 }
			]
		}
	];

	const profiles = [
		{
			minScore: 80,
			title: "AI Orchestrator (AI 协同架构师)",
			desc: "你处于 AI 适应度的第一梯队。你将 AI 视为认知放大器，在保持高度批判性思考的同时成倍提升效率。你已经具备了在 AI 时代蓬勃发展并发挥领导力的坐标。",
			recommendations: [
				"推荐阅读深度博客：<a href='/blog/systems-of-intelligence/' class='rec-link'>《Designing Systems of Intelligence: 超越聊天框，构建系统级智能》</a>",
				"推荐阅读核心原理：<a href='/blog/agent-harness-context/' class='rec-link'>《Agent = LLM + Harness：为什么上下文管理是智能体的灵魂？》</a>",
				"专注于构建个人专属的工作流，并尝试编排自主运行的智能体（Agents）。",
				"持续磨练你的深度品味 (Taste) 与直觉判断力——这是人类最后的价值壁垒。"
			]
		},
		{
			minScore: 50,
			title: "Resilient Adapter (敏捷适应者)",
			desc: "你正在积极地探索和适应。你利用 AI 提高了工作速度，但可能仍然处于较为被动的提问状态，或者缺乏系统性的工作流编排。优化你的 Prompt 架构将为你带来巨大的生产力红利。",
			recommendations: [
				"推荐阅读：<a href='/blog/learning-velocity/' class='rec-link'>《Accelerating Your Learning Velocity: 用 AI 教练开启超速学习时代》</a>",
				"挑战自己：利用 AI 在几天内用一门你完全不懂的编程语言独立完成一个 Side Project。",
				"建立日常惯例，主动测试 and 引入新的生产力工具。"
			]
		},
		{
			minScore: 0,
			title: "Traditional Humanist (传统探索者)",
			desc: "你目前仍在使用传统的方法开展工作。虽然你对人类核心价值的专注非常宝贵，但你也面临在执行速度和处理复杂任务能力上被拉开差距的风险。建议从在日常例程中融入微小的 AI 协同习惯开始。",
			recommendations: [
				"推荐阅读：<a href='/blog/beginners-manifesto/' class='rec-link'>《A Beginner's Manifesto: AI 时代个人重塑宣言》</a>",
				"从小事做起：每天花 10 分钟使用 AI 辅助润色邮件、翻译文档或提取长文摘要。",
				"关注降低 AI 引入门槛的基础指南，逐步构建起自己的基础工具箱。"
			]
		}
	];

	let currentQuestionIdx = 0;
	let userScore = 0;

	const startView = document.getElementById('start-view');
	const quizView = document.getElementById('quiz-view');
	const resultsView = document.getElementById('results-view');
	
	const startBtn = document.getElementById('start-btn');
	const restartBtn = document.getElementById('restart-btn');
	
	const progressBar = document.getElementById('progress-bar');
	const questionNumber = document.getElementById('question-number');
	const questionText = document.getElementById('question-text');
	const optionsContainer = document.getElementById('options-container');

	const scoreVal = document.getElementById('score-val');
	const resultCircle = document.getElementById('result-chart-circle') as SVGCircleElement | null;
	const profileTitle = document.getElementById('profile-title');
	const profileDesc = document.getElementById('profile-desc');
	const recsList = document.getElementById('recommendations-list');

	const showView = (viewId: string) => {
		[startView, quizView, resultsView].forEach(view => {
			if (view) view.style.display = 'none';
		});
		const targetView = document.getElementById(viewId);
		if (targetView) {
			targetView.style.display = 'flex';
		}
	};

	const startQuiz = () => {
		currentQuestionIdx = 0;
		userScore = 0;
		showView('quiz-view');
		renderQuestion();
	};

	const selectOption = (score: number) => {
		userScore += score;
		currentQuestionIdx++;
		if (currentQuestionIdx < questions.length) {
			renderQuestion();
		} else {
			showResults();
		}
	};

	const renderQuestion = () => {
		const q = questions[currentQuestionIdx];
		if (!q) return;

		const pct = ((currentQuestionIdx) / questions.length) * 100;
		if (progressBar) progressBar.style.width = `${pct}%`;
		if (questionNumber) questionNumber.textContent = `第 ${currentQuestionIdx + 1} 题 / 共 ${questions.length} 题`;
		if (questionText) questionText.textContent = q.question;

		if (optionsContainer) {
			optionsContainer.innerHTML = '';
			q.options.forEach((opt, idx) => {
				const char = String.fromCharCode(65 + idx);
				const btn = document.createElement('button');
				btn.className = 'option-btn';
				btn.innerHTML = `
					<div class="option-indicator">${char}</div>
					<span>${opt.text}</span>
				`;
				btn.addEventListener('click', () => selectOption(opt.score));
				optionsContainer.appendChild(btn);
			});
		}
	};

	const showResults = () => {
		showView('results-view');
		
		let currentVal = 0;
		const duration = 1000;
		const step = userScore / (duration / 16);
		const interval = setInterval(() => {
			currentVal += step;
			if (currentVal >= userScore) {
				currentVal = userScore;
				clearInterval(interval);
			}
			if (scoreVal) scoreVal.textContent = Math.round(currentVal).toString();
		}, 16);

		const maxOffset = 314;
		const targetOffset = maxOffset - (maxOffset * (userScore / 100));
		setTimeout(() => {
			if (resultCircle) {
				resultCircle.style.strokeDashoffset = targetOffset.toString();
			}
		}, 100);

		const profile = profiles.find(p => userScore >= p.minScore) || profiles[profiles.length - 1];
		if (profileTitle) profileTitle.textContent = profile.title;
		if (profileDesc) profileDesc.textContent = profile.desc;

		if (recsList) {
			recsList.innerHTML = '';
			profile.recommendations.forEach(rec => {
				const li = document.createElement('li');
				li.innerHTML = rec;
				recsList.appendChild(li);
			});
		}

		const readRecBtn = document.getElementById('read-rec-btn') as HTMLAnchorElement | null;
		if (readRecBtn) {
			if (userScore >= 80) {
				readRecBtn.href = '/blog/systems-of-intelligence/';
			} else if (userScore >= 50) {
				readRecBtn.href = '/blog/learning-velocity/';
			} else {
				readRecBtn.href = '/blog/beginners-manifesto/';
			}
		}
	};

	const bindEvents = () => {
		startBtn?.addEventListener('click', startQuiz);
		restartBtn?.addEventListener('click', startQuiz);
	};

	bindEvents();
	document.addEventListener('astro:after-swap', () => {
		bindEvents();
	});
</script>
```
