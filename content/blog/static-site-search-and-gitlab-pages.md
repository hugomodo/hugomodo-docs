+++
date = "2019-01-04T00:00:00+00:00"
images = []
title = "Fuse.js Static Site Search and GitLab Pages"
authors = [
 "Thom Bruce"
]
+++
The greatest selling point of static sites over dynamic is also their greatest drawback. They're essentially serverless. That means no logins, no dynamic rendering and no form submissions. Of course there are workarounds for all of these, and with the right setup nobody will be able to tell the difference.

So, how does search work? Classically, you'd require a server. Your search query would be delivered to the server, it would query its own database for matches, and the results would be returned with the next page load (or delivered as a parseable document in the case of dynamic queries made using JavaScript).

One way in which static sites work around this limitation is by using a third-party API server. Essentially, you provide that server with your indexed content, they store it in their database, and then you can query the API for search results. This works, but I fear it produces two related issues. First, it's an extra dependency we don't necessarily want. And second, that index will need updated which means finding a way to communicate with the API when content is updated (it would be tedious to have to manually update the index every single time).

With HugoModo, I'm initially trying to keep dependencies on the lighter side. If it's possible without a server dependency, all the better. And it is! Which is why I put together an extension that uses [Fuse.js](http://fusejs.io/).

[HugoModo FuseJS](/extensions/fusejs) provides a search page template, a JSON index layout and the JavaScript files required to get it all working. That JSON index is the same as the index those third-party providers would expect, so this is a necessary step anyway. And once we have it, why not use it ourselves?

So that's how our extension handles it: it queries that index, created with our site and stored publicly.

And it just works! ...in GitHub Pages, in Netlify...

...but not in GitLab Pages. I was surprised. You might be surprised too. The reason it doesn't work appears to be the very same thing that we thought we were getting away from by choosing a static site generator like Hugo: it seems the server is to blame.

So, yes, static sites aren't strictly serverless. They just run on very minimal servers. And when we submit a search form on our site in GitLab Pages in particular, their servers appear to be stripping the query parameters and redirecting to the search results page, albeit without the necessary search query. I go into further detail in an issue I opened on the GitLab Pages repo: [#191](https://gitlab.com/gitlab-org/gitlab-pages/issues/191).

Frustrating, but perhaps submitting the query as a full page request was never the right way to handle it anyway. This has prompted me to make that Fuse.js extension a little niftier.

We work around the problem like so:

    jQuery("#search-query").parent().submit(function( event ) {
      if(jQuery("#search-query").val()){
        executeSearch(jQuery("#search-query").val());
      } else {
        jQuery('#search-results').html("<p>Please enter a word or phrase above</p>");
      }
      event.preventDefault();
    });

_Yup, sadly jQuery is still a dependency for now. I'm working on it!_

So, above we hijack the `.submit` function, and importantly at the bottom there we `preventDefault()` behaviour which would be to submit the form alongside a page request. Instead, if a value is present we execute our search using that value, the script for which is, and always has been, run as JavaScript on the page (we can't use a server, remember). You may peruse the full file if you wish, here: [HugoModo FuseJS Search Script](https://github.com/hugomodo/hugomodo-fusejs/blob/master/assets/js/search.js), based on a [GitHub Gist by eddiewebb](https://gist.github.com/eddiewebb/735feb48f50f0ddd65ae5606a1cb41ae).

Problem solved then. Search results are now loaded, or more accurately displayed, without a page refresh.

But I do hope that GitLab Pages can support form submissions in the future. Silly as they may seem on static sites, they do still open up a lot of possibilities!
