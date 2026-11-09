# Karthikeyan K — Developer Website & Devlog

Personal portfolio, technical write-ups, and developer devlog for **Karthikeyan K**, deployed at [**https://k-karthikn.github.io/**](https://k-karthikn.github.io/).

Built with [Astro](https://astro.build/) and [Tailwind CSS](https://tailwindcss.com/), optimized for GitHub Pages static hosting.

---

## 🌟 Public Information Architecture

| Section      | URL                                                    | Purpose                                                  |
| :----------- | :----------------------------------------------------- | :------------------------------------------------------- |
| **Home**     | [`/`](https://k-karthikn.github.io/)                   | Profile introduction, recent posts, and developer bio    |
| **Blogs**    | [`/blogs/`](https://k-karthikn.github.io/blogs/)       | Technical articles, tutorials, and engineering write-ups |
| **Projects** | [`/projects/`](https://k-karthikn.github.io/projects/) | Open-source software and hardware projects               |
| **About**    | [`/about/`](https://k-karthikn.github.io/about/)       | Background, focus areas, and contact links               |
| **Archives** | [`/archives/`](https://k-karthikn.github.io/archives/) | Chronological timeline of published articles             |
| **Tags**     | [`/tags/`](https://k-karthikn.github.io/tags/)         | Browse articles by topic tag                             |
| **Search**   | [`/search/`](https://k-karthikn.github.io/search/)     | Instant static full-text search powered by Pagefind      |
| **RSS Feed** | [`/rss.xml`](https://k-karthikn.github.io/rss.xml)     | Syndication feed for web readers                         |

---

## 🚀 Key Features

- **Blazing-Fast Static Architecture**: Pre-rendered HTML output with near-zero runtime JavaScript overhead.
- **Client-Side Search**: Offline full-text search with [Pagefind](https://pagefind.app/) indexing HTML at build time.
- **Light & Dark Theme**: Zero-FOUC theme switching respecting system preferences and `localStorage`.
- **Accessible & Responsive**: Keyboard-navigable image zoom lightbox, floating Table of Contents, and reading progress indicators.
- **Authoring Experience**: Type-safe Markdown/MDX with frontmatter validation, code block syntax highlighting with Shiki, callout admonitions, and LaTeX math equations.
- **Automated Deployment**: GitHub Actions workflow that tests, builds, and deploys directly to GitHub Pages on every push.

---

## 🛠️ Quick Start

### Prerequisites

- **Node.js**: `>= 22.23.4`
- **pnpm**: `>= 11.3.0`

### Setup & Run

```bash
# 1. Install dependencies
pnpm install --frozen-lockfile

# 2. Start development server
pnpm run dev

# 3. Open in browser
http://localhost:4321/
```

### Verification & Build

```bash
# Lint source code
pnpm run lint

# Check code formatting
pnpm run format:check

# Run full typecheck and build
pnpm run build
```

---

## 📚 Documentation

Detailed documentation is available in the [`docs/`](docs/) directory:

- [**Architecture Guide**](docs/architecture.md) — System design, URL routing, components, and module boundaries.
- [**Development Guide**](docs/development.md) — Local workflow, script references, code quality tools, and Docker usage.
- [**Content Authoring Guide**](docs/content-authoring.md) — How to write new blog posts, frontmatter reference, and formatting features.
- [**Deployment Guide**](docs/deployment.md) — GitHub Pages hosting details and CI/CD workflow configuration.
- [**Agent Guidelines (`AGENTS.md`)**](AGENTS.md) — Instructions and architectural invariants for AI coding assistants.

---

## 📄 License & Attribution

- Built upon the [AstroPaper](https://github.com/satnaing/astro-paper) theme created by [Sat Naing](https://satnaing.dev/).
- Released under the [MIT License](LICENSE).
