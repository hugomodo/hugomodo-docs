+++
authors = ["Thom Bruce"]
date = "2019-02-16T00:00:00+00:00"
draft = true
images = []
title = "Choosing a Style Framework"

+++
Been a while since I've updated the blog here, but I've been busy whittling away at things in the background. HugoModo has made some developments with the introduction of two new themes, one based on Bootstrap and one on Foundation, but...

Well, I've been going about it all wrong. And now I'm thinking we'll have to drop support for not only the Foundation theme but also for the simpler Picnic and Milligram themes in order to focus purely on integrating with Bootstrap.

It's not ideal... but make no mistake, I do think those other options will make a return in the future. For now, I've tried to do too much with too little, and it's showing as I try to put things in their proper place with the Bootstrap theme.

## HugoModo is still a really clean, semantic HTML framework

The trouble is, I started out with the cleanest, simplest approach possible. The base HugoModo framework had only the essential HTML markup, the vast majority of it semantic and purposeful.

My theory was that I could leave the base theme that way and do everything I'd want in child themes via the CSS alone, no changes to the markup.

And it worked! For the really simple, [Best Motherfucking Website theme](/themes/best-motherfucking-website "HugoModo Best Motherfucking Website theme"), and for the truly minimalist and elegant frameworks, Milligram and Picnic.

So most recently, I've tried to extend the same approach to the more advanced frameworks, Bootstrap and Foundation.

Foundation has some... other problems, which despite my beginnings forced me to rule it out relatively early in development. Namely the use of pseudo-classes in Foundation just to achieve a sticky navbar. I still love Foundation, but that isn't going to fly for HugoModo - perhaps we'll revisit it later.

But the problem that Bootstrap and Foundation both have is the use of deep nesting in order to achieve grid and flexbox based layouts. It forced me to add additional divs to the base HugoModo theme, whose only purpose was to support advanced theming in these frameworks.

And that was fine, for a while. They didn't impeded other frameworks, and I had a vision of the base template being capable of supporting any of these theming frameworks.

The trouble came when attempting to make a big, full-width featured image for articles and then also a narrower, centred article with meta information aligned to the side.

To achieve that, I would have to... place the article content and meta in a shared container div. But this interrupts the document flow in a way that restricts further reordering and theming considerations, particularly when it comes to working with other themes.

It's the first blockade to a universal approach to markup, but it's a critical one.

Thus far, no other styling div I'd added had interrupted the overarching document structure, but this one would. And I can't have that; HugoModo is still, and should remain, a really clean and semantic HTML framework.

## Out with the new

So here's what's gonna happen...

Rather than attempt to make HugoModo a one-size-fits-all framework with universal markup that can be styled by any framework with CSS alone... I'm gonna return it to its roots.

HugoModo will return to purely the necessary, purely the semantic HTML that makes it great for SEO and accessibility.

Going forwards, the plan will be to make HugoModo's component partials as well-organised and modular as possible, so that styling frameworks have the minimum possible footprint when overwriting files.

But that's what they'll do... Styling frameworks will now overwrite partials, to provide whatever specific markup they need. The required changes are too flow-breaking to introduce to the base, so this is now an essential step.

It's a little more work, but ultimately a more familiar and customisable experience.

To focus on it, I'm dropping support for all themes other than _Best Motherfucking Website_ and _Bootstrap_. The former is tremendously minimalist but beautiful, and requires little reconfiguration. Bootstrap will require a little more work, hence the lack of time for others... and probably breaking changes for them for now. _That said, the Milligram and Picnic themes should continue to work in their current (incomplete) state with HugoModo v0.1.1 for the time being._

## I still think a minimalist CSS framework with advanced functionality is possible