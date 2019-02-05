+++
authors = ["Thom Bruce"]
date = "2019-02-04T00:00:00+00:00"
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

The idea is to create a `standalone` branch which reflects the current state of the main developments on `master`, then to copy files from the parent theme respecting and not replacing any that already exist in the child theme. This can be achieved relatively easily, like so:

```bash
cp -rn hugomodo hugomodo-best-motherfucking-website
```

The `r` and `n` flags tell the copy command to perform the copy recursively - meaning all files in all subdirectories - and to "not clobber" the destination files - meaning don't replace anything if it already exists. This way, the child theme will gain all of the files present in the parent but any overwrites will be respected and remain untouched.

The full process then is to checkout the `standalone` branch, reset it to the state of `master`, and copy the parent theme's files:

```bash
git checkout standalone
git reset master --hard
cp -rn ../hugomodo/. .
```

As you can see, I've slightly modified the `cp` command so it can be performed from the destination directory. `../hugomodo/.` means _look upwards one directory level for the hugomodo directory, and copy its root contents_. `.` doesn't really mean _root directory_, but for our purposes it's a fair explanation. The final `.` on its own means _the current directory_. In plain English, _go up one directory level, find hugomodo, copy its root contents into the current directory; do so recursively, respecting any files which already exist_. Or _copy recursive no-clobber hugomodo into this directory_.

Following this, the state of the `standalone` branch will be a match to the `master` development branch _plus_ the contents of the parent theme. Enough then to _stand alone_. We commit the changes and push to GitHub:

```bash
git add -A
git commit -m "rebuild standalone from master"
git push origin standalone --force
```

We have to force push here, because of our earlier `reset` - it destroys the branch history.

With that, I... _almost_ have a perfect `standalone` branch. There's just one more thing. After reseting to the state of `master` but before copying the parent files, I have to add:

```bash
rm -f config.toml
```

This `rm` _removes_ the file `config.toml`, and does so _forcefully_. We need to remove the child theme config file, because it's what tells Hugo to _also_ look for a parent theme, which we won't be using with this setup. We also need to adopt the config of the parent theme, to ensure that the default configuration settings are inherited.

And with that, we have a `standalone` branch. I've automated the entire task with a [Travis CI](https://travis-ci.com/ "Travis CI Continuous Integration") job, the full configuration for which is this:

```yaml
- stage: standalone
  script:
    - git checkout -B standalone
    - git reset master --hard
    - rm -f config.toml
    - cp -rn ../hugomodo/. . || true
    - git add -A
    - git commit -m "rebuild from master"
    - git push -u origin standalone --force
```

`git checkout -B` does a forceful branching from master into our target branch, `standalone`. Strictly, I think the `-B` flag renders the reset redundant, since it's a forceful rebranch of the state of `master`, but I've left the reset in for good measure. I added the `-B` so as to not assume the prior existence of the `standalone` branch - if it exists, that's fine, but if not this will make sure that it does. For that reason, I also have to say `git push -u`. The `-u` tells git to consider this destination the authority for this branch, which given that it's a new one (`-B`) we don't yet have; it's shorthand for `--set-upstream`.

There's only one change here I don't understand, and that's this: `cp -rn ../hugomodo/. . || true`. For some reason, the `cp` command was returning an error code and yet... _it was/is working_. I have no idea why at this time, so `|| true` is a hack that says _or return true_ or _if what comes before this errors or fails, return true instead_. The line doesn't fail, but it returns a code like it does, hence the need for that.

Anyway, hey! That's `standalone` branches **done!**

HugoModo Best Motherfucking Website now comes in a standalone flavour that doesn't require the base theme to work. Much easier installs for those who don't need the bleeding edge versions.

I'll be documenting the much improved installation process soon.
