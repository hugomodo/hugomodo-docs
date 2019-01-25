+++
authors = ["Thom Bruce"]
date = "2019-01-25T00:00:00+00:00"
draft = true
images = []
title = "A Super-Powered Default Menu"

+++
I put off redoing the menu setup for HugoModo for a little while. Hugo's menu system, I remembered, was a challenge I'd previously overcome. The required changes were going to take, I dunno, anywhere from a few hours to a day. But I was wrong. Hugo's menu system is actually very simple, and I was done in a matter of minutes.

The reason I thought it was going to be difficult is because I'd previously made it difficult for myself, but the result of that was a powerful default menu behaviour that went leagues beyond Hugo's "Section Menu for Lazy Bloggers". Hugo recommends that setup as a starting point, so we'll start there too.

## Hugo's Section Menu for Lazy Bloggers

Hugo's [Section Menu for Lazy Bloggers](https://gohugo.io/templates/menu-templates/#section-menu-for-lazy-bloggers "Hugo's Default Menu") lists all site sections that have content. It's a really quick and simple way to add site-wide navigation to a Hugo site.

Here's how I've implemented it in HugoModo:

```html
  <nav>
    <ul>
      {{ $currentPage := . }}
      {{ range .Site.Menus.main }}
        <li>
          <a class="sidebar-nav-item{{if or ($currentPage.IsMenuCurrent "main" .) ($currentPage.HasMenuCurrent "main" .) }} active{{end}}" href="{{ .URL }}" title="{{ .Title }}">{{ .Name }}</a>
        </li>
      {{ end }}
    </ul>
  </nav>
```

As of today, HugoModo supports this menu with minimal setup. To activate this menu, all that's needed is this line in your site's `config.toml` file:

```toml
sectionPagesMenu = "main"
```

That's it, you're all set!

## Adding Menu Items

With the above configured, adding menu items works just as it would with any other Hugo site.

You can add menu items to your site config:

```toml
[menu]
  [[menu.main]]
    name = "about"
    url = "/about/"
```

Or you can add them to your content frontmatter:

```yaml
---
menu: "main"
---
```

Instead of the lazy sections menu, the menu will now be populated by the items specified in either location (or indeed both).

Consult Hugo's own [documentation](https://gohugo.io/content-management/menus/ "Hugo's Menu Documentation") for further possibilities.

## HugoModo's Super-Powered Default Menu

Now, to the reason I had assumed I was in for a challenge with menus! I did this some time back when I wasn't as well-versed in Hugo's architecture as I am now., so at the time it took a lot of effort. And yet, it now looks so deceptively simple:

```html
  <nav>
    <ul>
      {{ $sections := .Site.Sections }}
      {{ $pages := where .Site.RegularPages "Section" "" }}
      {{ range (union $sections $pages) }}
        {{ if not (eq .Name .Site.Title) }}
          <li>
            <a href="{{ .Permalink }}" title="{{ .Title }}">{{ .Name }}</a>
          </li>
        {{ end }}
      {{ end }}
    </ul>
  </nav>
```

You'll see there the standard markup for an unordered HTML menu, the elements used: `<nav>`, `<ul>`, `<li>` and `<a>`.

You'll notice my menu also includes a site's sections, via the `$sections` variable. But it also brings in some of a site's pages. Here's the all-important line:

```html
{{ $pages := where .Site.RegularPages "Section" "" }}
```

The `where` query here finds only those pages whose `Section` is blank. This is effectively asking, "is it in the root folder?"

The resultant set of sections and pages is the collection of those at the root of the Hugo content folder.

    content/
      blog/							<- Appears in menu
        my-first-blog-post.md
      career/						<- Appears in menu
        experience/
          my-first-job.md
        projects/
          my-project.md
        _index.md
      about.md						<- Appears in menu
      contact.md					<- Appears in menu

Contrast to Hugo's lazy menu:

    content/
      blog/							<- Appears in menu
        my-first-blog-post.md
      career/
        experience/					<- Appears in menu
          my-first-job.md
        projects/					<- Appears in menu
          my-project.md
        _index.md
      about.md
      contact.md