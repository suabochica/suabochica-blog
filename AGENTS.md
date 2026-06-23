# AGENTS.md

This file provides guidance to AI coding agents (OpenCode, Claude Code) when working with code in this repository.

## Project Overview

Personal blog by Sergio Benítez (suabochica). Built with **Hugo 0.83.1** using the **m10c** minimal theme (MIT, by Fabien Casters "vaga"). Content is authored in Org mode (Doom Emacs) and exported to Markdown for Hugo. Hosted on Netlify at `https://blog.suabochica.com`.

## Commands

```sh
hugo server          # Dev server at http://localhost:1313
rm -rf public resources && hugo   # Clean build (always do this before prod)
hugo --gc --minify   # Production build (used by Netlify)
```

## Content Workflow

### Blog Posts
1. Draft in Org mode files under `src/<topic>/`
2. Export to Markdown using ox-hugo: `SPC m e m m` (Doom Emacs)
3. Add tags to format in exported `.md` file
4. Copy `.md` to `content/post/`

### Slides
1. Draft in Org mode under `slides/`
2. Export to RevealJS using org-reveal: `SPC m e R R`

### Filename Conventions
- `*` prefix (anywhere before date): unpublished post (e.g., `*post-topic.org`, `2022-01-02-*dapp-ux.org`)
- `***` prefix: work-in-progress post
- `en` in name: English content
- `es` in name: Spanish content
- Timestamps follow prefixes (e.g., `2021-03-10-por-que-no-react.md`)
- New format (in progress): topic ID + `#` + number (e.g., `JS#00.org`)

## Directory Map

```
content/
  post/              # 50 published blog posts (Markdown, TOML frontmatter)
  images/            # Post images by subdirectory: blockchain/ react/ microservices/
                     #   storybook/ emacs/ kokolock/ tacito/ sudamerica/
                     #   tipos_moviles/ cuadrante_phi/ funtlz/
src/
  blockchain/
    fundamentals/    # 5 org (3 published, 2 unpublished)
    ethereum/        # 3 org (all unpublished)
    architecture/    # 2 org (all unpublished)
    dapps/           # 4 org (all unpublished)
  emacs/             # 9 org (3 published, 6 unpublished)
  foundations/       # 2 org (1 unpublished)
  hugo/              # 4 md template files (not real drafts, ignore)
  javascript/        # 7 org (all unpublished; en/ subdir has English ones)
  microservices/     # 5 org (4 published, 1 unpublished in docker/)
  react/
    hooks/           # 2 org (1 published)
    overview/        # 3 org (all published)
    pure/            # 2 org (unpublished)
    server-rendering/# 4 org (unpublished, English)
    styles/          # 3 org (unpublished)
    tools/           # 1 org (unpublished)
  security/          # 1 org (unpublished, English)
  storybook/         # 6 org (all published)
slides/
  clean-arch/        # "Clean Architecture in Node.js" (org + html, RevealJS v4 CDN)
themes/m10c/         # Theme (7 templates, 12 SCSS files, no project-level layouts/)
public/              # Generated output (gitignored)
```

## Frontmatter Format (TOML)

```toml
+++
title = "Post Title"
date = 2021-03-10
tags = ["tag1", "tag2"]
categories = ["optional"]
+++
```

- `tags` array is **required**; `categories` is optional
- No `draft: true` — drafts are controlled by `*`/`***` filename prefix (not Hugo drafts)
- Frontmatter delimiters: `+++` (TOML), not `---` (YAML)

## Known Tags Inventory

| Category | Tags |
|----------|------|
| Web Dev | `javascript`, `react`, `react-hooks`, `css`, `functional-programming` |
| HTTP/Security | `HTTP` |
| Ops | `bash`, `ops`, `Ubuntu` |
| Microservices | `microservicios` |
| Blockchain | `blockchain` |
| Tooling | `emacs`, `spacemacs`, `storybook` |
| Design | `product-design`, `graphic-design`, `typography`, `editorial`, `web-design` |
| Games | `videogames` |
| Misc | `travel`, `misc`, `conf`, `labs` |

## Theme Customizations

- **Colors** (Gruvbox-dark inspired, set in `config.toml` params):
  - Background: `#282828`, Header bg: `#3c3836`
  - Body text: `#fbf1c7`, Headings: `#689d6a`
  - Links/accent: `#d65d0e`
- **TOC sidebar**: `baseof.html` modified to use Hugo's `.TableOfContents`
- **Static assets**: Custom profile image (`sua_profile.jpg`) and favicon (`favicon.ico`) in `themes/m10c/static/`
- **No project-level** `layouts/`, `static/`, or `shortcodes/` directories exist
- **`_extra.scss`** is empty — use this for any project-specific CSS

## Config Summary

```toml
baseurl = "https://blog.suabochica.com"
title = "Blog de suabochica"
theme = "m10c"
paginate = 12
```
- Author: Sergio Benítez
- Social: porfolio (suabochica.com), github, twitter, instagram
- Netlify: `hugo --gc --minify` with Hugo 0.83.1; deploy previews use `--buildFuture`

## Key Constraints

- Avoid `#+BEGIN-CENTER` blocks in Org (ox-hugo export issues)
- First heading in post content must be `h2` (TOC compatibility)
- Post images go in `content/images/<topic>/` not `static/`
- 50 posts published, ~41 Org drafts pending export (blockchain: 11, react: 10, emacs: 6, javascript: 7)
