# Content Authoring Guide

This guide describes how to write, configure, and publish content on the website.

---

## 1. Content Collections Overview

Content is organized under `src/content/` into two distinct type-safe collections:

- **`posts` (`src/content/posts/`)**: Technical articles, tutorials, devlogs, and notes.
- **`pages` (`src/content/pages/`)**: Standalone markdown pages (such as `about.md`).

Schemas and loaders are configured in [`src/content.config.ts`](../src/content.config.ts). Files starting with an underscore (`_`) are ignored by the loader.

---

## 2. Publishing a Blog Post

Create a new `.md` or `.mdx` file inside `src/content/posts/`. You can organize posts into subdirectories if desired; paths are automatically slugified:

```bash
# Example paths
src/content/posts/my-new-article.md
src/content/posts/linux/custom-shell-workflow.md
```

### Frontmatter Specification

Every blog post must begin with a YAML frontmatter block:

```yaml
---
title: "Building an Embedded Device with Rust"
author: "Karthikeyan K"
pubDatetime: 2026-11-10T10:00:00.000Z
modDatetime: 2026-11-12T14:30:00.000Z
description: "A comprehensive walk-through of firmware development using embedded Rust."
featured: true
draft: false
tags:
  - rust
  - embedded
  - linux
timezone: "Asia/Kolkata"
---
```

### Supported Frontmatter Fields

| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `title` | `string` | **Yes** | Article headline. |
| `description` | `string` | **Yes** | Summary shown in cards, RSS feeds, search, and SEO description tags. |
| `pubDatetime` | `Date` | **Yes** | Publication date/time in ISO 8601 format (e.g. `2026-11-10T10:00:00.000Z`). |
| `author` | `string` | No | Defaults to `site.author` from `astro-paper.config.ts`. |
| `modDatetime` | `Date` | No | Date of last modification. Displayed next to publication date if present. |
| `featured` | `boolean` | No | If `true`, the post appears in the "Featured" section on the homepage. |
| `draft` | `boolean` | No | If `true`, excluded from production builds. (Visible in local `pnpm run dev`). |
| `tags` | `string[]` | No | Topic tags for categorization (defaults to `["others"]`). |
| `ogImage` | `string` / `Image` | No | Custom social card banner. If omitted, a dynamic card is generated automatically. |
| `canonicalURL` | `string` | No | Overrides the canonical URL tag if cross-posting from an external platform. |
| `hideToc` | `boolean` | No | Hides the Table of Contents sidebar for this post. |
| `hideEditPost` | `boolean` | No | Hides the "Edit page" link on GitHub for this post. |
| `timezone` | `string` | No | Specific timezone for rendering byline dates (defaults to site timezone). |

---

## 3. Formatting & Content Features

### Syntax Highlighting with Code Block Titles

Specify language and filename headers using Shiki:

````markdown
```typescript title="src/utils/math.ts"
export function add(a: number, b: number): number {
  return a + b;
}
```
````

Code blocks automatically feature copy-to-clipboard buttons and syntax highlighting for dark and light modes.

### Admonitions / Callouts

Use GitHub-style alert callouts:

```markdown
> [!NOTE]
> Important context or informational detail.

> [!WARNING]
> Critical cautionary notice about system configuration.
```

### Accessible Image Lightbox

Images placed in markdown support click-to-zoom and keyboard navigation automatically:

```markdown
![System Architecture Diagram](@/assets/images/architecture.png)
```

### LaTeX Math Equations

Embed inline math with `$E = mc^2$` or display math with double dollar signs:

```markdown
$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$
```

---

## 4. Standalone Pages & Projects

- **About Page**: Edit [`src/content/pages/about.md`](../src/content/pages/about.md) to update personal bio, technical background, and links.
- **Projects Page**: Edit [`src/pages/projects/index.astro`](../src/pages/projects/index.astro) to showcase repositories, case studies, or hardware experiments.
