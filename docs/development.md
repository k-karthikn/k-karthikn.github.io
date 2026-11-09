# Development Guide

This guide covers local environment setup, everyday development workflows, code quality tools, and local testing.

---

## 1. Prerequisites

- **Node.js**: `>= 22.23.4` (Node 22 LTS or Node 24 recommended)
- **Package Manager**: `pnpm` (`>= 11.3.0`)
- **Git**

Verify your environment:

```bash
node -v
pnpm -v
```

---

## 2. Installation & Setup

Clone the repository and install dependencies with a frozen lockfile:

```bash
pnpm install --frozen-lockfile
```

Synchronize Astro content collections and generate type definitions:

```bash
pnpm run sync
```

---

## 3. Available Package Scripts

| Script | Command | Purpose |
| :--- | :--- | :--- |
| `pnpm run dev` | `astro dev` | Starts the Astro development server at `http://localhost:4321/`. |
| `pnpm run build` | `astro check && astro build && pagefind --site dist && node copy-pagefind.mjs` | Runs full type-checking, builds static HTML to `dist/`, generates the Pagefind search index, and copies assets. |
| `pnpm run preview` | `astro preview` | Serves the production build from `dist/` locally for manual review. |
| `pnpm run lint` | `eslint .` | Runs ESLint against Astro, TypeScript, and JavaScript source files. |
| `pnpm run format:check` | `prettier --check .` | Checks all files for Prettier formatting compliance. |
| `pnpm run format` | `prettier --write .` | Automatically formats all files using Prettier. |
| `pnpm run sync` | `astro sync` | Generates TypeScript types for `astro:content` collections. |

---

## 4. Local Testing & Verification Workflow

Before pushing any changes or opening a pull request, run the verification pipeline:

```bash
# 1. Check code quality and formatting
pnpm run lint
pnpm run format:check

# 2. Compile static build and search index
pnpm run build

# 3. Preview output in a browser
pnpm run preview
```

> **Note on Offline Development**: Dynamic Open Graph images leverage `@astrojs/assets` with Google Fonts. In offline environments (or sandboxes without outbound network access), the OG image endpoints fail gracefully and will not break the static build. When deployed to GitHub Actions with internet access, font assets are retrieved automatically.

---

## 5. Docker Local Development (Optional)

A `compose.yaml` and `Dockerfile` are provided for containerized local development:

```bash
# Start dev server inside Docker
docker compose up

# Visit the running container
http://localhost:4321/
```
