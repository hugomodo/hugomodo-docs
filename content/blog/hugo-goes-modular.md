---
title: "Hugo Goes Modular"
date: 2018-12-29T01:11:18Z
draft: true
---

## A Better Base

HugoModo removes the main content div from Hugo's default base template, and adds a new partial called the 'foot'. The aim is to follow certain best practices. We want to keep markup to a minimum, and where possible to only use HTML5's semantic elements ('main', 'section', etc. rather than 'div'). And the foot partial is provided mainly to load scripts after the main content, so as to provide for fast page rendering to visitors.

### The Hugo Way

``` go-html-template
<!DOCTYPE html>
<html>
    {{- partial "head.html" . -}}
    <body>
        {{- partial "header.html" . -}}
        <div id="content">
        {{- block "main" . }}{{- end }}
        </div>
        {{- partial "footer.html" . -}}
    </body>
</html>
```

### The HugoModo Way

``` go-html-template
<!DOCTYPE html>
<html>
  {{- partial "head.html" . -}}
  <body>
    {{- partial "header.html" . -}}

    {{- block "main" . }}{{- end }}

    {{- partial "footer.html" . -}}

    {{- partial "foot.html" . -}}
  </body>
</html>
```

## Theme Partial Organisation

Hugo themes are easy to extend and make adjustments to by overwriting partials. Many themes however are comprised of partials that do an enormous amount. In order to change one small detail, the entire partial must be copied across. And if the base theme then receives an update, it can be a chore to find small changes in those files to upgrade your child theme or site.

HugoModo aims to solve this problem by reducing partials to tightly scoped directories. Rather than having one 'head' partial, HugoModo's 'head' calls on many child partials in `partials/head`. Any one can be overwritten without throwing the rest of the theme out of sync with the upstream, meaning easier upgrades and lower maintenance over time.

## List Pages

Because Hugo builds sites that reflect the directory structure of its content files precisely, it is easy to create deeply nested pages.

There is no list page behaviour by default, but the standard practise is to just list `.RegularPages` for each section. However, this is a poor reflection of Hugo's fuller capabilities which allows for sections inside of sections, and deeply nested collections of pages.

HugoModo includes a default list template that combines and sorts a section's child sections and pages, listing and paginating them together.

## Superior Asset Bundling

Hugo hasn't always had great support for CSS and JavaScript assets. It still doesn't have the greatest, but it's moving in the right direction.

HugoModo gives Hugo a gentle nudge slightly further. It uses Hugo's support for data files to provide a single location where JavaScript and SASS resources may be listed. The included js and scss resource partials read these resource lists, concatenate and minify the named files.

It's a lot like Ruby's Bundler, or NPM. Basically a pseudo package manager for Hugo. It's awesome!

And this is where HugoModo gets truly modular. Regardless of how many child themes or extensions you invoke in your site, the resource bundler collects all of the associated assets and bundles them for you.
