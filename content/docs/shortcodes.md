---
title: "Shortcodes"
date: 2018-12-29T12:58:37Z
draft: false
description: Built-in Hugo shortcodes and custom shortcodes provided by HugoModo.
---

## Hugo

All of the standard [Hugo shortcodes](https://gohugo.io/content-management/shortcodes/) work with HugoModo by default.

Those listed below also work with the shims provided by the [Shortcode Partials](/extensions/shortcode-partials) extension.

### figure

#### Markup

``` html
{{</* figure src="/img/mountaineer.jpg" title="A walk with a view." */>}}
```

#### Example

{{< figure src="/img/mountaineer.jpg" title="A walk with a view." >}}

### gist

#### Markup

``` html
{{</* gist spf13 7896402 */>}}
```

#### Example

{{< gist spf13 7896402 >}}

### highlight

#### Markup

``` html
{{</* highlight go-html-template */>}}
<section id="main">
  <div>
   <h1 id="title">{{ .Title }}</h1>
    {{ range .Pages }}
        {{ .Render "summary"}}
    {{ end }}
  </div>
</section>
{{</* /highlight */>}}
```

#### Example

``` go-html-template
<section id="main">
  <div>
   <h1 id="title">{{ .Title }}</h1>
    {{ range .Pages }}
        {{ .Render "summary"}}
    {{ end }}
  </div>
</section>
```

*Note: If you use one of the content shims provided by a HugoModo extension, the above shortcode example will fail. You should opt for markdown code fences instead. Check out the Hugo docs for setup instructions: [Highlight in Code Fences](https://gohugo.io/content-management/syntax-highlighting/#highlight-in-code-fences).*

<!---
### instagram

#### Markup

``` html
{{</* instagram BWNjjyYFxVx */>}}
```

#### Example

{{< instagram BWNjjyYFxVx >}}

### param

*Requires Hugo version 0.52 or higher*

#### Markup

``` html
{{</* param description */>}}
```

#### Example

{{< param description >}}

### ref and relref

#### Markup

``` markdown
[Hugo Goes Modular]({{</* ref "/blog/hugo-goes-modular.md" */>}})
[List Pages]({{</* relref "/blog/hugo-goes-modular.md#list-pages" */>}})
```

#### Example

[Hugo Goes Modular]({{< ref "/blog/hugo-goes-modular.md" >}})
[List Pages]({{< relref "/blog/hugo-goes-modular.md#list-pages" >}})

### tweet

#### Markup

``` html
{{</* tweet 877500564405444608 */>}}
```

#### Example

{{< tweet 877500564405444608 >}}

### vimeo

#### Markup

``` html
{{</* vimeo 146022717 */>}}
```

#### Example

{{< vimeo 146022717 >}}

### youtube

#### Markup

``` html
{{</* youtube w7Ft2ymGmfc */>}}
```

#### Example

{{< youtube w7Ft2ymGmfc >}}
--->

## HugoModo

HugoModo will extend the standard set of shortcodes. Take a look at the extensions library for a complete list.
