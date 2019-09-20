---
layout: post
date: 2019-06-02
published: true
comments: true
thumbnail: https://avatars3.githubusercontent.com/u/6506055?s=400&v=4
title: Embedding asciinema cast in your Hugo site
tags:
  - hugo
  - asciinema
categories:
  - linux
asciinema: true
---

Background
-------

I use [Asciinema](https://github.com/asciinema/asciinema) commonly to share my favorite commands, it is an excellent tool for technical blogs, documentation, and landing pages alike. It allows you to create compact animations of commands along with their output and formatting with far less overhead than the alternative of creating a gif or mp4.

You can embed casts hosted on asciinema into your Hugo posts and pages by embedding a snippet like this directly into the markdown:

```js
<script src="https://asciinema.org/a/186686.js" id="asciicast-186686" async></script>
```

Although, it works fine, you could use it into your blog without upload the cast file in public. Thinks in security IT department rules, if you have a private blog it could be important to host the cast file into your statics files. So, these are the steps to do that

## Embedding in Hugo with shortcodes

Download the latest asciinema-player release from their [GitHub page](https://github.com/asciinema/asciinema-player/releases).

#### Add static assets to your head

Add this code in your `head` template section:

```html
{{ if .Params.asciinema }}
    <link rel="stylesheet" type="text/css" href="{{ .Site.BaseURL }}css/asciinema-player.css" />
{{ end }}
```
And this too, before the closing `body` tag in your template’s footer:

```html
{{ if .Params.asciinema }}
    <script src="{{ .Site.BaseURL }}js/asciinema-player.js"></script>
{{ end }}
```

#### Create a shortcode

Create a file `layouts/shortcodes/asciinema.html` with the following contents:

<script src="https://gist.github.com/jenciso/c41c5be03bb1f412b96326b1cb885cc0.js"></script>

> The fields map directly to those documented [here](https://github.com/asciinema/asciinema-player#asciinema-player-element-attributes). All fields are supported, but only the key is required.

#### Create casts

Follow the instructions to create new casts. Save them to `static/casts/<key>.cast`.

#### Using the shortcode

You can now embed asciinema casts in your Hugo posts and pages with the following shortcode:

```sh
{{</* asciinema key="demo-cast" rows="10" preload="1" */>}}
```
You also need to include `asciinema = true` in the frontmatter for you post too, to make sure the javascript and css assets are included.

#### Result

You should end up with something like this:

{{< asciinema key="249623" cols="158" rows="40" preload="1" speed="1" >}}

#### References 

https://www.tonylykke.com/posts/custom-fonts-in-asciinema/
