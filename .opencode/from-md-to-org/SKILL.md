---
name: from-md-to-org
description: Convert Markdown file to ORG file using the specified format for ox-hugo.
---

To convert blog post from Markdown (`.md`) to ORG (`.org`) please follow the next steps:

1. Set the next header in the org file:

```
#+TITLE: {Post title}
#+DESCRIPTION: {Post description}
#+AUTHOR: Sergio Benítez
#+DATE: {Post Date}
#+STARTUP: fold
#+HUGO_BASE_DIR: ~/Development/suabochica-blog/
#+HUGO_SECTION: /post
#+HUGO_WEIGHT: auto
#+HUGO_AUTO_SET_LASTMOD: t
```

2. Keep the headers (h1,h2,h3), code snippets, block codes, and more...
3. Keep the same name for both files, just change the format.
