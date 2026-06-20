# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a personal academic portfolio website built on the **Academic Pages** Jekyll template (a fork of Minimal Mistakes), deployed via **GitHub Pages** at `frosty-jackal.github.io`. The site owner is Rui Hou (Frosty-Jackal), an AI student at Sichuan University.

## Commands

### Local development (Ruby/Jekyll)

```bash
# Install Ruby dependencies (run once)
bundle install

# Start Jekyll dev server with live reload
jekyll serve -l -H localhost
# or explicitly:
bundle exec jekyll serve -l -H localhost

# Site available at http://localhost:4000
```

### Docker development

```bash
docker compose up
# Site at http://localhost:4000
```

When using VS Code, the Dev Container (`.devcontainer/devcontainer.json`) handles this automatically — reopen with **F1 → DevContainer: Reopen in Container**.

### JavaScript build

```bash
npm install                  # Install JS dependencies
npm run build:js             # Minify JS into assets/js/main.min.js
npm run watch:js             # Watch JS files and rebuild on change
```

The JS build pipeline concatenates and minifies: jQuery, FitVids, SmoothScroll, Plotly, GreedyNavigation, and `_main.js` into `assets/js/main.min.js`. The unminified source (`assets/js/_main.js`) and `assets/js/plugins/` are excluded from the Jekyll build (set in `_config.yml`).

### Markdown generators (optional)

Python scripts and Jupyter notebooks in `markdown_generator/` convert TSV files (`talks.tsv`, `publications.tsv`) into properly-formatted markdown files for the respective collections. Use these when bulk-adding publications or talks.

## Architecture

### Static site generator

This is a **Jekyll** site. All content is authored in Markdown/HTML and compiled to a static site by Jekyll, then served by GitHub Pages (which uses the `github-pages` gem). The `Gemfile` pins `github-pages` which dictates compatible gem versions.

Jekyll scans directories at build time. The following are treated as **collections** (defined in `_config.yml`): `_teaching`, `_publications`, `_portfolio`, `_talks`. Each collection item is a Markdown file with YAML frontmatter that gets rendered with the layout specified in `_config.yml` defaults.

### Key directories

| Directory | Purpose |
|---|---|
| `_config.yml` | All site-wide configuration: author info, theme, collections, plugins, analytics, SEO |
| `_pages/` | Static pages (about, cv, publications listing, talks listing, etc.) |
| `_data/` | Structured data files — `authors.yml` (multi-author bios), `cv.json` (JSON Resume format for the CV page), `ui-text.yml` (localized UI strings) |
| `_includes/` | Reusable HTML/Liquid partials (masthead, sidebar, footer, analytics, comments, SEO tags, etc.) |
| `_layouts/` | Page layout templates — `single.html` for most pages, `talk.html` for talks, `cv-layout.html` for CV, `archive.html` for archive pages |
| `_sass/` | SCSS source files, imported in order by `assets/css/main.scss` |
| `assets/` | Compiled/minified JS, CSS webfonts, and images |

### Theme system

The site supports 6 visual themes: `default`, `air`, `sunrise`, `mint`, `dirt`, `contrast`. Each has a `_light` and `_dark` variant (12 SCSS files in `_sass/theme/`). The active theme is set by `site_theme` in `_config.yml`. Dark/light mode toggling happens client-side via `localStorage` and the `data-theme` attribute on `<html>`, managed by `assets/js/_main.js`.

### Content type → layout mapping

Content type determines which layout renders it, configured in `_config.yml` defaults:

- **Posts** (`_posts/`) → `single` layout with author profile, read time, comments, share, related
- **Pages** (`_pages/`) → `single` layout with author profile
- **Publications** (`_publications/`) → `single` layout
- **Portfolio** (`_portfolio/`) → `single` layout
- **Teaching** (`_teaching/`) → `single` layout
- **Talks** (`_talks/`) → `talk` layout (different from the rest)

### Sidebar / author profile

The left sidebar is rendered by `_includes/author-profile.html` and populated from `_config.yml` → `author:` section, with fallback to `_data/authors.yml`. The sidebar shows avatar, bio, location, and social/ academic links. Social link icons are controlled by which fields are populated in `author:` (blank = hidden).

### JavaScript modules

`assets/js/_main.js` uses ES modules: it imports theme layout config from `assets/js/theme.js`. The file handles: theme detection/toggling, Plotly chart rendering from Markdown code blocks, sticky footer resizing, smooth scrolling, and the follow-menu dropdown.

### CV page

The CV page (`_pages/cv.md`) renders from `_data/cv.json` using the JSON Resume schema format. The `_includes/cv-template.html` partial reads this JSON and renders education, publications, presentations, teaching, and portfolio sections. The CV JSON and collection markdown files are **independent** — updating one does not update the other.

### Docker setup

- `Dockerfile` — builds a Ruby 3.2 image with Jekyll dependencies, runs as non-root user `vscode` (UID 1000)
- `docker-compose.yaml` — mounts the repo at `/usr/src/app`, exposes port 4000, runs Jekyll with both `_config.yml` and `_config_docker.yml` (which overrides `url` to empty for local dev)
- `.devcontainer/devcontainer.json` — VS Code Dev Container config referencing the docker-compose service

## Configuration notes

- The site is primarily **English**. The portfolio collection (`_portfolio/`) retains Chinese content. `_config.yml` author fields (bio, location, employer) remain in Chinese to reflect the site owner's actual affiliation. When editing `.md` files, use English; portfolio items are the exception.
- `_config.yml` → `url` is set to `https://frosty-jackal.github.io`. For local dev with Docker, `_config_docker.yml` overrides this to empty (relative URLs).
- The template is detached from upstream (`academicpages/academicpages.github.io`), so syncing upstream changes is done manually — do not run `git merge upstream/main` without careful conflict resolution.
- `_config.yml` changes require a full Jekyll restart; Markdown/HTML changes are picked up automatically with the `-l` (live reload) flag.
