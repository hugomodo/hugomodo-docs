---
title: "Taxonomies"
date: 2019-02-08T18:59:58Z
---
HugoModo is configured to add seamless taxonomy support to your Hugo website. Simply [add a taxonomy](#add-a-taxonomy), [start using it](#using-a-taxonomy), and your sites will be populated with taxonomy pages and footer navigation links by default. You can [add taxonomy pages](#taxonomy-pages) to customise the leading content which displays on those pages.

A great taxonomy to start with is the authors taxonomy. Let's take a look.

## Add a Taxonomy

By default, Hugo has taxonomies enabled for categories, series and tags. When you add more taxonomies to the configuration, you'll need to add these too if you want to continue using them or else you'll overwrite the defaults.

To add an _authors_ taxonomy, add this to your site's config.toml:

```toml
[Taxonomies]
  category = "categories"
  series = "series"
  tag = "tags"
  author = "authors"
```

## Using a Taxonomy

With the taxonomy configured, you can now use it to list tags in your content page's frontmatter. Let's assume we have a blog post called "My First Blog Post". With an author added, our frontmatter might look like this:

```yaml
title: My First Blog Post
date: 2019-02-08T18:59:58Z
authors:
- John Doe
```

When you publish, the blog post page will be automatically generated as well as additional pages for the authors taxonomy and an author page for John Doe.

## Taxonomy Pages

Taxonomy pages can be customised in much the same way as any other Hugo content page, and HugoModo has native support for this.

You may customise the taxonomy list page itself, as well as individual tag pages.

### Custom Taxonomy Terms Page

In our example, this is the page that will list our authors. To add custom content, simply create a file at `content/authors/_index.md`. Like any other page, this content file can contain frontmatter and content and will be automatically built with your site.

```markdown
---
title: Authors
---
You can add whatever content you like here, as either markdown or HTML. It will appear above the list of authors in your HugoModo theme.
```

### Custom Taxonomy Tag Page

Individual tag pages also behave like list content in Hugo. As such, they reside in `_index.md` files as well. This means that so that Hugo understands the path to the page, in the case of our author - John Doe - we should add a file at `content/authors/john-doe/_index.md`. As above, this accepts frontmatter configuration and content.

```markdown
---
title: John Doe
---
You can add whatever content you like here, as either markdown or HTML. It will appear above the list of John Doe's content in your HugoModo theme.
```
