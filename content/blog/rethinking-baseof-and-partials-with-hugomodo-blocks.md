+++
authors = ["Thom Bruce"]
date = "2019-02-24T00:00:00+00:00"
images = []
title = "Rethinking baseof and Partials with HugoModo Blocks"

+++
So, I think I mentioned this idea of "blocks" in my previous blog post but just in case, here's a primer:

Blocks would differ from regular content files (which take a single HTML content field) by allowing numerous HTML content areas that can be reordered, and that can be any one of several block types either native to HugoModo or custom in your own theme or site. Block types like "photo gallery" and "carousel". Using these, you could then build a page with numerous, highly stylised features instead of having to write heaps of custom HTML for the same result.

Blocks are an idea at this stage, but HugoModo already has some similar features, with the footer in particular already supporting footer modules.

All of that said, there is something that gets in the way of creating a totally unique page from scratch.

## Hugo's baseof Layout

The Hugo `baseof.html` file sits in the `_default` directory of layouts, and acts as the base for every other page.

At present, HugoModo's baseof file looks like this:

``` go-html-template
<!DOCTYPE html>
<html lang="{{ .Site.Language.Lang }}" itemscope itemtype="http://schema.org/WebPage">
  {{- partial "head.html" . -}}
  <body id="hugoModule-body">
    <div id="hugoModule-bodyWrapper">
      {{- partial "header.html" . -}}

      {{- block "main" . }}{{- end }}

      {{- partial "footer.html" . -}}

      {{- partial "foot.html" . -}}
    </div>
  </body>
</html>
```

Standard stuff there. You have the DOCTYPE declaration, then the typical HTML markup including `head` as a partial and the `body` comprised of a header, footer and main section. The `foot` mainly adds additional JavaScript for some features; it's not strictly layout, nor is it getting in the way of what we'd like to achieve.

It's the `header` and `footer` partials. They're the problem. Those partials contain the main title and navigation components that, yes, typically would feature on every page of a website... but that's an assumption. And actually both partials are somewhat opinionated, which limits HugoModo's utility for sites that want to stray even just slightly from the norm.

We do, yes, still want to provide a default that sticks with the norm and is highly opinionated for the benefit of those users who want a quick start. But we do too want to allow far more flexibility.

Preferably, then, header and footer would be moved into the _main_ layouts, and our baseof would look a little more like this:

``` go-html-template
<!DOCTYPE html>
<html>
  {{- partial "head.html" . -}}
  <body>
    {{- block "main" . }}{{- end }}

    {{- partial "foot.html" . -}}
  </body>
</html>
```

Then, individual layouts could specify whether or not to include the header and footer - useful for designing big splash pages.

## An Alternative Approach

Okay, so call the above _approach one_. There is another way... I think. [Hugo's base template lookup order](https://gohugo.io/templates/base/#base-template-lookup-order "Hugo's Base Template Lookup Order") searches first by `<TYPE>` then by `<SECTION>` for a base template...

With _approach one_, above, layout would be definable by means of a `layout` attribute in a page's frontmatter. Sections and types are different things...

Sections I know well enough are defined by a content file's location in the directory structure. A page at `content/blog/my-first-post.md` for example would be considered as being in the _blog_ section.

Type... I think I'm less familiar with. Reading [the docs](https://gohugo.io/content-management/types/ "Hugo's Content Types") it looks like by default, _section_ and _type_ are one and the same, but `type` is provided as a means to override this, meaning that a content file in the `blog` directory could be given the type `blogpost` instead, or even `photograph` or `academicpost`. Certainly handy for altering the output of individual pages... maybe also how they present on list pages, but will have to look into that.

So I could conceivably present alternative `baseof` files for a `blocks` type and a `blank` type and a `default` type, rendering HugoModo layouts selectable by this attribute.

But is it right?

`layout` or `type`... those are my options, where layout is _approach one_ and type we'll call _approach two_.

## In Favour of Approach One

I think this best represents, semantically, the behaviour we're aiming to achieve.

The trouble is... we don't override baseof, which would mean that everything we put in the main block would render. This matters if and when additional section or type baseofs are added in a manner like _approach two_. It's a problem, but...

We can make use of Hugo blocks other than _main_. The docs give an example where [both _main_ and _footer_ blocks are defined](https://gohugo.io/templates/base/#define-the-base-template "Hugo Blocks Usage").

I don't think there's any major limiting factor here.

## In Favour of Approach Two

With _approach two_, we'd actually override the `baseof` file, which is beneficial as it would mean a default `baseof` file could remain in tact which presents an opinionated layout.

As a second benefit, by targeting the `type` attribute, we'd also have layouts that are overwritable per section. The lookup order goes a little something like this: **First**, _does a baseof exist for this section and type;_ **Second**, _does a baseof exist for this type;_ **Third**, _does a baseof exist for this section_. Which means that an end use could override the _type-based_ layout completely, or for a certain section, and they could define their own for other sections.

**But there's one major drawback...** Because `type` is based on the `section` name, there's a chance that an end user would wind up using a custom type unexpectedly, as they create content in a certain directory or _section_. That's a big kicker...

## The Decision: Approach One

Because of the major drawback to approach two, even though it did have more going for it on the whole, it looks like _Approach One_ is the way to go. That's great, I preferred it semantically anyway... although I do wish it drilled a little deeper like _approach two_ does.

So a tentative example of a new default `baseof` would look like this:

``` go-html-template
<!DOCTYPE html>
<html>
  {{- partial "head.html" . -}}
  <body>
    {{- block "header" . }}{{- end }}

    {{- block "main" . }}{{- end }}

    {{- block "footer" . }}{{- end }}

    {{- partial "foot.html" . -}}
  </body>
</html>
```

The `header` and `footer` partials are replaced with blocks, to be defined in the layout. The layout would then provide the definitions for these blocks. And the definitions could be absent or black, in which case Hugo would fallback to the contents of the block as defined in `baseof` (empty in the above example).

To my immediate knowledge, this won't present any conflicts as _approach two_ inevitably would. And it offers us total control over page layouts as desired.

I'm going to start working on this this afternoon.
