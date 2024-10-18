# 📝 Suabochica Blog

## TODO

[x] Check how to compile theme changes.
[x] Check how to consume iA Writer fonts.
[ ] Customize a theme for RevealJS.
[ ] Optimize images.

## 🧰 Tech Stack

- [hugo](https://gohugo.io/getting-started/quick-start/), a go framework for build websites.

## 🗂 Workflow

In this blog I have to types of exports:

- Blog posts
- Slides with RevealJS

Both, have the same source; the `ORG` mode of emacs. I'm using Doom Emacs as a framework configuration network.

To export blog post from `.org` to markdown (`.md`) I use the [ox-hugo](https://ox-hugo.scripter.co/)

1. Use ORG mode to create drafts.
2. Export ORG to markdown using the `SPC m e m m` keybindings.
3. Add tags in the exported markdown file
4. Copy the generated `.md` file into the `/content/post` folder.
5. Run `rm -rf /public /resources`
6. Run `hugo`

To export a slide from `.org` to RevealJS I use the [org-reveal](https://alexshroyer.com/posts/2021-08-13-Org-Reveal.html)

1. Use ORG mode to create drafts.
2. Export ORG to RevealJS using the `SPC m e R R` keybindings.
3. Open in your browser the exported file

## 🚀 Commands


Command to build the page

```sh
hugo
```

Command to run the hugo server

```sh
hugo server
```

Then visit the blog in http://localhost:1313

## 📁  Folder structure

    From these folder structure, below I list the relevant ones:

- `content/post`: folder with all posts. They should be in markdown
- `src/`: folder with in progress posts. They write in ~.org~ and should exported to markdown
- `themes/m10c`: folder with the hugo themes for the blog.

## 👁️‍ Overview

- filenames that start with the `***` prefix are _in progress_ posts.
- filenames that start with the `*` prefix are _not published_ posts.
- filenames has a timestamp after prefix.
- filenames that has an `en` in the name, mean that the post is in English.
- filenames that has an `es` in the name, mean that the post is in Spanish.
- `themes/m10c`: folder with hugo themes

## 🔖 Adding tags

To add tags to the posts, at the beginning of each file we have to set the ~tags~ property with an array of the relevant tags. Below it I share an example.

``` markdown
+++
title = "Capricornio 26"
description = "Proyecto mobiliario sobre licorera bogotana"
date = 2011-06-21T02:13:50Z
author = "Sergio Benítez"
tags = [
    "design",
    "furniture",
]

# What is this?

categories = [ 
    "development",
    "golang",
]
+++
```
