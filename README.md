# Garden - Content for damianflynn.com

This repository contains all the content for my personal website [damianflynn.com](https://damianflynn.com).

## Structure

```
garden/
├── content/           # All content organized by type
│   ├── posts/         # Blog posts
│   ├── series/        # Series collections
│   └── about/         # About pages
├── data/              # Data files (authors, etc.)
└── static/            # Static files (images, etc.)
```

## Adding Content

### New Blog Post

Create a new markdown file in the `content/posts` directory with the following frontmatter:

```markdown
---
title: "Your Post Title"
subtitle: "Optional subtitle"
date: 2023-05-18T10:30:00+08:00
lastmod: 2023-05-18T10:30:00+08:00
draft: false
author: "Damian Flynn"
authorLink: "https://damianflynn.com"
description: "Short description of the post"

tags: ["tag1", "tag2"]
categories: ["category1"]
series: ["series-name"]

featuredImage: "featured-image.jpg"
featuredImagePreview: "featured-image-preview.jpg"

toc:
  enable: true
math:
  enable: false
lightgallery: false
---

Your post content here...
```

### Series Configuration

To create a new series, add a file in the `content/series` directory with information about the series.

## License

The content of this repository is licensed under [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
