---
title: "Image Processing"
date: 2018-12-30T16:54:08Z
draft: false
description: Adds Hugo's image processing to HugoModo for Page Bundles.
---
## Description

### Features

- Provides `imgproc` and `figproc` shortcodes for image processing in your content files.
- Uses `srcset` to load responsive image sizes on different screen resolutions.
- Can be configured to use Page Bundles, a headless Resources bundle, or both!

#### TODO

- Equivalents for theme layouts are provided: `image-processing/imgproc.html` and `image-processing/figproc.html`
  1. These already exist. Modification required so that context can be passed.

## Installation

## Configuration

HugoModo Image Processing can be configured one of two ways. The default is to use Hugo's Page Bundles, but an alternative approach using an uploads directory that provides compatibility with third party Content Management Systems can be setup with minimal configuration.

### Page Bundles

By default, HugoModo Image Processing uses Page Bundles as described by the Hugo docs: [Page Bundles](https://gohugo.io/content-management/organization/#page-bundles)

No additional configuration is required to use HugoModo Image Processing this way.

### Uploads Directory

You can configure HugoModo Image Processing to use an uploads directory instead, for compatibility with some Content Management Systems such as Forestry who write up the idea behind the uploads directory here: [How To Use Hugo's Image Processing With Forestry](https://forestry.io/blog/how-to-use-hugo-s-image-processing-with-forestry/)

The HugoModo implementation is a little bit different, but fully compatible with Forestry at the time of writing.

To set it up add a file to your site at `data/imageProcessing/config.toml` with the following configuration:

``` toml
uploadsDir = "uploads"
```

...and that's it! You can now add your images to a `content/uploads` directory and have HugoModo's image processing shortcodes will look for your resources there instead.

### ...or both!

It is also possible to mix the two approaches by passing a `context` attribute to the provided shortcodes and templates:

``` go-html-template
{{</* imgproc context="uploads" src="fabian-grohs-423591-unsplash.jpg" */>}}
```

## Shortcodes

### imgproc

{{< imgproc context="extensions/image-processing" src="fabian-grohs-423591-unsplash.jpg" >}}

### figproc

{{< figproc context="extensions/image-processing" src="fabian-grohs-423591-unsplash.jpg" caption="Computer on a desk" >}}
