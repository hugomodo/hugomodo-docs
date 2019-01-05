---
title: "Shortcode Partials"
date: 2018-12-31T20:08:37Z
draft: false
---
### What's the issue?

HugoModo strives for third-party compatibility. The baseline for this is currently Forestry CMS. When asking the question, "does it work with Forestry CMS?" of the [Image Processing](/extensions/image-processing) extension, the answer was not yes - it was *mostly*.

The problem is, Forestry is unaware of shortcodes. These could be hand-typed by a developer easily, but when it comes to non-technical authors adding images to content, they probably aren't going to do that. And as a matter of usability, I also prefer to use the GUI to add content to a post. On Forestry, this results in a markdown declaration.

No assessment of the size of the image is performed, and so the result is the raw output of - *more than likely* - an oversized image.

HugoModo Image Processing aims to circumvent that issue with an interpreter of its own for outputting content. But while this interpreter then succeeds in producing resized image resources, it breaks all of the other shortcodes that can be used in Hugo.

### The solution

HugoModo Shortcode Partials aims to provide all of the existing Hugo shortcodes as partials, as well as an override for content interpretation. This solves the problem above, and provides a clear best practise for adding additional shortcodes.

### This is not recommended

Where absolutely possible, it is better to do without overriding Hugo's own `.Content` function. Using this shim and/or the Image Processing override may lead to a severe decrease in performance.

It is provided as a convenience, for anywhere markdown reinterpretation is preferred.
