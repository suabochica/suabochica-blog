# Suabochica Blog

Command to run the hugo server

```
hugo server
```

## Folder structure

```
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

## TODO
1. Add favicon
2. Add tags
3. Organize images
4. Customize styles
