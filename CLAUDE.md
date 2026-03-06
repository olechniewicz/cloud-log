# CLAUDE.md

This file documents the codebase structure, conventions, and development workflows for AI assistants working on this repository.

## Project Overview

**cloud-log** is a minimalist static blog built with [Jekyll](https://jekyllrb.com/). It serves as a public worklog for documenting cloud systems and architecture topics. There are no npm, Python, or Ruby gem dependencies — Jekyll itself is the only tool needed to build and serve the site.

## Repository Structure

```
cloud-log/
├── _config.yml          # Jekyll site configuration
├── index.md             # Blog home page (lists all posts)
├── README.md            # Brief project description
├── _layouts/
│   ├── home.html        # Layout template for the home page
│   ├── post.html        # Layout template for individual posts
│   └── style.css        # Duplicate of assets/style.css (kept in sync)
├── _posts/              # Blog posts in Markdown
│   └── YYYY-MM-DD-slug.md
└── assets/
    └── style.css        # Main site stylesheet
```

## Jekyll Configuration (`_config.yml`)

```yaml
title: Blog
theme: null          # No external theme — fully custom layouts
baseurl: /cloud-log  # Deployed under a subpath
permalink: /:title/  # URLs use post title (slug), not date
```

The `baseurl` is `/cloud-log`, so all internal links must use `{{ site.baseurl }}` as a prefix (e.g., `{{ site.baseurl }}/assets/style.css`).

## Creating Blog Posts

Posts live in `_posts/` and follow Jekyll's standard naming convention:

```
_posts/YYYY-MM-DD-post-title-slug.md
```

Every post file requires a YAML front matter block at the top:

```yaml
---
layout: post
title: Your Post Title Here
---

Post content in Markdown goes here.
```

- `layout: post` is required — it applies `_layouts/post.html`.
- `title` is required — it appears in the `<title>` tag and is used in the post list on the home page.
- The permalink is derived from the `title` field per the `/:title/` permalink setting (not the filename slug).

## Layouts

### `_layouts/home.html`
Wraps `index.md`. Renders the site title in `<title>`, links the stylesheet, and injects `{{ content }}` inside `<main>`.

### `_layouts/post.html`
Wraps individual posts. Renders `{{ page.title }} | {{ site.title }}` in `<title>`, includes a `← blog` back-link to `{{ site.baseurl }}/`, and injects `{{ content }}`.

Both layouts set `lang="pl"` (Polish) on the `<html>` element.

## Styling

All styles live in `assets/style.css`. The `_layouts/style.css` file is a copy — keep them in sync when making changes.

Design tokens (CSS custom properties defined on `:root`):

| Variable    | Value       | Usage              |
|-------------|-------------|--------------------|
| `--bg`      | `#0e0e11`   | Page background    |
| `--fg`      | `#e6e6eb`   | Body text          |
| `--muted`   | `#9aa0a6`   | Secondary/footer text |
| `--accent`  | `#7aa2f7`   | Links              |

Key layout rules:
- `main` is constrained to `max-width: 760px`, centered with `margin: 0 auto`.
- Font stack: `system-ui, -apple-system, Segoe UI, Roboto, monospace`.
- Links have no underline by default; underline appears on `:hover`.

## Development Workflow

### Building and serving locally

Jekyll must be available on the system. Standard commands:

```bash
# Serve with live reload (recommended for development)
jekyll serve --baseurl ""

# Build to ./_site/ for production
jekyll build
```

Pass `--baseurl ""` when serving locally so that links work at `http://localhost:4000` instead of `http://localhost:4000/cloud-log`.

### No linting, tests, or CI

There are no configured linters, formatters, test suites, or CI/CD pipelines. No pre-commit hooks are active.

## Key Conventions

- **Language attribute**: All HTML layouts use `lang="pl"`. Keep this unless the project language changes.
- **Baseurl prefix**: Always prefix asset/page links with `{{ site.baseurl }}` in templates and content.
- **No inline styles**: All styling goes through `assets/style.css` (and its mirror `_layouts/style.css`).
- **No theme**: `theme: null` in `_config.yml`. Do not introduce an external Jekyll theme without discussion.
- **Post naming**: Files must follow `YYYY-MM-DD-slug.md` — Jekyll ignores files that don't match this pattern.
- **Minimal markup**: Layouts are intentionally bare. Avoid adding navigation, sidebars, or other chrome unless asked.
