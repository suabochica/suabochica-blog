---
name: post-redux
description: Translate an English Redux blog post into Spanish, create Org draft, and publish it. Use when the user wants to publish or translate a Redux-related post from src/react/redux/en/.
---

You are a professional bilingual blogger and localization expert. Your task is to translate an English Redux blog post into Spanish and publish it. Follow these steps:

## Step 1: Identify the source file

Look in `src/react/redux/en/` for the English Markdown post to translate. If the user doesn't specify which one, ask.

## Step 2: Translate to Spanish

Translate the post following these rules:
- Provide localized, natural-sounding Spanish (not a literal word-for-word translation).
- Maintain the original Markdown formatting: headings, bullet points, code snippets, etc.
- Adapt idioms and expressions for a native-level reading experience.
- Preserve SEO structure with localized keyword equivalents where appropriate.

Save the translated Markdown file in `src/react/redux/es/` with the same filename.

## Step 3: Create the Org mode draft

Convert the translated Markdown to Org mode format. Save it in `src/react/redux/es/` with the same filename but `.org` extension.

Use this header template:

```
#+TITLE: {Post title}
#+DESCRIPTION: {Brief description in Spanish}
#+AUTHOR: Sergio Benítez
#+DATE: <YYYY-MM-DD Day>
#+STARTUP: fold
#+HUGO_BASE_DIR: ~/Development/suabochica-blog/
#+HUGO_SECTION: /post
#+HUGO_WEIGHT: auto
#+HUGO_AUTO_SET_LASTMOD: t
```

Syntax conversion (Markdown → Org):
| Markdown | Org |
|----------|-----|
| `# Heading` | `* Heading` |
| `## Heading` | `** Heading` |
| `### Heading` | `*** Heading` |
| `` `code` `` | `~code~` |
| `_italic_` | `/italic/` |
| `**bold**` | `*bold*` |
| ` ```lang ` blocks | `#+BEGIN_SRC lang` / `#+END_SRC` |
| `---` (hr) | `-----` |

## Step 4: Publish to content/post/

Create the final Markdown file in `content/post/` with TOML frontmatter:

```toml
+++
title = "Título en español"
date = YYYY-MM-DDThh:mm:ssZ
author = "Sergio L. Benítez D."
description = "Descripción breve en español."
tags = [
    "javascript",
    "react",
    "redux",
]
+++
```

Use the same date as the source post. Tags should include `javascript`, `react`, `redux` plus any topic-specific tags from the [known tags inventory](../../AGENTS.md).

Convert the Org content back to Markdown for this file:
- `* Heading` → `Heading` with `===` underline
- `** Heading` → `Heading` with `---` underline
- `*** Heading` → `### Heading`
- `~code~` → `` `code` ``
- `/italic/` → `_italic_`
- `#+BEGIN_SRC lang` / `#+END_SRC` → ` ```lang ` fences

## Step 5: Build and verify

```sh
rm -rf public resources && hugo
```

Confirm the post appears in the build output (page count should increase by 1). If the build fails, fix any issues and rebuild.
