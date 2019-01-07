+++
date = "2019-01-07T00:00:00+00:00"
images = []
title = "Hugo's Author and Authors"
authors = [
  "Thom Bruce"
]
+++
HugoModo aims, best as it can, to do things the Hugo way. This means not superseding any functionality, and not providing a wildly different setup. Essentially, one should be able to build a Hugo site by following Hugo's own docs, and that site should be compatible with HugoModo's themes and extensions. And so far, I believe that's true.

I characterise HugoModo as a framework built around Hugo that provides an approach to managing functionality in a way that I think a lot of developers will find more approachable. HugoModo emulates package management, which is something that may always be lacking from Hugo simply because of the nature of the Go programming language... _I think._ (Disclaimer: I don't know enough about this problem).

So I'm quite happy to "extend" Hugo with my [Image Processing](/extensions/image-processing) extension, which activates a feature already present in Hugo and follows the established markup of Hugo's own internal templates.

It's a convenience to then be able to include that among the themes for my Hugo sites and just have it work!

But tonight I am faced with a problem.

I believe that a common need of blogging websites is an authors list, with author pages displaying their articles. And at the moment, this is only [proposed Hugo functionality](https://github.com/gohugoio/hugo/issues/3088).

At present, Hugo supports a single author by default in a site's config:

    Author:
      givenName: John
      familyName: Doe
      displayName: John Doe

It does extend a `map` of authors for a site via `.Site.Authors`, but I am unsure of how or even if this can be populated at present. Which means... we'll have to deviate from the path a little and rejoin Hugo along the way, when the Authors model is in a more complete state.

There exist many write-ups online for how to add authors to Hugo, and they differ greatly. We're going to do [the Netlify way](https://www.netlify.com/blog/2018/07/24/hugo-tips-how-to-create-author-pages/), because it provides the discussed functionality with minimal configuration. And because Netlify are big proponents of the JAMstack and I dig that! ❤️

### The Implementation

For single-author sites we're already set, as shown above. My intention then shall be to ensure that this continues to work either as a fallback or for sites that simply don't need multiple authors. This will be easy enough to achieve. If a content page lists no authors, we simply use the fallback information. We will lack the content list page for the author this way, but if they are the sole author on the site this won't matter: every list page is de facto a list of their content.

And we will make the assumption that our new author pages will provide all of the same information as the `Author` config permits.

The first steps to activating an authors list for a site are ones we cannot take in a theme or extension (well, we could but it's best we don't). 'Authors' needs to be configured as a taxonomy:

    taxonomies:
      category: categories
      series: series
      tag: tags
      author: authors

When you add a taxonomy in Hugo, you're actually overwriting the hash of taxonomies Hugo assumes active by default. These are categories, series and tags. You can safely leave them out if you don't want them.

With that done, we can now add authors in our content files as frontmatter:

    title: Some Piece of Content
    authors:
    - John Doe

That's it. When we now build our site, an author page will exist for listed author and all of his/her associated content will be shown there. In fact, HugoModo's themes already output the author tag with each post. No further work needed.

In two really easy steps, we now have author pages. But they don't tell us very much. They merely list an author's name and content.

For customisable author pages, we'll have to add a section for our `authors` taxonomy as well as a page for each author. The resultant directory structure should look something like this:

    content
    - authors
      - _index.md
      - john-doe
        - _index.md

Both `authors` and `john-doe` here are what Hugo defines as `Branch Bundles`, designated by the presence of a `_index.md` file (don't forget the underscore). This is important, as this will lead Hugo to interpret the author page as a collection, rather than as a single page. And this will enable Hugo to list the author's content.

### Next Steps

If authors were _just_ another taxonomy, that would be the end of it. But they aren't.

With the above setup, Hugo will now generate an index page listing the site's authors, and a page for each author.

Theme developers might like to know how to override these, and I personally find the Hugo docs perplexing on this subject. Hopefully this is a little clearer:

To override the authors index page, create a layout at:

    /layout/authors/terms.html

To override the individual author page:

    /layout/authors/taxonomy.html

Hugo will fallback to using the default layout for either, or a default list layout if present.

### As for HugoModo...

It's getting late now, so I'm going to finish this tomorrow.

I'll be adding a default authors taxonomy layout to the HugoModo base theme, that will check for the author params as provided by Hugo for single site authors. And I'll be adding a fallback if these aren't found to behave just as other taxonomies would.

That should provide a convention that I think Hugo will eventually settle on. When multi-author support is implemented, I will update HugoModo to behave accordingly. But with this setup, I don't think very much will have to change at all.
