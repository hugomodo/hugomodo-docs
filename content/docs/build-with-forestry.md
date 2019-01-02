+++
date = "2019-01-02T00:00:00+00:00"
images = []
title = "Build with Forestry"

+++
Forestry has some configuration settings that will allow automatic builds whenever content is updated via their dashboard.

I've just set this up and am publishing this post to check it out.

As Forestry remains our benchmark for compatibility, it will always be important that builds are successful from this source.

Unfortunately, it appears that Forestry can only handle project-based GitHub Pages, rather than user or organisation ones - which is what we want for HugoModo.

I will revisit this and document the approach for project-based sites at another time, but [Forestry have a great guide](https://forestry.io/docs/hosting/github-pages/) themselves should you need it.

For the time being, where it comes to HugoModo, I'm going to see if deployment via a CI service such as TravisCI is feasible.