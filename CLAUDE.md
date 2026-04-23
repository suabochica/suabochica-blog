# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal blog built with **Hugo** (extended version) using the **m10c** theme. Content is authored in Org mode (Emacs) and exported to Markdown for Hugo.

## Commands

```sh
hugo              # Build the site
hugo server       # Start dev server at http://localhost:1313
```

Before building, clean the public directory:
```sh
rm -rf public resources
hugo
```

## Content Workflow

### Blog Posts
1. Draft in Org mode files under `src/` subdirectories (categorized by topic: javascript, react, blockchain, etc.)
2. Export to Markdown using ox-hugo in Emacs (`SPC m e m m`)
3. Add tags to frontmatter in exported `.md` file
4. Copy `.md` to `content/post/`

### Slides
1. Draft in Org mode under `slides/`
2. Export to RevealJS using org-reveal (`SPC m e R R`)

### Filename Conventions
- `*` prefix: unpublished posts
- `***` prefix: work-in-progress posts
- `en` in name: English content
- `es` in name: Spanish content
- Timestamps follow prefixes (e.g., `2021-03-10-por-que-no-react.md`)

## Architecture

```
content/post/     # Published blog posts (Markdown)
src/              # Draft posts (Org mode), organized by topic
slides/           # RevealJS slide decks (Org mode)
themes/m10c/      # Hugo theme (customized)
public/           # Generated site output
```

### Key Constraints
- Avoid `#+BEGIN-CENTER` blocks (ox-hugo export issues)
- Use `h2` as first heading in content for TOC compatibility
- Frontmatter requires `tags` array; `categories` optional
