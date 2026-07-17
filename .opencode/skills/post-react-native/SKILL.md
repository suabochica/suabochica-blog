---
name: post-react-native
description: Translate an English React Native blog posts into Spanish, create Org draft, and publish it. Use when the user wants to publish or translate a Redux-related post from src/react/react-native/en/.
---

You are a professional bilingual blogger and localization expert. Your task is to translate an English React Native blog posts into Spanish and publish it. Follow these steps:

## Step 1: Identify the source file

Look in `src/react/react-native/en/` for the English Markdown post to translate. If the user doesn't specify which one, ask.

## Step 2: Create the original Org draft file in English

Convert the original Markdown file to Org format using the ../from-md-to-org/SKILL.md. Save the generated file in `src/react/react-native/en` with the same filename

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

Then, delete the original markdown.

## Step 3: Translate the Org draft file to Spanish

Translate the post following these rules:

- Provide localized, natural-sounding Spanish (not a literal word-for-word translation).
- Maintain the original Markdown formatting: headings, bullet points, code snippets, etc.
- Adapt idioms and expressions for a native-level reading experience.
- Preserve SEO structure with localized keyword equivalents where appropriate.

Please, use the ../translate-es/SKILL.md and put the translated Org file in `src/react/react-native/` with the same filename.

## Step 5: Publish to content/post/

Create the final Markdown file in `content/post/` with TOML frontmatter, and using as reference the ../export-org-to-md/SKILL.md.

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

Use the same date as the source post. Tags should include `javascript`, `react`, `react-native` plus any topic-specific tags from the [known tags inventory](../../AGENTS.md).

Convert the Org content back to Markdown for this file:
- `* Heading` → `Heading` with `===` underline
- `** Heading` → `Heading` with `---` underline
- `*** Heading` → `### Heading`
- `~code~` → `` `code` ``
- `/italic/` → `_italic_`
- `#+BEGIN_SRC lang` / `#+END_SRC` → ` ```lang ` fences

## Step 6: Build and verify

```sh
rm -rf public resources && hugo
```

Confirm the post appears in the build output (page count should increase by 1). If the build fails, fix any issues and rebuild. Finally check that under the ../../../src/ folder there is *no* Markdown files.
