# Project Exploration Summary (2026-06-11)

## 1. PROJECT STRUCTURE

```
/home/suabochica/Development/suabochica-blog/
├── config.toml              # Main Hugo configuration
├── netlify.toml              # Netlify deployment configuration
├── CLAUDE.md                 # AI assistant guidance
├── README.md                 # Project documentation
├── .gitignore
├── .hugo_build.lock
│
├── content/
│   ├── post/                 # 50 published blog posts (Markdown)
│   └── images/               # Post images organized by topic subdirectories
│
├── src/                      # Draft blog posts in Org mode (unpublished/works-in-progress)
│   ├── blockchain/           # 14 Org draft posts (3 fundamentals published, 11 unpublished)
│   │   ├── fundamentals/     # 5 drafts (3 published, 2 unpublished with * prefix)
│   │   ├── ethereum/         # 3 unpublished drafts (* prefix)
│   │   ├── architecture/     # 2 unpublished drafts (* prefix)
│   │   └── dapps/            # 4 unpublished drafts (* prefix)
│   ├── emacs/                # 9 drafts (3 published as Markdown, 6 unpublished)
│   ├── foundations/          # 2 drafts (0 published, 1 with * prefix = unpublished)
│   ├── hugo/                 # 4 placeholder/template .md files (from theme example)
│   ├── javascript/           # 7 drafts (0 published; 5 ES, 2 EN)
│   │   └── en/               # 2 English drafts using new JS# format
│   ├── microservices/        # 5 drafts (4 published, 1 unpublished docker draft)
│   │   └── docker/           # 1 unpublished draft (* prefix)
│   ├── react/                # 15 drafts across 6 sub-topics
│   │   ├── hooks/            # 2 (1 published, 1 unpublished)
│   │   ├── overview/         # 3 (all published)
│   │   ├── pure/             # 2 (unpublished)
│   │   ├── server-rendering/ # 4 (all unpublished)
│   │   ├── styles/           # 3 (unpublished)
│   │   └── tools/            # 1 (unpublished)
│   ├── security/             # 1 unpublished draft (* prefix, in English)
│   └── storybook/            # 6 drafts (all published)
│
├── slides/                   # RevealJS presentations
│   └── clean-arch/           # "Clean Architecture in Node.js" (org + exported html)
│
├── themes/m10c/              # Hugo theme (MIT license, by Fabien Casters "vaga")
│   ├── layouts/              # 7 template files
│   │   ├── _default/         # baseof.html, list.html, single.html, terms.html
│   │   ├── partials/         # icon.html, pagination.html
│   │   └── 404.html
│   ├── assets/css/           # 12 SCSS files (main.scss + components)
│   ├── static/               # Static assets (fonts, favicon, profile image)
│   ├── data/m10c/            # icons.json (Feather SVG icons dictionary)
│   ├── scripts/              # update_feather_icons.sh
│   ├── exampleSite/          # Theme demo content
│   └── theme.toml            # Theme metadata
│
├── public/                   # Generated site output (gitignored)
└── resources/                # Hugo resource cache (gitignored)
```

## 2. THEME DETAILS

**Theme**: `m10c` -- "A minimalistic (m10c) theme for bloggers"
- **Author**: Fabien Casters ("vaga") -- https://vaga.io
- **License**: MIT
- **Homepage**: https://github.com/vaga/hugo-theme-m10c
- **Minimum Hugo version**: 0.55

### Template files (7 total)
- `layouts/_default/baseof.html` -- Main base template with CSS Grid layout (header, post, TOC sidebar)
- `layouts/_default/single.html` -- Individual post page (title, meta with date/reading time/tags, content, Disqus)
- `layouts/_default/list.html` -- List page showing paginated posts with date and reading time
- `layouts/_default/terms.html` -- Tags taxonomy page with post counts
- `layouts/partials/icon.html` -- Renders Feather SVG icons from `data/m10c/icons.json`
- `layouts/partials/pagination.html` -- Page navigation (prev/next with page numbers)
- `layouts/404.html` -- Custom 404 error page

### SCSS architecture (12 files)
- `main.scss` -- entry point, injects Hugo theme color variables, imports all components
- `_base.scss` -- resets, typography (iA Writer fonts), code blocks, tables
- `components/_app.scss` -- CSS Grid layout (3-column at 940px+)
- `components/_post.scss` -- Post title, content images, code block left-border accent
- `components/_posts_list.scss` -- Blog list items with dashed separators
- `components/_tags_list.scss` -- Tag list with flexbox wrapping at 450px+
- `components/_tag.scss` -- Individual tag pill styling with hover effects
- `components/_fonts.scss` -- Self-hosted iA Writer fonts (Duo, Quattro, Mono)
- `components/_toc.scss` -- Table of contents sidebar with left border accent
- `components/_icon.scss` -- SVG icon sizing
- `components/_pagination.scss` -- Page number styling with active state
- `components/_error_404.scss` -- 404 page centered layout
- `_extra.scss` -- Placeholder for project-specific CSS overrides (currently empty)

