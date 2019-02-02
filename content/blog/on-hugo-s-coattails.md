+++
authors = ["Thom Bruce"]
date = "2019-02-02T00:00:00+00:00"
images = []
title = "On Hugo's Coattails"

+++
## Hugo v0.54.0 is Released

Today is the day I've been waiting for. I've had a keen eye on Hugo's [v0.54 milestone](https://github.com/gohugoio/hugo/milestone/86?closed=1 "Hugo v0.54 Milestone") for a while now. I know that it was delayed, pushed back, and then - surprise! - released ahead of the rescheduled date.

The new version contains fixes for some issues I've banged my head against while developing HugoModo. There's a fix for `getJSON` causing build aborts, which is something I ran into when trying to have the theme and extension pages of this very website load README files from their respective repos - time to revisit that, I guess. There's also the introduction of full and proper semver; the release is v0.54.0 rather than v0.54. A big discussion about that change can be found here: [https://github.com/gohugoio/hugo/issues/5639](https://github.com/gohugoio/hugo/issues/5639 "Hugo Semver Discussion").

But most importantly for HugoModo, [slash handling in taxonomies has been fixed](https://github.com/gohugoio/hugo/issues/5571 "Hugo Restore Slash Handling Discussion").

Had you asked me yesterday what version of Hugo would give you the best experience with HugoModo, I'd have likely said v0.47. This is the version I've been using on my own blog, because I like being able to list taxonomies beneath site sections; a bit like this: `author/genres`. This is a helpful feature when a site has many sections that aren't necessarily related by tags. Over on [my personal website](https://thombruce.com/ "thombruce.com - My Personal Website and Blog"), in fact, I've used the typical `tags` taxonomy as well as a `developer/tags` taxonomy that is strictly for tagging my software projects. There's clear use for the distinction in keeping distinct sections organised and clean of other sections' descriptors. Like [JLKM](https://github.com/JLKM "JLKM's GitHub Profile") who opened the issue on Hugo, I would be inclined to agree taxonomies are one of the things that makes Hugo stand out from other static site generators.

I'm so happy they're back! And now I can recommend the very latest version of Hugo for use with HugoModo: version 0.54.0.

## HugoModo v0.1 is Released

In other news, I've started to introduce versioning to the HugoModo project with the release of [version 0.1](https://github.com/hugomodo/hugomodo/releases/tag/v0.1 "HugoModo Version 0.1 Release").

Semver doesn't matter as much to a project like HugoModo, and I missed the discussion about semver above before putting out this release... so I'm undecided as to whether I'll adopt full and proper semver with future releases. It doesn't matter, because HugoModo is not a module and so it isn't something that will be pulled into other software projects. It is still _just a theme_ for Hugo. But I expect I'll adopt the practise, just to better mirror Hugo releases.

Speaking of which, HugoModo will continue to ride Hugo's coattails, so to speak, up to the first major release of Hugo v1.0.0. It doesn't seem right to me that HugoModo jump ahead and declare itself a major, stable release when Hugo - which it of course depends so greatly on - has not.

But why use semantic versioning at all? HugoModo is _just a theme!_

With the release of v0.1, I've also setup some automation to handle release branching. A couple of things will happen when I tag a release:

1. GitHub will create a ZIP file and tarball of the repo at that point in time
2. Travis CI will create a new branch based on the version number and push it to GitHub

This means two additional ways to install a stable version of HugoModo. One, you can download either of the compressed files. Or two, you can use Git submodules and reference a versioned branch. _I think that will work, I haven't yet tested it_.

That second approach (assuming it works) will become the recommended way to install HugoModo. It will provide a stable, unchanging version of the project that you can update whenever you choose simply by altering the version number in your `.gitmodules` file and running an update.

I've described this as an effort to simulate package management for Hugo websites, and that is a pretty close approximation of package management.

And that's the news! Big day, big week for HugoModo. Small changes with big effects; the project is now very close to its goal of being easy to install, easy to update and stable.