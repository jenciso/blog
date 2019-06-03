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

#### Download the latest asciinema-player release from their [GitHub page](https://github.com/asciinema/asciinema-player/releases).

#### Add static assets to your head

Add the following to your template’s `head` section:

```
{{ if .Params.asciinema }}
    <link rel="stylesheet" type="text/css" href="{{ .Site.BaseURL }}css/asciinema-player.css" />
{{ end }}
```
Add the following just before the closing `body` tag in your template’s footer:

```
{{ if .Params.asciinema }}
    <script src="{{ .Site.BaseURL }}js/asciinema-player.js"></script>
{{ end }}
```

#### Create a shortcode

Create a file `layouts/shortcodes/asciinema.html` with the following contents:

```html
<p>
    <asciinema-player
        src="/casts/{{ with .Get "key" }}{{ . }}{{ end }}.cast"
        cols="{{ if .Get "cols" }}{{ .Get "cols" }}{{ else }}640{{ end }}"
        rows="{{ if .Get "rows" }}{{ .Get "rows" }}{{ else }}10{{ end }}"
        {{ if .Get "autoplay" }}autoplay="{{ .Get "autoplay" }}"{{ end }}
        {{ if .Get "preload" }}preload="{{ .Get "preload" }}"{{ end }}
        {{ if .Get "loop" }}loop="{{ .Get "loop" }}"{{ end }}
        start-at="{{ if .Get "start-at" }}{{ .Get "start-at" }}{{ else }}0{{ end }}"
        speed="{{ if .Get "speed" }}{{ .Get "speed" }}{{ else }}1{{ end }}"
        {{ if .Get "idle-time-limit" }}idle-time-limit="{{ .Get "idle-time-limit" }}"{{ end }}
        {{ if .Get "poster" }}poster="{{ .Get "poster" }}"{{ end }}
        {{ if .Get "font-size" }}font-size="{{ .Get "font-size" }}"{{ end }}
        {{ if .Get "theme" }}theme="{{ .Get "theme" }}"{{ end }}
        {{ if .Get "title" }}title="{{ .Get "title" }}"{{ end }}
        {{ if .Get "author" }}author="{{ .Get "author" }}"{{ end }}
        {{ if .Get "author-url" }}author-url="{{ .Get "author-url" }}"{{ end }}
        {{ if .Get "author-img-url" }}author-img-url="{{ .Get "author-img-url" }}"{{ end }}
    ></asciinema-player>
</p>
```
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
