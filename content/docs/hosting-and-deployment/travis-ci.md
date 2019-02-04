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

Is the above faster than below? Almost certainly.

```yaml
dist: xenial

addons:
  snaps:
  - name: hugo
    channel: extended

script:
- hugo
```

This is now my preferred approach.

### Cons

#### No Asset Pipeline???

## Fastest

Include the Hugo binary.

```yaml
language: minimal

script:
- bin/hugo
```

This appears to cut my build times on Travis CI in half... probably because we only fetch resources from one source: the repo.

Can't seem to get the extended binary to execute for Linux this way, so I opt to use MacOS instead:

```yaml
os: osx
```

### Cons

#### Need to Include a Binary

#### No Asset Pipeline???

## Extended

## GitHub Pages

### User/Organization Site

### Project Site
