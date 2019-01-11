---
title: "Hugomodo"
date: 2018-12-29T14:38:59Z
draft: false
description: The base theme for HugoModo's modular approach to Hugo website design.
---
The base theme for HugoModo's modular approach to Hugo website design. It provides the asset precompilation pipeline and bundler data files, clean and semantic HTML, and well-organised partials for ease of developing child themes.

It provides minimal styling, so is best used as the base for a child theme. HugoModo themes which depend on it will include this theme automatically.

### Installation

```bash
git submodule add https://github.com/hugomodo/hugomodo themes/hugomodo
```

```toml
themes = ["hugomodo"]
```
