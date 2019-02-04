+++
authors = ["Thom Bruce"]
date = "2019-02-04T00:00:00+00:00"
draft = true
images = []
title = "Considerations for Standalone Theme Releases"

+++
With the release of HugoModo version 0.1, I've turned my consideration to the child themes which inherit its features. The plan is to introduce versioning to these as well, but also an additional flavour called _standalone_. So as well as a `0.1.0` branch, child themes will also feature a `0.1.0-standalone` branch and release files that will ultimately make it easier to use HugoModo themes that may have one or two to many levels of inheritance. At present, to use a HugoModo theme one must also download and include the base theme in their project. This is a minimal inconvenience when the theming goes just a couple of levels deep, but as I add more themes and adaptations of child themes, it's going to potentially become a bit of a burden, and certainly it goes against the goal to make HugoModo easy to use.

## How HugoModo Currently Works

Let's take the first theme I've developed for HugoModo, based on the excellent _The Best Motherfucking Website_ by [Denys Vitali](https://denv.it/ "Denys Vitali's Website"). The theme is called _HugoModo Best Motherfucking Website_, and to use it requires you take two steps.

First, you have to add not only that theme but the base HugoModo theme to your Hugo site. That might look a little something like this:

```bash
git submodule add https://github.com/hugomodo/hugomodo themes/hugomodo
git submodule add https://github.com/hugomodo/hugomodo-best-motherfucking-website themes/hugomodo-best-motherfucking-website
```

Then, you need to utilise the themes in your project. The child theme already includes the base theme for you, so you only need to add this to your Hugo config:

```toml
theme = [
  "hugomodo-best-motherfucking-website"
]
```

Nothing too complicated. But as we add themes that might inherit from the child theme, then additional themes that inherit from those (as is likely to be the case as I adapt a number of Bootstrap templates), that first step is going to become a big one. That won't do.

Better would be, regardless of the depth of inheritance, a single step like so:

```bash
git submodule add -b standalone https://github.com/hugomodo/hugomodo-best-motherfucking-website themes/hugomodo-best-motherfucking-website
```

One command with just one small change, the option to use the `standalone` branch with `-b standalone`.

That's the future of HugoModo, I just need to figure out how to achieve it.

## Submodules within Submodules

This is the first method I've tried. I'd been super hopeful about it for a long time, and it mostly works.

See, when you're inside a Git submodule, it's just like any other repo. It is just another repo, which means it can also have submodules included.

My cunning plan to incorporate the base theme into standalone versions of child themes was to include the base theme in a `themes` directory within the child theme, which itself of course would be in the Hugo site's `themes` directory.

Then, rather than including `theme = ["hugomodo"]` in a child theme, I would instead include in the theme config:

```toml
theme = [
  "hugomodo-best-motherfucking-website/themes/hugomodo"
]
```

I was hesitant about this setup, but in my early tests it worked!

There were drawbacks. This would only really work for one level of theme inheritance, where down the line I might go several themes deep. But this could be worked around inheriting from the standalone branches of those intermediary themes - a sort of chain of standalone inheritance. That would be absolutely fine.

But I've made some changes to how HugoModo works recently. Changes for the better - they make it more extendible, more modular (the _Modo_ in _HugoModo_).

In my most recent attempt to work through this approach, I've ran up against a wall with data files. In the submodule-based standalone version, Hugo can no longer locate the data files of the base theme, which I've used to make resource and asset extendibility a breeze.

Since I won't rollback the resource extendibility, this method is now a no-go.

## Copy Theme Files

This is the other option, and while it isn't ideal I think it's going to be the only way to have this work.

### TODO

https://superuser.com/a/1171400

```bash
cp -rn /hugomodo /hugomodo-best-motherfucking-website
```

That's probably what we want to do. `-r` is recursive, `-n` is no-clobber which will respect destination files if they exist.

But... `-n`... What if the base theme has new files added, or old ones removed?

Good question! I think we're going to have to rebuild from `@master` every time. So... how do we get standalone to match master, then copy the files?

Probably going to have to do a `git reset` against master: https://stackoverflow.com/a/36321787/2225649 We'll lose history... but them's the breaks, I guess. It is the simplest solution... and since it's _strictly_ a release branch, we're not that concerned with the history anyway.

So...

```bash
git checkout standalone
git reset master --hard
cp -rn ../hugomodo .
git commit -A -m "some commit message"
git push origin standalone --force # or --force-with-lease (but I have to consider the implications)
```
