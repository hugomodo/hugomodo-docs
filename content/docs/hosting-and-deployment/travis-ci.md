---
title: "Travis CI"
date: 2019-02-02T22:06:34Z
draft: true
description: Deploy HugoModo with Travis CI.
---
## Minimalist

```yaml
dist: xenial

addons:
  snaps:
  - hugo

script:
- hugo
```

```yaml
dist: xenial

addons:
  snaps:
  - name: hugo
    channel: extended

script:
- hugo
```

### Cons

#### No Asset Pipeline???

## Fastest

Include the Hugo binary.

```yaml
language: minimal

script:
- bin/hugo
```

### Cons

#### Need to Include a Binary

#### No Asset Pipeline???

## Extended

## GitHub Pages

### User/Organization Site

### Project Site
