---
name: from-md-to-org
description: Convert a Markdown blog post to Org mode format with ox-hugo headers. Use when a .md draft needs to be converted to .org for Emacs editing or ox-hugo export.
---

To convert a blog post from Markdown (`.md`) to Org (`.org`), follow these steps:

## 1. Add the ox-hugo header

```
#+TITLE: {Post title}
#+DESCRIPTION: {Post description}
#+AUTHOR: Sergio Benítez
#+DATE: {Post Date in org format: <YYYY-MM-DD Day>}
#+STARTUP: fold
#+HUGO_BASE_DIR: ~/Development/suabochica-blog/
#+HUGO_SECTION: /post
#+HUGO_WEIGHT: auto
#+HUGO_AUTO_SET_LASTMOD: t
```

## 2. Convert syntax (Markdown → Org)

| Markdown | Org |
|----------|-----|
| `# Heading` | `* Heading` |
| `## Heading` | `** Heading` |
| `### Heading` | `*** Heading` |
| `` `inline code` `` | `~inline code~` |
| `_italic_` | `/italic/` |
| `__italic__` | `/italic/` |
| `**bold**` | `*bold*` |
| ` ```lang ` (open) | `#+BEGIN_SRC lang` |
| ` ``` ` (close) | `#+END_SRC` |
| Unordered `- item` | `- item` (same) |
| Ordered `1. item` | `1. item` (same) |
| `> blockquote` | `#+BEGIN_QUOTE` / `#+END_QUOTE` |
| `---` (horizontal rule) | `-----` |
| `[text](url)` | `[[url][text]]` |
| `![alt](url)` | `[[url]]` |

## 3. Naming

Keep the same filename, only change the extension from `.md` to `.org`.
