---
title: "Fusejs"
date: 2019-01-03T03:09:44Z
draft: false
---

### Installation

Follow the usual steps for installing a HugoModo extension, then:

* Add a JSON output for `home` in your site config:

``` toml
[outputs]
  home = [
    "HTML",
    "RSS",
    "JSON"
  ]
```

* Add a content file at `content/search.md` with frontmatter:

``` yaml
---
title: Search
layout: search
---
```

* That's it! Were you expecting extra steps? Load up your site and visit `/search` to see it working.

### Roadmap

1. Searchbar partial for inclusion in templates.
