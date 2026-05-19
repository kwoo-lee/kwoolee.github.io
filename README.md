# kwoolee.github.io

Personal tech blog. Built with [Astro](https://astro.build), styled after
[Distill](https://distill.pub), hosted on GitHub Pages.

## Local development

```sh
npm install
npm run dev      # http://localhost:4321
```

## Writing

Blog posts live in `src/content/blog/`, projects in `src/content/projects/`.
Each is a Markdown (or MDX) file with frontmatter:

```yaml
---
title: 'Post title'
description: 'One-line summary used in cards and meta tags.'
pubDate: 2026-05-19
tag: 'Note'         # optional badge: Note, Essay, Project, etc.
draft: false        # set true to hide from build
---
```

## Build & deploy

```sh
npm run build    # outputs to ./dist
npm run preview  # serve the built site locally
```

Pushes to `main` trigger `.github/workflows/deploy.yml`, which builds with
Astro and publishes to GitHub Pages.

### One-time GitHub setup

In repo Settings → Pages, set **Source** to **GitHub Actions**.

## Structure

```
src/
  components/   reusable UI (header, footer, article card, head)
  content/      markdown content (blog, projects)
  layouts/      page shells (BaseLayout, PostLayout)
  pages/        routes (index, about, blog, projects, rss.xml)
  styles/       global.css — design tokens + typography
  consts.ts     site title, author, contact info, nav links
```
