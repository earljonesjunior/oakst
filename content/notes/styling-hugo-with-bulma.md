---
title: "Styling Hugo With Bulma"
date: 2020-07-12T14:47:25-05:00
slug: ""
draft: false
tags: ["bulma", "hugo"]
project: ""
description: ""
---

Styling Hugo with [Bulma](https://bulma.io/) is surprisingly simple. Unfortunately there isn't much information in the Hugo documentation on how to actually do it. Here's what you'll need to do.

## Download and import Bulma
Download Bulma and extract the zip into the `/assets` directory in the root of your Hugo project. You'll 
need to create the folder if it does not exist. Hugo will search this directory when processing the SCSS.

Create a new SCSS stylesheet in the `/assets` directory. We'll call it `stylesheet.scss`. This is the
stylesheet you'll use to import the Bulma stylesheets and customize the default variables. The
[Bulma documentation](https://bulma.io/documentation/customize/concepts/) shows you all of the variables that
you can customize. Here's a snipit from this website's stylesheet:

```scss
// assets/stylesheet.scss
@charset "utf-8";

// Variable overrides come before you import Bulma
$primary: #f05444;
$size-1: 2rem;

// import Bulma
@import "./bulma-0.9.0/bulma.sass";

// styles that use the Bulma mixins come after you import Bulma
#navMenu{
    @include mobile{
        display: none;
        background-color: $white-bis;

        a{
            padding: 1em !important;
        }

    }
}
```

Take note the order of the steps from the above example. You first define your variables, then you import
Bulma, then you can use the Bulma mixins. I use the `@include mobile` mixin to hide the mobile navigation
menu when the page is viewed on wider viewports.

Bulma was designed in such a way where it's possible (and recommended) to import only the styles that you
need to decrease page load times and save some bandwidth. Check out how that's done in the [documentation](https://bulma.io/documentation/customize/with-node-sass/#6-add-your-own-bulma-styles).

## Use Hugo Pipes to process the SCSS

The SCSS will need to be transpiled into CSS. You can certainly use other build pipelines but
[Hugo Pipes](https://gohugo.io/hugo-pipes/) makes things incredibly easy. 

I import my Bulma stylesheets in `baseof.html` because that then applies the styles to 
every page in my website. This is what your head might look like:

```html
// layouts/_default/baseof.html

<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
  {{ $style := resources.Get "/stylesheet.scss" | toCSS | minify }}
  <link rel="stylesheet" href="{{ $style.Permalink | relURL }}" />
  <title>{{ .Title }} </title>
</head>
```

The line surrounded by the brackets is where the magic happens. Hugo gets the SCSS file and transpiles
it into CSS, and minifies it all in one step. How simple is that?