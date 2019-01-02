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

And do you know what? It is. There are a lot of blog posts from others online with setups that vary in complexity. None are as simple as this:

``` yaml
# Clean and don't fail
install:
  - rm -rf public || exit 0

# Build the website
script:
  - bin/hugo

deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GITHUB_TOKEN  # Set in the settings page of your repository, as a secure variable
  keep-history: true
  local-dir: public
  repo: hugomodo/hugomodo.github.io
  allow-empty-commit: true
  target-branch: master
  on:
    branch: master
```

This is mostly TravisCI's own suggested configuration for publishing to GitHub Pages, with the exception of adding the `local-dir`, `repo`, `allow-empty-commit` and `target-branch` options. These tell Travis to publish the contents of `public` to our separate GitHub Pages repo at `hugomodo/hugomodo.github.io` and to push those contents to the `master` branch. `allow-empty-commit: true` is added in case of build failure, so that the site can be rebuilt without changes.

The only other required setup is to add a GitHub token to the repo config in TravisCI, and provide Hugo as a binary in the `bin` folder.
