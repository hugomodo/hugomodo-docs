---
title: "The Trouble With Chroma, Hugo's Syntax Highlighting Library"
date: 2018-12-31T22:57:07Z
draft: false
---
One of the core principles of HugoModo is to stick to the Hugo way of doing things where possible and sensible to do so. This reduces the likelihood of incompatibility, and should make for greater ease of use for anyone familiar with Hugo or working from the Hugo documentation.

Of course, HugoModo deliberately does a lot differently, emphasising the principle of *Convention over Configuration* where Hugo otherwise emphasises speed. The aim is to achieve a comfortable middle-ground that remains blazing fast, but that also provides for greater ease of use and extension.

I just don't yet know what to do about Chroma.

Chroma is Hugo's in-built syntax highlighter, for rendering beautiful code samples like this one:

``` go-html-template
{{ if .IsPage }}
  {{ .Content }}
{{ end }}
```

At the time of writing, this code sample is being interpreted by Chroma and displayed with inline style declarations.

Chroma has a couple of different flavours though. The default is these inline styles, but Hugo also provides a configuration option and CSS generator to serve Chroma syntax highlighting with classes used and styles served from a separate stylesheet.

Thing is, neither of these approaches is really the HugoModo way.

Another principle of HugoModo is HTML minimalism. By this I mean that the markup is intended to be clear of style declarations and class attributes. Unless strictly necessary, I believe that HTML should be free from both, with a CSS file doing the heavy lifting to style a page. If necessary later, an additional script should be invoked to inline above-the-fold styles, for instance. The point is separation of scope in development: HTML files should be strictly markup, CSS should have almost total authority when it comes to styling.

HugoModo will continue to support Chroma for now. But I am beginning to wonder... is it the HugoModo way?
