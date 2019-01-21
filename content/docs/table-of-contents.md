---
title: "Table of Contents"
date: 2019-01-21T20:09:38Z
---

HugoModo features [Hugo's built-in support for generating a table of contents](https://gohugo.io/content-management/toc/) for a page.

This is off by default, and can be enabled or disabled for the entire site or individual pages.

## Enable for your site

Add this to your site's `config.toml` file:

```toml
[Params]
  toc = true
```

## Enable for a single page

If you only want it enabled for a single page, add this to your page's frontmatter:

```toml
toc = true
```

## Disable for a single page

To disable the table of contents for certain pages, add this to your page's frontmatter:

```toml
toc = false
```
