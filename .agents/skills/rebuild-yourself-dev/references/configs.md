# Configuration Blueprints for Rebuild.AI

Below are the exact configuration files required to bootstrap and run the Rebuild.AI Astro blog.

## 1. `package.json`
```json
{
  "name": "rebuild-yourself",
  "type": "module",
  "version": "0.0.1",
  "engines": {
    "node": ">=22.12.0"
  },
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview",
    "astro": "astro"
  },
  "dependencies": {
    "@astrojs/mdx": "^6.0.3",
    "@astrojs/rss": "^4.0.18",
    "@astrojs/sitemap": "^3.7.3",
    "astro": "^6.4.5",
    "sharp": "^0.34.3"
  }
}
```

## 2. `tsconfig.json`
```json
{
  "extends": "astro/tsconfigs/strict",
  "include": [".astro/types.d.ts", "**/*"],
  "exclude": ["dist"],
  "compilerOptions": {
    "strictNullChecks": true
  }
}
```

## 3. `astro.config.mjs`
```javascript
// @ts-check
import mdx from '@astrojs/mdx';
import sitemap from '@astrojs/sitemap';
import { defineConfig, fontProviders } from 'astro/config';
import { unified } from '@astrojs/markdown-remark';

// Custom Rehype plugin to open all external links in a new tab
function rehypeExternalLinks() {
	return (tree) => {
		function traverse(node) {
			if (node.type === 'element' && node.tagName === 'a') {
				const href = node.properties?.href;
				if (href && (href.startsWith('http://') || href.startsWith('https://'))) {
					node.properties.target = '_blank';
					node.properties.rel = ['noopener', 'noreferrer'];
				}
			}
			if (node.children) {
				node.children.forEach(traverse);
			}
		}
		traverse(tree);
	};
}

// https://astro.build/config
export default defineConfig({
	site: 'https://example.com',
	integrations: [mdx(), sitemap()],
	markdown: {
		processor: unified({
			rehypePlugins: [rehypeExternalLinks],
		}),
		shikiConfig: {
			themes: {
				light: 'github-light',
				dark: 'github-dark',
			},
		},
	},
	fonts: [
		{
			provider: fontProviders.local(),
			name: 'Atkinson',
			cssVariable: '--font-atkinson',
			fallbacks: ['sans-serif'],
			options: {
				variants: [
					{
						src: ['./src/assets/fonts/atkinson-regular.woff'],
						weight: 400,
						style: 'normal',
						display: 'swap',
					},
					{
						src: ['./src/assets/fonts/atkinson-bold.woff'],
						weight: 700,
						style: 'normal',
						display: 'swap',
					},
				],
			},
		},
	],
});
```

## 4. `src/content.config.ts`
```typescript
import { defineCollection } from 'astro:content';
import { glob } from 'astro/loaders';
import { z } from 'astro/zod';

const blog = defineCollection({
	// Load Markdown and MDX files in the `src/content/blog/` directory.
	loader: glob({ base: './src/content/blog', pattern: '**/*.{md,mdx}' }),
	// Type-check frontmatter using a schema
	schema: ({ image }) =>
		z.object({
			title: z.string(),
			description: z.string(),
			// Transform string to Date object
			pubDate: z.coerce.date(),
			updatedDate: z.coerce.date().optional(),
			heroImage: z.optional(image()),
			tags: z.array(z.string()).optional(),
			video: z.string().optional(),
			videoType: z.enum(['local', 'youtube', 'bilibili', 'google-drive']).optional(),
		}),
});

export const collections = { blog };
```

## 5. `src/consts.ts`
```typescript
// Place any global data in this file.
// You can import this data from anywhere in your site by using the `import` keyword.

export const SITE_TITLE = 'Rebuild.AI';
export const SITE_DESCRIPTION = '如何在 AI 时代重塑自我 - 探索人类智能与人工智能的最佳协同路径。';
```
