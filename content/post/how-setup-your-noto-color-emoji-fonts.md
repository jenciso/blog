---
title: How setup the Noto Color Emoji in your Ubuntu
description: Setting the color emoji unicode chars
layout: post
published: true
comments: true
thumbnail: "/images/noto_color_emoji01.png"
date: 2019-06-11
tags:
  - ubuntu
  - emoji
---

## Background

Ubuntu 18.04 LTS ships with all-new color emoji for use in messaging apps, text editors, and also on the web. Emoji is nothing new for Ubuntu. Older versions of Ubuntu come with simple black-and-white emoticons.In Ubuntu 16.04 LTS it could be a little hard to install it. So, here are th step to install it manually


## Steps

* Download from https://www.google.com/get/noto. If you prefer, you could download it directly from [here](https://noto-website-2.storage.googleapis.com/pkgs/NotoColorEmoji-unhinted.zip)

* Copy the ttf file into the `.fonts` dir

```shell
$ cd ~/Downloads
$ unzip NotoColorEmoji-unhinted.zip
$ mkdir ~/.fonts
$ mv NotoColorEmoji.ttf ~/.fonts
```

* Edit the `.fonts.conf` file

```shell
$ vim ~/.fonts.conf
```

* Copy the following text

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>

   <alias>
     <family>sans-serif</family>
     <prefer>
       <family>Noto Color Emoji</family>
       <family>Noto Emoji</family>
     </prefer>
   </alias>

   <alias>
     <family>serif</family>
     <prefer>
       <family>Noto Color Emoji</family>
       <family>Noto Emoji</family>
     </prefer>
   </alias>

   <alias>
    <family>monospace</family>
    <prefer>
      <family>Ubuntu Mono</family>
     </prefer>
   </alias>

</fontconfig>
```

* Rebuild the cache font

```shell
$ sudo fc-cache -fv
```

> You need to restart your chrome and if you have problems with your Terminal, because some characters appears a little big or some wierd, you should setup your Profile to specify the size font and not permit to ubuntu setup for you the fonts
 
