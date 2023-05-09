# 📝 Suabochica Blog

## 🧰  Tech Stack

- [hugo](https://gohugo.io/getting-started/quick-start/)

## TODO

1. ~~Add favicon~~
2. ~~Add tags~~
3. ~~Organize images~~
4. ~~Customize styles~~ changes applied form: suabochica-blog/themes/m10c/resources/_gen/assets/scss/css/main.scss_e86386c8bae1cd02295de71a1be078d6.content
5. Check how to compile theme changes

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
- `themes/m10c`: folder with in progress posts. They are written in ~.org~ and should be exported to markdown

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
