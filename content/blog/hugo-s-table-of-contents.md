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

The table of contents nests our list **two whole levels** before we get to our `h3` titles. The result is... ugly:

![Ugly unordered list nesting with empty list items](/uploads/Screenshot 2019-01-21 at 14.02.33.png "Hugo's Table of Contents Ugly Nesting")

As you can see, we get two additional and unnecessary levels of nesting by default.

### A Semantically Dirty Solution

The quickest way to resolve this is to do what actually I imagine end users will wind up doing, particularly non-technical users. That is to just start content titles at `h1` and suffer the invalid HTML warnings, and perhaps also the small hit to SEO.

```html
<h1>Site Title</h1>
<h2>Article Title</h2>
<h1>Site and Article Titles</h1>
```

That will clean up our table of contents, but I don't consider it an option. HugoModo should support semantically correct HTML by default, and it shouldn't require an SEO hit just to provide cleaner presentation.

There's got to be a better way.

### A Better h1

Okay, so maybe the problem is a little with my layout. We should take the opportunity to think a little about what an `h1` should be. As search engines are going to consider this the most important header in indexing a page, it clearly makes sense to give priority to the page title - at present, I'm favouring the site title.

The actual SEO improvements for this will be negligible if they exist at all. And we'll have to restyle our site title so it still appears like a leading header on the page, even though semantically it no longer is. Bootstrap has [display headings](https://getbootstrap.com/docs/4.2/content/typography/#display-headings "Bootstrap Display Headings") for just this purpose, so I'll follow their lead and use `display-1` in place of `h1` in my example below:

```html
<strong class="display-1">Site Title</strong>
<h1>Article Title</h1>
<h2>Site and Article Titles</h2>
```

Okay, a decision to explain there. I've use `strong` for the site title because this semantically suggests that the enclosed text is **important**. In design, we might often use it just to make text bold... but this isn't actually its intended use. It does that, yes, but we can restyle it any way we choose to remove the **bold** properties. We should - and this is what I've done here - use it to mark important text, regardless of styling. (This choice may change when I come to do my [accessibility overhaul](https://waffle.io/hugomodo/hugomodo/cards/5c448c725f102403a99235a8 "Waffle.io Accessibility Overhaul Project Management Card"), but for now it stands as reasonable.)

Next, I've used `h1` for the article title. Feels like the best choice as regards SEO and properly indexing the page. Probably best for accessibility too.

But... a problem. My content titles now start at `h2`. One fewer levels of nesting, yes, but still we haven't entirely resolved that table of contents issue.

So let's look at what we have to support.

### Two Kinds of People

Let's assume that developers, designers and technical users like myself will follow best practices and start their content titles at `h2`:

```html
<h1>Article Title</h1>
<h2>Site and Article Titles</h2>
```

Those are our first kind of person.

Our second kind are end users, non-technical website owners for whom the first kind of person have built a site using HugoModo. They are likely to give less consideration to header levels, and they will wind up breaking HTML validity, possibly accessibility and SEO this way, but we can't save everybody, right? They're going to start with `h1` and nest downwards from there.

```html
<h1>Article Title</h1>
<h1>Site and Article Titles</h1>
```

Invalid, but they actually get the benefit of a prettier table of contents before I've even fixed the styling in a step to come.

### h1 is like h2 or h1.5

Quick note: Regardless of the theme or framework I'm using, the use of `h1` is still going to result in **BIG STYLING** of the text. A little quick fix for that (not tested) might be:

```sass
h1:not(:first-of-type) {
  @extend h2;
}
```

...maybe. Just thinking out loud here. This will (or should) at least get any `h1` but the first on the page to adopt the styling rules of a `h2` instead.

But actually... We don't want it to be identical to an actual `h2`. We do still want to respect those users' nesting decisions. So probably better, rather than `@extend h2` to do something more like `font-size:2.25em;` or whatever styling choices we have to make to have the result stylistically appear to be a "h1.5", with size and weight somewhere between the main `h1` and an `h2`.