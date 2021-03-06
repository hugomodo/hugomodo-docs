---
title: Exif Orientation
date: 2019-01-02 13:05:15 +0000
authors:
- Thom Bruce
---
When you take a picture with your camera or phone, you can do so holding the device at any of four orientations. When using my iPhone for instance, I can take a photo vertically holding my phone with the home button at the bottom, I can take a photo with the home button oriented to the left or the right, I could even take a photo with the home button at the top - my phone upside down.

When one takes a snap on a modern device, the device itself will factor in orientation and store a little snippet of information along with the image in what's called Exif metadata. So when I'm flicking through the gallery on my phone, I tend to see my images oriented correctly. When I transfer them to my computer, the same is true when I'm looking at them in the file system. Both devices know that despite the orientation of my phone at the time of taking the picture, I want to see the image as it was taken in reality. This is the most common expected user experience, and our devices are very good at intuiting how we'd like to see these images.

That said, here's a photo of a duck:

![Image of a duck inserted manually](/uploads/duck_hugo_dev.JPG)

My phone and laptop know to show me this image the right way up, but Hugo doesn't ([yet](https://github.com/gohugoio/hugo/issues/4600)).

The above photo was added directly to my site offline via a text editor. A simple copy and paste from one folder to another. No magic is happening, because there's no room for it to.

Next, we'll try adding the same image via Forestry (HugoModo's benchmark for compatibility):

![Image of a duck uploaded via Forestry](/uploads/IMG_0760.JPG)

The same image again, still displayed with incorrect orientation by default. And it looks as though Forestry currently lacks native image editing or processing capabilities ([although it sounds like they have it in the works, as well](https://twitter.com/forestryio/status/1080244202464788480)).

### We have some options...

Hugo's image processing functionality does already provide the possibility to rotate images, but this must currently be done manually and per image - as noted above, it seems Hugo yet lacks awareness of Exif info. This could be a cumbersome undertaking for an end user with a photo gallery on their site, particularly if they have no development experience.

Hugo is set to gain that information, with a working implementation already. But it looks like we're waiting on an upstream bugfix before it finds its way into a release.

So I will revisit this at that time.

Fortunately, for now, there is another way. Forestry have a partnership with the actually phenomenal image storage service, [Cloudinary](https://cloudinary.com/) who I believe handle this issue by default.

One final test. Here's that duck again:

![Image of a duck uploaded to Cloudinary](https://res.cloudinary.com/thombruce/image/upload/v1546437589/IMG_0760.jpg)

Oops. The orientation of our duck here was corrected in both Forestry and Cloudinary's media dashboards, but is still showing sideways in our content.

So as a final correction, it looks like we still have to adjust that URL. Not ideal. But this is where Cloudinary shines.

Cloudinary takes a parameter for the angle of an image like so:

```
https://res.cloudinary.com/example/a_90/image.jpg
```

The `a_90` tells Cloudinary we want that image rotated 90 degrees, and it will serve us just that.

This isn't what we want in this case, as Cloudinary displays the image corrected by default, at least in the dashboard. So we add `a_0` instead, to tell Cloudinary we want this image served the right way up:

![Image of a duck uploaded to Cloudinary and rotated](https://res.cloudinary.com/thombruce/image/upload/a_0/IMG_0760.jpg)

Hey presto!

Is this ideal? No. It still took some work to get us to this point, where we'd like to just upload our images and have 0 degrees be considered the default orientation.

I will continue to look into this.
