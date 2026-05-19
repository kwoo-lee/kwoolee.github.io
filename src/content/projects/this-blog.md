---
title: 'kwoolee.github.io'
description: 'A personal tech blog built with Astro, hosted on GitHub Pages, styled after Distill.'
pubDate: 2026-05-19
url: https://kwoolee.github.io
repo: https://github.com/kwoolee/kwoolee.github.io
tag: 'Web'
---

The site you're reading right now. Built with **Astro 5**, content collections for blog posts and projects, RSS feed, sitemap, and a clean Distill-inspired layout. Deployed automatically to GitHub Pages via GitHub Actions on every push to `main`.

## Overview

The goal was a fast, minimal, content-first blog with as little JavaScript as possible. Astro fits well: it renders everything to static HTML by default, lets me write content in Markdown with typed frontmatter, and the build artifact drops cleanly onto GitHub Pages.

## Stack

- Astro 5 with content collections
- Markdown + MDX
- System sans (Geist) + system mono for body and code
- Plain CSS — no framework, no Tailwind
- GitHub Actions → GitHub Pages

## Design

The visual language is borrowed from [Distill](https://distill.pub): a deep teal header, generous whitespace, narrow reading column, and a small right-hand sidebar with a table of contents and external links.

### Color

A single dark accent (`hsl(200, 60%, 15%)`) for the header, a muted red for inline accents, and otherwise grayscale.

### Typography

Geist for everything — UI, headings, body, and code (Geist Mono). The pairing keeps the look consistent with modern developer tools.

## Deployment

Pushes to `main` trigger `withastro/action@v3`, which builds the site and uploads the artifact. The `actions/deploy-pages@v4` job then publishes to GitHub Pages. Source must be set to **GitHub Actions** in the repo's Pages settings once.
