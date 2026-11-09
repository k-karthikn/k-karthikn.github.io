# Deployment Guide

This guide details the deployment architecture, hosting environment, and automated GitHub Actions CI/CD workflows for [`https://k-karthikn.github.io/`](https://k-karthikn.github.io/).

---

## 1. Hosting Target & URL Configuration

The application is hosted as a **GitHub Pages User Site** from the repository `k-karthikn/k-karthikn.github.io`.

- **Public Origin**: `https://k-karthikn.github.io/`
- **Root-Hosted Architecture**: The application is deployed at the domain root (`/`). 
- **Base Path Rule**: Never configure Astro's `base` setting to `/blogs`. Astro's `base` option prefixes all site assets and routes across the entire deployment. Sections like `/blogs/` and `/projects/` are handled naturally through standard Astro page routes.

---

## 2. GitHub Pages Settings

To enable automated deployment on GitHub:

1. Navigate to your repository on GitHub: `https://github.com/k-karthikn/k-karthikn.github.io`.
2. Go to **Settings** &rarr; **Pages**.
3. Under **Build and deployment** &rarr; **Source**, select **GitHub Actions**.

---

## 3. GitHub Actions Workflows

Automated CI/CD is managed via two workflows located in `.github/workflows/`:

### A. Production Deployment (`.github/workflows/deploy.yml`)

Runs automatically on every push to the `main` branch or when triggered manually via `workflow_dispatch`.

1. **Checkout**: Checks out the repository code.
2. **Setup**: Installs Node.js 22 and `pnpm` (version 11.3.0).
3. **Dependency Installation**: Runs `pnpm install --frozen-lockfile`.
4. **Build**: Executes `pnpm run build` to type-check, render static HTML, and build Pagefind search indexes.
5. **Artifact Upload**: Packages `./dist` as a GitHub Pages artifact using `actions/upload-pages-artifact@v3`.
6. **Deployment**: Deploys the artifact directly to the GitHub Pages edge network using `actions/deploy-pages@v4`.

### B. Pull Request CI Checks (`.github/workflows/ci.yml`)

Runs automatically on every pull request targeting `main`:

1. Lints codebase via `pnpm run lint`.
2. Validates code formatting with `pnpm run format:check`.
3. Validates static compilation via `pnpm run build`.

---

## 4. Pre-Deployment Checklist

Before merging or pushing commits to `main`, run the local verification suite:

```bash
# 1. Run linting
pnpm run lint

# 2. Check formatting
pnpm run format:check

# 3. Verify static build and search generation
pnpm run build
```
