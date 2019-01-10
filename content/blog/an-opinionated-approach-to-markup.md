+++
date = "2019-01-09T00:00:00+00:00"
images = []
title = "An Opinionated Approach to Markup"

+++
I was asked today how many users Hugomodo has at the moment. I expect the answer is actually just the one, and I kind of hope that's the case for now too. I do have plans to introduce a [sort of semantic versioning](https://github.com/hugomodo/hugomodo-best-motherfucking-website/issues/1 "Semantic Versioning, Sort Of Issue on GitHub") strategy for themes but it isn't my immediate priority. So for the time being, HugoModo development is unstable. I'm still moving the pieces around before they settle into place.

For instance, I now have some experimentation to do with HTML and SCSS. I have a philosophy in mind that is something I've wanted to try for a long time. I'm sure that I'll discover along the way the reasons that others don't do this. It seems to me it would be the obvious choice, and yet it's hard to find discussion of this principle online (maybe I'm not looking hard enough).

That philosophy is this: HTML documents should contain as little markup as necessary and as few classes as necessary to achieve the desired structure and style.

Now, I'm more developer than designer which may well be my major fault in considering what I intend to embark on here. But the thought first occurred to me when I discovered [Tachyons](https://tachyons.io/ "Tachyons CSS Framework").

Tachyons is a CSS toolkit that seems, at first glance, to be in total violation of what I'm talking about. Far from utilising as few classes as possible, Tachyons has many, many, many classes that may be used in conjunction on the same element to style it a myriad of different ways.

It is fantastic!

But if you're using Tachyons that way, I argue there's a better way.

That better way is a CSS preprocessor like [SASS](https://sass-lang.com/ "SASS CSS Preprocessor"). Y'see, SASS allows you to write style rules like this:

```scss
.my-selector {
  @extend .some-other-selector;
}
```

In that example, the `.my-selector` selector will obtain all of the style rules that `.some-other-selector` possesses, and any element in your HTML having the class `.my-selector` will be provided those styles.

Let's take a look at how we might one of Tachyons' styles in a practical example.

Tachyons provides the classes `.underline` and `.ttc` for use in HTML. The former does as it says, it underlines text, and `.ttc` is a text transform utility that will CAPITALISE the text of elements it is added to. Both might be handy for one of our header elements:

```html
<h1>Site Title</h1>
<h2 class="underline ttc">Article Title</h2>
```

With Tachyons loaded, the above markup would underline and capitalise the article title, but not the site title.

But let's say we decide against using Tachyons, and instead fall back to using something a little more traditional, [Bootstrap](https://getbootstrap.com/ "Bootstrap Styling Framework").

Searching the docs for Bootstrap, I don't see an underline utility. They give an example of underlining text using the `<u>` element, but I think that's bad practise. They do meanwhile have `.text-uppercase` to capitalise text.

To preserve the styles, we now have to do this:

```html
<h1>Site Title</h1>
<h2 class="text-uppercase"><u>Article Title</u></h2>
```

The class is easily enough handled with a find and replace, but that `<u>` element has caused us a second problem. It's not as easily inserted.

But if we instead had the following in our SCSS stylesheet:

```scss
// Tachyons example
h2 {
  @extend .underline;
  @extend .ttc;
}
```

We could replace that right there with the Bootstrap styling rules:

```scss
// Bootstrap example
h2 {
  @extend u;
  @extend .text-uppercase;
}
```

In theory, that does the trick. In principle, it is probably better to manually style that underline rather than extend `u` but I'll leave that as is for illustrative purposes.

I think it's easy to see that keeping styling rules and decisions about their combination in the stylesheet is far more maintainable in the long term.

Rather than attaching two styling classes to our article titles, we might attach just the one class, say `.article-title` and style it with extensions and rules of its own in our SASS stylesheets. We might even extend another class or element with that class, and it would inherit all of the extensions that it has. SASS is Awesome!

So, with all of that in mind - as few styling artefacts as possible in markup, use a CSS preprocessor, use `@extend` - my aim is to have as few classes in my markup as possible. Preferably they should be purely semantic, describing the nature of the content they contain, not the nature of the style applied. I can then make my styling decisions in the SASS stylesheets.

Example markup:

```html
<html>
  <head>
  	...
  </head>
  <body>
    <header>
      ...
    </header>
  	<main>
      <article class="blog-post">
        <figure class="featured-image">
        </figure>
        <header>
          <h1>Article Title</h1>
        </header>
        <section>
          ...
        </section>
        <footer>
          ...
        </footer>
      </article>
    </main>
    <footer>
      ...
    </footer>
  </body>
</html>
```

Notice the use of semantic HTML5 elements (not a `div` in sight yet) and the use of only **two** classes, `blog-post` and `featured-image`.

We don't, for example, need to provide a class to either of the `header` elements, because they can be described perfectly well by the section they belong to, `body` or `article.blog-post`. We do add a class to `article`, because we might add a secondary article type, a product listing perhaps that we'd like to style differently.

We might also consider adding a class to the body, to differentiate between pages of blog posts and of product listings. `<body class="blog">` and `<body class="store">` for instance. And from that we could style every item on the page differently straight from the stylesheet.

And given that we've used semantic HTML5 elements, we don't have countless divs that need differentiating between. _Although I should note that while I recommend this approach, these HTML5 elements should not be used merely for styling purposes but should absolutely also be descriptive of their contents. Do use `div` for arbitrary styling of block-level elements (and `span` incidentally for text, not `u`)._

I guess what I'm arguing for is an approach to styling that's semantic, like the HTML it ought to be being applied to here in 2019 (I don't blame you if you don't - I'm as bad as you are for not keeping up).

The philosophy is simple:

1. HTML should be clean and semantic.
2. HTML classes should be few and semantic.
3. CSS should do the heavy lifting.
   1. Use a CSS preprocessor.
   2. Use `@extend`.
   3. Use variables, mixins, whatever.

This should mean minimal reconfiguration or easier refactoring if and when style frameworks are swapped out. Something that's so important for the modularity of HugoModo. We'll see how that pans out...

If everything goes wrong, I'll do a follow up with lessons learned. If not, I will return waving a beautifully styled flag and encouraging you all to join the revolution. Watch this space.
