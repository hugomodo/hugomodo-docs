+++
date = "2019-01-07T00:00:00+00:00"
draft = true
images = []
title = "Hugo's Author and Authors"

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