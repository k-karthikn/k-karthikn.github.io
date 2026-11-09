# Instructions for Coding Agents (AGENTS.md)

This document provides concise instructions and architectural guardrails for AI coding assistants working in this repository.

---

## 1. Project Overview

- **Repository**: Personal developer website and devlog for **Karthikeyan K**.
- **Tech Stack**: Astro 7, TypeScript, Tailwind CSS v4, Pagefind (search), Satori/Sharp (OG images).
- **Target Host**: GitHub Pages (`https://k-karthikn.github.io/`).

---

## 2. Core Architectural Invariants

1. **Blog Route Invariant (`/blogs/`)**:
   - The canonical blog route is `/blogs/` (listing) and `/blogs/<slug>/` (individual posts).
   - **Never re-create `/posts/` routes.** Legacy `/posts/` paths were intentionally removed to prevent duplicate routes.
   - When generating or linking to post URLs in Astro components or endpoints, always call `getPostUrl(id, filePath, locale)` from `src/utils/getPostPaths.ts`.

2. **Single-Application Deployment Invariant**:
   - The entire website is a single Astro application hosted at domain root (`/`).
   - **Never configure Astro's `base` setting to `"/blogs"`.** Sections like `/blogs/` and `/projects/` are standard Astro routes.

3. **Content Collection Invariant**:
   - Blog posts are stored in `src/content/posts/` (loaded by `posts` collection in `src/content.config.ts`).
   - Standalone pages are stored in `src/content/pages/` (loaded by `pages` collection).
   - Any file or folder starting with an underscore (`_`) is ignored by the content loader.

4. **Preserve Integrity & No Fabricated Content**:
   - Never invent fake personal details, employer names, or fabricated project case studies.
   - Central site configuration (author name, social links, feature toggles) lives in `astro-paper.config.ts`.

---

## 3. Directory Map

| Path | Responsibility |
| :--- | :--- |
| `src/pages/` | Public routing tree. `/blogs/`, `/projects/`, `/about/`, `/archives/`, `/tags/`, `/search/`, `index.astro`. |
| `src/content/` | Type-safe Markdown/MDX content (`posts/` and `pages/`). |
| `src/components/` | Shared UI components (`Header.astro`, `Footer.astro`, `Card.astro`, `Breadcrumb.astro`, etc.). |
| `src/layouts/` | `Layout.astro` (HTML shell, head metadata, FOUC-free theme script) and `PostLayout.astro` (article JSON-LD). |
| `src/styles/` | Tailwind CSS v4 stylesheets and typography. |
| `src/utils/` | URL helpers (`getPostPaths.ts`), post sorting (`getSortedPosts.ts`), and filtering. |
| `astro-paper.config.ts` | User-editable site configuration and feature flags. |
| `docs/` | Technical documentation (`architecture.md`, `development.md`, `content-authoring.md`, `deployment.md`). |

---

## 4. Verification Workflow

Always run and verify these commands before concluding any task:

```bash
# 1. Check linting (ESLint)
pnpm run lint

# 2. Check formatting (Prettier)
pnpm run format:check
# (Run `pnpm run format` if formatting needs to be updated)

# 3. Type-check, compile static HTML, and build Pagefind search indexes
pnpm run build
```
