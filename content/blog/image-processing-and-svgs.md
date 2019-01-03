+++
date = "2019-01-03T00:00:00+00:00"
images = ["/uploads/hugo-logo-wide.svg"]
title = "Image Processing and SVGs"

+++
One of the first HugoModo extensions I've written is [HugoModo Image Processing](/extensions/image-processing). This extension does a few things. By default it just provides a pair of shortcodes for Hugo that achieve image processing in content files. This is "the Hugo way", so is the sensible behaviour. But we go beyond that.

A config setting may also be adjusted to have the extension interpret image markdown in the same way as it does shortcodes. Let's call this "the markdown way", because Hugo shortcodes aren't valid markdown but with this setting configured we achieve the same shortcode behaviour with valid content that any other interpreter will display as normal.

No problems yet. But this brings us to the third feature, one I've been working on tonight. An additional config setting can be set to achieve the same image processing available to content files in the site's head. At present, it just provides a default resizing behaviour for the Open Graph protocol (which is what Facebook uses to display images with links).

Hugo already provides a template for including Open Graph data in the document head. The only adjustment I've made is to provide image processing.

That's all fine, until we come to think about image formats and in particular SVGs.

SVGs aren't like other images, especially not to Hugo. Hugo doesn't interpret these graphics files as images and it doesn't have any means of performing processing on them (but they're lossless, so why would it need to?).

HugoModo Image Processing, however, does need to be aware of these files, which might be stored and presented alongside other images in a site's content. If they aren't filtered out before reaching the image processing steps, Hugo will throw an error and our sites won't be built. So, let's solve this problem now!

Here, we have the Hugo logo:

![](/uploads/hugo-logo-wide.svg)

I've added that here, and to the page's metadata for interpretation by the script for Open Graph images in my site's head.

The first thing I notice, adding this in Forestry's editor is... it doesn't display in Forestry's editor. The entity is there, it simply isn't showing in the body of text. I imagine this will be different when I preview the site so I'm going to save this file and move to the next step...

As should be expected, an error:

    execute of template failed: template: partials/image-processing/imgproc.html:13:10: executing "partials/image-processing/imgproc.html" at <.Resize>: can't evaluate field Resize in type resource.Resource

If I turn off my content shims we avoid this, but then we lack image processing and, in fact, all shortcode functionality.