+++
date = "2018-12-31T00:00:00+00:00"
images = ["/uploads/thomas-le-578994-unsplash.jpg"]
title = "Forestry Compatible"
authors = [
 "Thom Bruce"
]
+++
HugoModo is designed to be compatible with the static site, headless CMS, [Forestry](https://forestry.io/ "Forestry.io CMS").

When you first hook a site up with Forestry, it assumes the same default behaviour that HugoModo does. In particular, this means images are stored in the static directory and are exempt from image processing.

But Forestry has offered configuration for using a content directory instead for quite a while now. Enabling this feature will store uploads as page resources, and will allow for the use of Hugo's in-built image processing.

This can be activated in HugoModo by installing the HugoModo Image Processing extension.

By default, HugoModo Image Processing will assume the use of page bundles to store images. This is the de facto Hugo way, but is currently unsupported by Forestry. But just one extra step gets us to where we need to be.

HugoModo Image Processing can be configured to look for and use any content directory for image resources.

To turn this on, simply add a file at `data/imageProcessing/config.toml` with the content:

``` toml
uploadsDir = "uploads"
```

This can be any path you like, but the folder should exist in your site's content directory, and should contain an `index.md` set to act as a headless bundle.

The following image has been uploaded in Forestry's dashboard:

![Image of a mic uploaded via Forestry](/uploads/thomas-le-578994-unsplash.jpg)

Unfortunately, inline images cannot be added via the Forestry dashboard and handled automatically by HugoModo's image processing shortcodes.

This is something I am looking to remedy. Watch this space.
