---
name: export-org-to-md
description: Export the ORG files to Markdown files using the Emacs export file functionality, and post the Markdown.
---

To export blog post from ORG `.org` to Markdown (`.md`) I use the [ox-hugo](https://ox-hugo.scripter.co/)

1. Use ORG mode to create drafts.
2. Once the ORG post is finishes, export the ORG to markdown using the `SPC m e m m` keybindings.
3. Add tags in the exported markdown file.
4. Copy the generated `.md` file into the `/content/post` folder.
5. Run `rm -rf /public /resources`.
6. Run `hugo` to test that the new post was published.
