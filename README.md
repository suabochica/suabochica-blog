# 📝 Suabochica Blog

## 🧰  Tech Stack

- [hugo](https://gohugo.io/getting-started/quick-start/)

## TODO

[ ] Check how to compile theme changes

## 🗂️ Workflow

1. Use ORG mode to create drafts.
2. Export ORG scratch to markdown.
3. Add tags in markdown file

## 🚀 Build

Command to run the hugo server

```sh
hugo server
```

Command to build the page

```sh
hugo
```

## :file_folder:  Folder structure

```txt
.
├── content
│   ├── images
│   │   ├── jpg
│   │   ├── microservices
│   │   └── png
│   └── post
├── resources
│   └── _gen
│       └── images
├── src
│   ├── emacs
│   └── microservices
└── themes
    └── m10c
        ├── assets
        │   └── css
        │       └── components
        ├── data
        │   └── m10c
        ├── exampleSite
        │   └── content
        │       └── posts
        ├── images
        ├── layouts
        │   ├── _default
        │   └── partials
        ├── resources
        │   └── _gen
        │       └── assets
        │           └── scss
        │               └── css
        ├── scripts
        └── static
```

From these folder structure, below I list the relevant ones:

- `content/images`: folder with all the local images
- `content/post`: folder with all posts. They should be in markdown
- `src/`: folder with in progress posts. They are written in ~.org~ and should be exported to markdown
<<<<<<< HEAD
- `themes/m10c`: folder with the hugo themes for the blog.

## 👁️‍🗨️ Overview

- filenames that start with the `***` prefix are _in progress_ posts.
- filenames that start with the `*` prefix are _not published_ posts.
- filenames has a timestamp after prefix.
- filenames that has an `en` in the name, mean that the post is in English.
- filenames that has an `es` in the name, mean that the post is in Spanish.
=======
- `themes/m10c`: folder with hugo themes
>>>>>>> 2bd26b1 (blog: fit some posts.)

## :bookmark: Adding tags

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
