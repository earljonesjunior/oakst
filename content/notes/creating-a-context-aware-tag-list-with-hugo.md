---
title: "Creating a Context-Aware Tag List With Hugo"
date: 2020-07-15T15:41:52-05:00
slug: ""
draft: false
tags: ["hugo", "front-end"]
project: ""
description: ""
---

Check out the list of site tags to the right (to the bottom for mobile users). Notice how some of the 
tags are highlighted? The current page you are viewing is tagged with these highlighted tags. Clicking 
any of these tags brings you to a list of the all of the pages on this site that have that tag.

How cool is that! Here's how it's done.

## Make a new partial

The list of tags can be placed anywhere but I think it makes the most sense to put it in its own partial. 
I call my partial `tagbar.html`. This partial gets called in my `baseof.html` with `{{ partial "tagbar.html" . }}`. Note the period at the end is needed as it passes along the page's context to the partial.

## Grab the page's tags
The following snippet grabs the page's tags and places them into a new slice called `$pageTags`:

```html
{{ $pageTags := slice "" }}
{{ range (.GetTerms "tags") }}
    {{ $pageTags = $pageTags | append .LinkTitle}}
{{ end }}
```

## Compare the page tags to the site's tags
This next chunk of code grabs all of the tags for the website and checks if they exist in the `$pageTags` slice. It also checks the title of the page to see if it matches a tag - this is used when viewing a list
of pages belonging to that taxonomy term. If that tag matches a tag in the `$pageTags` slice, 
`$isActive` is set true.

Note how `site` in the first line of this chunk is lowercase -
this tells Hugo to use the global site function as the range's context. Read more about that
[here](https://gohugo.io/variables/site/#get-the-site-object-from-a-partial).

```html
{{ range site.Taxonomies.tags }}
    {{ $isActive := or (in $pageTags .Page.LinkTitle) ( eq $.Page.LinkTitle .Page.LinkTitle)}}

    ...

{{ end }}
```

## Highlight the active tags
If `$isActive` is set true, the tag style is set to the highlight CSS class. If set false, the inactive
CSS class is used. I use the Bulma CSS framework to style the elements on this website but any CSS classes can be used.

```html
<a 
class="tag {{ if $isActive }} is-warning is-light {{ else }} is-white {{ end }}" 
href="{{ .Page.Permalink }}"
>
        #{{ .Page.Title }}
</a>
```

## Putting it all together
This is the final result, placed inside a div:

```html
// layouts/partials/tagbar.html

<div class="content has-text-centered tagbar">

    {{ $pageTags := slice "" }}
    {{ range (.GetTerms "tags") }}
        {{ $pageTags = $pageTags | append .LinkTitle}}
    {{ end }}

    {{ range site.Taxonomies.tags }}
        {{ $isActive := or (in $pageTags .Page.LinkTitle) ( eq $.Page.LinkTitle .Page.LinkTitle)}}

        <a 
        class="tag {{ if $isActive }} is-warning is-light {{ else }} is-white {{ end }}" 
        href="{{ .Page.Permalink }}"
        >
            #{{ .Page.Title }}
        </a>

    {{ end }}

</div>
```