### Customizations made to the theme
1. Custom profile image (`sua_profile.jpg`) and favicon (`favicon.ico`) in `themes/m10c/static/`
2. Gruvbox-inspired color palette overridden in root `config.toml`
3. Base template modified to use Hugo's `.TableOfContents` variable in the sidebar
4. No project-level `layouts/` or `static/` directories
5. `_extra.scss` is still at its default (empty)

## 3. CONTENT

**50 published blog posts** in `/content/post/`, all in Markdown with TOML frontmatter (`+++`).

**Languages**: Predominantly Spanish. Some English content in drafts under `src/`.

### Tags used across all posts

| Category | Tags |
|----------|------|
| **Web Development** | `javascript` (5), `react` (4), `react-hooks` (1), `css` (1), `functional-programming` (1) |
| **HTTP/Security** | `HTTP` (6) |
| **Operations** | `bash` (4), `ops` (5, includes Ubuntu) |
| **Microservices** | `microservicios` (4) |
| **Blockchain** | `blockchain` (3) |
| **Tooling** | `emacs` (3), `spacemacs` (3), `storybook` (6) |
| **Design** | `product-design` (1), `graphic-design` (2), `typography` (1), `editorial` (1), `web-design` (2) |
| **Games** | `videogames` (3) |
| **Personal/Misc** | `travel` (1), `misc` (1), `conf` (2), `labs` (1) |

**Post date range**: June 2011 through November 2021

### Content images directories
`blockchain/` (20), `react/` (5), `microservices/` (16), `storybook/` (10), `emacs/` (4), `kokolock/` (7), `tacito/` (4), `sudamerica/` (2), `tipos_moviles/` (4), `cuadrante_phi/` (3), `funtlz/` (2)

## 4. CONFIGURATION

**`config.toml`**:
```toml
baseurl = "https://blog.suabochica.com"
title = "Blog de suabochica"
theme = "m10c"
paginate = 12
```
- Author: Sergio Benítez
- Social links: porfolio (suabochica.com), github, twitter, instagram
- Custom Gruvbox-dark color scheme

**`netlify.toml`**:
- Build: `hugo --gc --minify`
- Hugo version: 0.83.1
- Multiple deployment contexts: production, deploy-preview, branch-deploy
- Deploy preview supports `--buildFuture` for draft previews

## 5. CUSTOM LAYOUTS/SHORTCODES

**None found.** No `layouts/` or `shortcodes/` directories at project root. The project relies entirely on the m10c theme's built-in templates.

## 6. BUILD PROCESS

```sh
hugo server          # Dev server at http://localhost:1313
hugo                 # Full build to public/
rm -rf public resources && hugo  # Clean build
```

**CI/CD**: Netlify deploys on git push. No Makefile, shell scripts, or GitHub Actions.

## 7. DRAFT/UNPUBLISHED CONTENT

### Draft counts by topic

| Topic | Published | Unpublished (`*` prefix) | WIP (`***` prefix) | Total |
|-------|-----------|--------------------------|---------------------|-------|
| storybook | 6 | 0 | 0 | 6 |
| microservices | 4 | 1 | 0 | 5 |
| emacs | 3 | 6 | 0 | 9 |
| blockchain/fundamentals | 3 | 2 | 0 | 5 |
| blockchain/ethereum | 0 | 3 | 0 | 3 |
| blockchain/architecture | 0 | 2 | 0 | 2 |
| blockchain/dapps | 0 | 4 | 0 | 4 |
| react/overview | 3 | 0 | 0 | 3 |
| react/hooks | 1 | 1 | 0 | 2 |
| react/pure | 0 | 2 | 0 | 2 |
| react/server-rendering | 0 | 4 | 0 | 4 |
| react/styles | 0 | 3 | 0 | 3 |
| react/tools | 0 | 1 | 0 | 1 |
| javascript | 0 | 7 | 0 | 7 |
| security | 0 | 1 | 0 | 1 |
| foundations | 0 | 1 | 0 | 2 |
| hugo | 0 | 4 (template files, not real drafts) | 0 | 4 |

**Total unpublished drafts**: ~41 Org mode files across all topics (excluding the 4 hugo template files).

### Filename conventions
- `*` prefix = unpublished post
- `***` prefix = work-in-progress post
- Timestamps follow prefixes (e.g., `2021-11-17*-datos-en-blockchain.org`)
- New format: theme ID + `#` + incremental number (e.g., `JS#00.org`)
- `en` subdirectory or `en` in name = English content

### Publishing workflow (from README.md)
1. Write draft in Org mode under `src/<topic>/`
2. Export Org to Markdown using ox-hugo: `SPC m e m m` (Doom Emacs keybinding)
3. Add tags to the frontmatter of the exported `.md` file
4. Copy the `.md` file into `content/post/`
5. Clean build: `rm -rf public resources && hugo`

### Slides
- `slides/clean-arch/` -- "Clean Architecture in Node.js" (org-reveal, exported to HTML)
- Uses org-reveal for export: `SPC m e R R`
- RevealJS version 4, loaded from CDN
