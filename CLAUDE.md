# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Fuwari is a static blog template built with **Astro 5 + Tailwind CSS + Svelte**. It is a personal blog theme with rich features: light/dark mode, search (Pagefind), math rendering (KaTeX), table of contents, RSS, responsive design, and smooth page transitions (swup).

## Key Commands

All commands are run from the project root with `pnpm`:

| Command | Action |
|---|---|
| `pnpm install` | Install dependencies (requires pnpm >= 9) |
| `pnpm dev` | Start local dev server at `localhost:4321` |
| `pnpm build` | Build production site to `./dist/`, then run Pagefind search indexing |
| `pnpm preview` | Preview the production build locally |
| `pnpm check` | Run `astro check` (TypeScript type checking + lint) |
| `pnpm type-check` | Run `tsc --noEmit --isolatedDeclarations` for interface-only checks |
| `pnpm format` | Format `./src` with Biome |
| `pnpm lint` | Check and fix `./src` with Biome |
| `pnpm new-post <filename>` | Create a new blog post in `src/content/posts/` |
| `pnpm astro ...` | Run Astro CLI commands (add, check, etc.) |

## Architecture & Structure

```
fuwari/
├── astro.config.mjs          # Astro config: integrations (Tailwind, Svelte, swup, ExpressiveCode, sitemap), Markdown plugins (remark/rehype pipeline)
├── tailwind.config.cjs       # Tailwind CSS configuration
├── biome.json                # Biome lint/format rules
├── tsconfig.json             # TypeScript: strict mode, path aliases (@components/*, @utils/*, @layouts/*, etc.)
├── pagefind.yml              # Pagefind search indexing config
│
├── src/
│   ├── config.ts             # ** Main blog config: site metadata, nav bar, profile, sidebar cards, license, banner, theme hue
│   ├── constants/
│   │   └── constants.ts      # Global constants (LIGHT_MODE, DARK_MODE, AUTO_MODE, etc.)
│   ├── types/
│   │   ├── config.ts         # Type definitions for all config structures (SiteConfig, NavBarConfig, ProfileConfig, BlogPostData, etc.)
│   │   └── ...               # Other type definitions
│   ├── plugins/              # Custom Astro/remark/rehype plugins
│   │   ├── remark-excerpt.js
│   │   ├── remark-reading-time.mjs
│   │   ├── rehype-component-admonition.mjs
│   │   ├── rehype-component-github-card.mjs
│   │   ├── remark-directive-rehype.js
│   │   └── expressive-code/  # Expressive Code plugins (language badge, custom copy button)
│   ├── layouts/              # Astro layout components (Default, Post, etc.)
│   ├── components/           # Svelte components
│   │   ├── widget/           # Sidebar widgets, header, etc.
│   │   ├── control/          # UI controls (theme toggle, scrollbar, etc.)
│   │   └── misc/             # Miscellaneous shared components
│   ├── pages/                # Astro page routes
│   │   ├── posts/            # Dynamic post pages [...slug]
│   │   └── *.astro           # Other pages (index, about, archive, tags)
│   ├── content/
│   │   ├── posts/            # Blog posts (.md/.mdx files with YAML frontmatter)
│   │   └── spec/             # Content collections spec
│   ├── i18n/                 # Internationalization
│   │   └── languages/        # Translation files per locale
│   ├── styles/               # Global CSS / Tailwind layers
│   ├── assets/               # Static images, icons
│   └── utils/                # Utility functions
│
├── public/                   # Public static assets (favicons, robots.txt, etc.)
├── docs/                     # Localized README files
├── scripts/
│   └── new-post.js           # CLI helper to scaffold new blog posts
```

## Development Guidelines

### Configuration
- All blog customization is done through **`src/config.ts`** — site title, language, theme color hue, banner image, nav links, profile, sidebar cards, license, and TOC settings.
- To change the site URL, edit `site` in `astro.config.mjs`.
- The `trailingSlash: "always"` setting is used for consistent URL patterns.

### Adding Posts
- Run `pnpm new-post <filename>` to scaffold a new post.
- Posts live in `src/content/posts/` as Markdown files with YAML frontmatter:
  ```yaml
  ---
  title: Post Title
  published: 2024-01-01
  description: Summary
  image: ./cover.jpg
  tags: [Tag1, Tag2]
  category: Category
  draft: false
  lang: jp  # (optional) override post language
  ---
  ```

### Markdown Extensions
- **Admonitions**: `note`, `tip`, `important`, `caution`, `warning` blocks (rendered via `rehype-component-admonition`)
- **GitHub Cards**: `github` directive for repo cards
- **Expressive Code**: enhanced code blocks with line numbers, collapsible sections, syntax highlighting (theme from `expressiveCodeConfig.theme` in config)
- **KaTeX**: math expressions via `remark-math` + `rehype-katex`
- **Auto-linked headings**: headings get anchor links

### Path Aliases (from tsconfig.json)
- `@components/*` → `src/components/*`
- `@utils/*` → `src/utils/*`
- `@layouts/*` → `src/layouts/*`
- `@i18n/*` → `src/i18n/*`
- `@constants/*` → `src/constants/*`
- `@assets/*` → `src/assets/*`
- `@/*` → `src/*`

### Build Output
- Production build goes to `./dist/`
- Search index is generated by `pagefind` as the final step of `pnpm build`

### Key Dependencies
- **Astro 5** — static site framework
- **Tailwind CSS 3** — utility-first CSS
- **Svelte 5** — reactive components
- **swup** — smooth page transitions (AJAX navigation)
- **Expressive Code** — enhanced syntax highlighting
- **Pagefind** — client-side full-text search
- **KaTeX** — math rendering
- **Biome** — linting and formatting
- **sharp** — image optimization (build-time)
