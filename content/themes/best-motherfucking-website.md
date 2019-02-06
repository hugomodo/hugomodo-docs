---
title: "Best Motherfucking Website"
date: 2018-12-29T14:39:14Z
draft: false
description: A highly readable theme for HugoModo with minimal custom CSS and JavaScript.
repo: hugomodo/hugomodo-best-motherfucking-website
tags:
- minimalist
---
# HugoModo Best Motherfucking Website

Based on [The Best Motherfucking Website](https://thebestmotherfucking.website), this theme provides minimal style definitions to create a clean, highly readable theme to be used as the base for customisation or for websites with a minimalist feel.

## Installation

### Simple

The recommended way to install and use the _HugoModo Best Motherfucking Website_ theme is to install the latest standalone version as a Git Submodule using the `-b 0.1.0-standalone` flag:

```text
git submodule add -b 0.1.0-standalone https://github.com/hugomodo/hugomodo-best-motherfucking-website themes/hugomodo-best-motherfucking-website
```

```toml
themes = ["hugomodo-best-motherfucking-website"]
```

### Advanced Options

#### Standalone

If you're happy targeting an edge branch, you can drop the version number when installing. Use the `-b standalone` flag:

```text
git submodule add -b standalone https://github.com/hugomodo/hugomodo-best-motherfucking-website themes/hugomodo-best-motherfucking-website
```

#### Original

For the most advanced usage that will let you update the _HugoModo_ core independently of the theme, install both independently using a versioned branch flag:

```text
git submodule add -b 0.1.1 https://github.com/hugomodo/hugomodo themes/hugomodo
git submodule add -b 0.1.0 https://github.com/hugomodo/hugomodo-best-motherfucking-website themes/hugomodo-best-motherfucking-website
```

_Note that you can also use `-b master` or forego the flag entirely to get the very latest edge version of each._

## Updating

With _HugoModo Best Motherfucking Website_ installed as a Git submodule, it's easy to update to the latest version of the branch you're on. From your site's root directory, simply run:

```text
git -C themes/hugomodo-best-motherfucking-website pull
```

If you're on a versioned branch, and want to switch to a different version, run these commands substituting `0.1.0-standalone` for the new branch you want to target:

```text
git -C themes/hugomodo-best-motherfucking-website checkout 0.1.0-standalone
git config -f .gitmodules submodule.themes/hugomodo-best-motherfucking-website.branch 0.1.0-standalone
```

### Updating All Themes and Extensions

If you have multiple themes or extensions installed, you can update them all with one command like so:

```text
git submodule foreach git pull
```

## Support on Beerpay
Hey dude! Help me out for a couple of :beers:!

[![Beerpay](https://beerpay.io/hugomodo/hugomodo-best-motherfucking-website/badge.svg?style=beer-square)](https://beerpay.io/hugomodo/hugomodo-best-motherfucking-website)  [![Beerpay](https://beerpay.io/hugomodo/hugomodo-best-motherfucking-website/make-wish.svg?style=flat-square)](https://beerpay.io/hugomodo/hugomodo-best-motherfucking-website?focus=wish)
