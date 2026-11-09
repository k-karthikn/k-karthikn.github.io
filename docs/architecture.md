# Architecture Documentation

This document describes the architectural foundation and structural design of the personal developer website and devlog hosted at [`https://k-karthikn.github.io/`](https://k-karthikn.github.io/).

---

## 1. Architectural Philosophy

1. **Static-First Performance**: Every public route is pre-rendered at build time into pure HTML, CSS, client-side JavaScript, and static JSON search indexes. No backend servers, databases, or runtime Node.js containers are required in production.
2. **Single-Application Simplicity**: A single Astro project housed in one GitHub repository deployed to the domain root. Section paths (e.g. `/blogs/`, `/projects/`) are standard Astro routes rather than multi-zone deployments or separate sub-applications.
3. **Canonical Route Ownership**: The blog lives canonically at `/blogs/` and `/blogs/<slug>/`. Legacy `/posts/` routes from upstream templates have been eliminated to ensure consistent SEO, breadcrumbs, search indexing, and RSS feeds.
4. **Separation of Concerns**: Routes, markdown content, layouts, components, utilities, styling tokens, and site configuration each have dedicated responsibilities without unnecessary abstraction layers.

---

## 2. Public Route Architecture

| Route | Source File | Type | Description |
| :--- | :--- | :--- | :--- |
| `/` | `src/pages/index.astro` | Static Page | Personal homepage, developer bio, social links, and latest blog posts. |
| `/about/` | `src/pages/about.astro` | Content Page | Author background, technical focus areas, and contact links (rendered from `src/content/pages/about.md`). |
| `/blogs/` | `src/pages/blogs/[...page].astro` | Dynamic Paginated | Canonical blog directory with pagination. |
| `/blogs/<slug>/` | `src/pages/blogs/[...slug]/index.astro` | Dynamic Route | Individual article reader with TOC, share links, and adjacent navigation. |
| `/projects/` | `src/pages/projects/index.astro` | Static Page | Portfolio and case studies showcase. |
| `/archives/` | `src/pages/archives/index.astro` | Static Page | Chronological article archive grouped by year and month. |
| `/tags/` | `src/pages/tags/index.astro` | Static Page | Tag cloud listing all published topics. |
| `/tags/<tag>/` | `src/pages/tags/[tag]/[...page].astro` | Dynamic Paginated | Articles filtered by tag with pagination. |
| `/search/` | `src/pages/search.astro` | Static UI | Client-side static full-text search powered by Pagefind. |
| `/rss.xml` | `src/pages/rss.xml.ts` | Endpoint | Validated RSS 2.0 feed linking directly to canonical `/blogs/<slug>/` URLs. |
| `/robots.txt` | `src/pages/robots.txt.ts` | Endpoint | Search engine crawl directives referencing `sitemap-index.xml`. |
| `/sitemap-index.xml` | `@astrojs/sitemap` | Integration | Auto-generated XML sitemap containing all published routes. |
| `/404.html` | `src/pages/404.astro` | Fallback | Accessible 404 page for missing routes. |

---

## 3. Directory Layout & Module Responsibilities

```
src/
├── assets/
│   ├── icons/            # SVG icons for UI navigation and social platforms
│   └── images/           # Workspace for author-uploaded images and assets
├── components/           # General UI components (Header, Footer, Card, Breadcrumb, etc.)
├── content/
│   ├── pages/            # Markdown content for standalone pages (e.g. about.md)
│   └── posts/            # Markdown / MDX articles for the blog
├── i18n/                 # Localization and UI string dictionaries
├── layouts/              # Top-level HTML shell and OpenGraph/SEO wrappers
│   ├── Layout.astro      # Core HTML shell (head meta, theme script, client router)
│   └── PostLayout.astro  # Article layout injecting JSON-LD schema and article metadata
├── pages/                # Astro page routes (defines the public URL tree)
│   ├── blogs/            # Canonical blog routes and article-specific components
│   │   └── [...slug]/_components/  # TOC, EditPost, ShareLinks, Lightbox, etc.
│   └── projects/         # Portfolio routes
├── styles/               # Tailwind CSS v4 stylesheets, color themes, typography
└── utils/                # Helper functions for URL slugification, sorting, and filters
```

### Key Modules

- **Content Loader & Schemas (`src/content.config.ts`)**: Defines type-safe collections (`posts` and `pages`) using Astro's `glob` loader and Zod schemas. Frontmatter validation occurs at build time.
- **Centralized Configuration (`astro-paper.config.ts`)**: User-editable configuration for site title, author name, public origin, default OG images, timezone, feature flags, and social links.
- **URL Resolution (`src/utils/getPostPaths.ts`)**: Centralizes the `getPostUrl()` logic so internal links across cards, adjacent post navigation, and RSS feeds always stay in sync with the `/blogs/` route structure.

---

## 4. Key Client-Side Features

1. **Light & Dark Theme Persistence**: An inline, render-blocking script in `src/layouts/Layout.astro` reads `localStorage` and OS preferences (`prefers-color-scheme`) before the page paints, preventing Flash of Unstyled Content (FOUC).
2. **Pagefind Full-Text Search**: Built statically into `dist/pagefind/` at compile time. The UI loads lazily when the search page is opened, executing queries against local pre-indexed WebAssembly/JSON bundles with zero network latency.
3. **Article Lightbox & Interactions**: Individual articles provide a reading progress bar, copyable code blocks with custom title bars, an accessible keyboard-navigable image zoom lightbox, and a floating Table of Contents.
4. **Smart Back Navigation**: `src/components/Main.astro` persists the user's referrer route in `sessionStorage` (`backUrl`), allowing back buttons on articles to return the reader to their previous filter (e.g. search query or tag list) rather than resetting to home.
