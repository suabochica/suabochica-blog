---
name: export-org-to-md
description: Export an Org mode draft to a Hugo-ready Markdown file with TOML frontmatter and publish it. Use when an .org draft is ready for publishing.
---

To export a blog post from Org (`.org`) to a published Markdown (`.md`) file:

## Preferred method: Emacs ox-hugo (GUI)

1. Open the `.org` file in Doom Emacs.
2. Export with `SPC m e m m` (calls `org-hugo-export-wim-to-md`).
3. The exported `.md` file will appear in `content/post/`.
4. Add tags to the exported file's TOML frontmatter.
5. Run `rm -rf public resources && hugo` to verify.

## Fallback method: Manual conversion

When Emacs batch export is unavailable, manually create the file in `content/post/`:

### TOML frontmatter format

```toml
+++
title = "Post Title"
date = YYYY-MM-DDThh:mm:ssZ
author = "Sergio L. Benítez D."
description = "Brief description."
tags = [
    "tag1",
    "tag2",
]
+++
```

### Org → Markdown syntax conversion

| Org | Markdown (content/post/) |
|-----|--------------------------|
| `* Heading` | `Heading` + `===` underline |
| `** Heading` | `Heading` + `---` underline |
| `*** Heading` | `### Heading` |
| `~code~` | `` `code` `` |
| `/italic/` | `_italic_` |
| `*bold*` | `**bold**` |
| `#+BEGIN_SRC lang` / `#+END_SRC` | ` ```lang ` fences |
| `[[url][text]]` | `[text](url)` |
| `- item` | `- item` (same) |

### Post-publish

```sh
rm -rf public resources && hugo
```

Confirm the post is published (check `public/post/<post-slug>/index.html` exists).
