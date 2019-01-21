+++
authors = ["Thom Bruce"]
date = "2019-01-21T00:00:00+00:00"
draft = true
images = []
title = "Hugo's Table of Contents"

+++
If you've read my earlier blog entries, you'll know that I don't intend for HugoModo to supplant Hugo. I call it a 'framework' but it's really just a collection of themes. The aim is not to require a radically different, custom setup, but to work by-default with any Hugo site. And a part of achieving that is to do things the Hugo way, which means if a feature exists natively in Hugo I ought to utilise it, rather than provide an alternative that fragments HugoModo from the Hugo foundation it's built on.

So... Hugo has this [Table of Contents feature built in](https://gohugo.io/content-management/toc/ "Hugo Table of Contents"). Beautiful! If you have headers in your content markup, it _just works_. It creates a list from those headers, wraps that list in links and a `ul` element, and you're done - instant table of contents.

That would be fine if I weren't also intent on making HugoModo semantically correct and valid HTML. Y'see... I've taken a bit of a workaround in my content files. To explain the situation, we need to talk about headers and we should begin with the site layout files.

### Site and Article Titles

Every page on HugoModo features two titles by default: the title of the site and the title of the current page. Stripped of all other markup, they look like this:

```html
<h1>Site Title</h1>
<h2>Article Title</h2>
```

Because `h#` elements are more than just stylistic, they're a semantic element parsed with meaning by search engines, there are some best practises as concerns their levels and nesting them. So given that my article titles use `h2`, then if I'm using any additional headers in my page content I will tend to start at `h3`.

```html
<h1>Site Title</h1>
<h2>Article Title</h2>
<h3>Site and Article Titles</h3>
<p>Every page on HugoModo features two titles by default...</p>
```

With this approach, the page remains nice and semantic and easily parsed by web crawlers for search engine indexing.

### The Table of Contents

To include a table of contents in a layout in Hugo, one simply adds this:

```html
{{ .TableOfContents }}
```

Quick, simple, automatic. And as described above it will wrap found headers in a list and anchor links, a bit like so:

```html
<ul>
  <li>
    <a href="#site-title">Site Title</a>
    <ul>
      <li>
        <a href="#article-title">Article Title</a>
        <ul>
          <li>
            <a href="#site-and-article-titles">Site and Article Titles</a>
          </li>
          <li>
            <a href="#the-table-of-contents">The Table of Contents</a>
          </li>
        </ul>
      </li>
    </ul>
  </li>
```

That's all automatic: nesting levels, anchor links, all of it. And in the example above, you can see no nesting level has been skipped...

...but I've cheated there. 'Site Title' and 'Article Title' are not in the content. Those are stored and rendered separately as properties of the page. The first title in the content is actually 'Site and Article Titles', and it's a `h3`. The actual output for this page at present would be:

```html
<ul>
  <li>
    <ul>
      <li>
        <ul>
          <li>
            <a href="#site-and-article-titles">Site and Article Titles</a>
          </li>
          <li>
            <a href="#the-table-of-contents">The Table of Contents</a>
          </li>
        </ul>
      </li>
    </ul>
  </li>
```

The table of contents nests our list **two whole levels** before we get to our `h3` titles. The result is... ugly.