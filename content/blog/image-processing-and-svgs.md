+++
date = "2019-01-03T00:00:00+00:00"
images = ["/uploads/hugo-logo-wide.svg"]
title = "Image Processing and SVGs"

+++
One of the first HugoModo extensions I've written is [HugoModo Image Processing](/extensions/image-processing). This extension does a few things. By default it just provides a pair of shortcodes for Hugo that achieve image processing in content files. This is "the Hugo way", so is the sensible behaviour. But we go beyond that.

A config setting may also be adjusted to have the extension interpret image markdown in the same way as it does shortcodes. Let's call this "the markdown way", because Hugo shortcodes aren't valid markdown but with this setting configured we achieve the same shortcode behaviour with valid content that any other interpreter will display as normal.

That's all fine, until we come to think about image formats and in particular SVGs.

SVGs aren't like other images, especially not to Hugo. Hugo doesn't interpret these graphics files as images and it doesn't have any means of performing processing on them (but they're lossless, so why would it need to?).

HugoModo Image Processing, however, does need to be aware of these files, which might be stored and presented alongside other images in a site's content. If they aren't filtered out before reaching the image processing steps, Hugo will throw an error and our sites won't be built. So, let's solve this problem now!

Here, we have the Hugo logo:

![](/uploads/hugo-logo-wide.svg)

I've added that here, and to the page's metadata for interpretation by the script for Open Graph images in my site's head (these are the images Facebook uses to display a picture when you share a link).

The first thing I notice, adding this in Forestry's editor is... it doesn't display in Forestry's editor. The entity is there, it simply isn't showing in the body of text. I imagine this will be different when I preview the site so I'm going to save this file and move to the next step...

As should be expected, an error:

    execute of template failed: template: partials/image-processing/imgproc.html:13:10: executing "partials/image-processing/imgproc.html" at <.Resize>: can't evaluate field Resize in type resource.Resource

If I turn off my content shims we avoid this, but then we lack image processing and, in fact, all shortcode functionality.

For content files, Hugo provides a handy `.File.Ext` variable for looking at a file's extension. But with our approach to image processing, we are using a `string` to find a Hugo `Resource`. Neither's a content file, so it won't work here. We'll have to examine the string instead for its extension.

To do this, we'll split the string on `.` like so:

    {{ $pathArr := split $src "." }}
    {{ $pathLen := len $pathArr }}
    {{ $ext := index $pathArr (sub $pathLen 1) }}

We already have the `$src` var, it's simply the path to the image. And now before we do anything else with it, we can check whether its extension is equal to `svg` like so:

    {{ if eq $ext "svg" }}

We then act on that accordingly, using a different block than we will for further image processing.

Unfortunately, this hasn't immediately worked. It looks like some pre-existing steps I've taken strip the path to my `uploads` folder from the `src` string. I should refactor a little to improve the approach, but for a quick fix while I still have the `uploads` context kicking around I simply:

    {{ printf "/%s%s" .context.Dir $src }}

`.context` is a variable I've passed to this partial, and represents the resource bundle itself (in my case "uploads"). The `.Dir` variable provides the path to the folder containing that bundle, and we've already discussed `$src`. The rest merely combines those two variables back into the original source path. A hack for now, but a working one.

That handles SVGs in my content files. I now see the above image displaying when I build my site (which means you will too, hurray!).

Next steps are to duplicate the behaviour in other locations its needed, but where the output needs to be a little different. And then that refactor I talked about. I think there's a more ideal way we can achieve this, so I'll have a play around with that.

For now: SVGs and image processing working together! Brilliant.