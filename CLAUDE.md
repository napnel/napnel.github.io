# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Astro-based static blog site deployed to GitHub Pages at https://napnel.github.io. It's built using the Astro Blog template with support for Markdown/MDX content, RSS feeds, and sitemap generation.

## Development Commands

```bash
# Install dependencies
npm install

# Start development server (runs on localhost:4321)
npm run dev

# Build for production (output to ./dist/)
npm run build

# Preview production build locally
npm run preview

# Run Astro CLI commands
npm run astro ...
```

## Architecture

### Content Collections

The blog uses Astro's Content Collections API (src/content.config.ts:4-19) for type-safe content management:

- Blog posts live in `src/content/blog/` as Markdown or MDX files
- Schema defined with Zod validation requiring: `title`, `description`, `pubDate`, and optional `updatedDate` and `heroImage`
- Access posts via `getCollection('blog')` in pages
- Content loader uses glob pattern: `**/*.{md,mdx}`

### Routing

- File-based routing in `src/pages/`
- Dynamic blog post routes handled by `src/pages/blog/[...slug].astro`
- `getStaticPaths()` generates routes from content collection at build time
- Each post route uses `render()` to transform content and passes to `BlogPost` layout

### Site Configuration

- Site URL configured in `astro.config.mjs:9` as `https://napnel.github.io`
- Global constants (site title, description) in `src/consts.ts`
- Integrations: MDX, Sitemap
- TypeScript with strict null checks enabled

### Deployment

GitHub Actions workflow (`.github/workflows/deploy.yml`) automatically:
1. Builds on push to `develop` branch or manual trigger
2. Uses Node.js 20
3. Runs `npm ci` and `npm run build`
4. Deploys `./dist/` to GitHub Pages

## Branch Strategy

- **develop**: Default branch. All development work happens here, auto-deploys on push
- **feature/***: Feature branches. Merge into develop when ready
- **main**: Not used (legacy)

## Project Structure

```
src/
├── assets/          # Static assets processed by Astro
├── components/      # Reusable Astro components (Header, Footer, etc.)
├── content/
│   └── blog/        # Blog post Markdown/MDX files
├── layouts/         # Page layouts (BlogPost.astro)
├── pages/           # File-based routes
│   ├── blog/
│   │   ├── [...slug].astro  # Dynamic blog post pages
│   │   └── index.astro      # Blog listing
│   ├── about.astro
│   ├── index.astro
│   └── rss.xml.js           # RSS feed generation
└── styles/          # Global styles
```

## Adding New Blog Posts

Create a new `.md` or `.mdx` file in `src/content/blog/` with required frontmatter:

```md
---
title: "Post Title"
description: "Post description"
pubDate: 2026-01-01
updatedDate: 2026-01-02  # optional
heroImage: ./image.png    # optional, relative to src/assets/
---

Content here...
```

The schema validation ensures type safety and will error if required fields are missing.
