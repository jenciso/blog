---
layout: post
published: true
categories:
  - network
  - vpn
mathjax: false
featured: true
comments: true
title: Using a custom openconnect version for VPN
toc: false
date: 2017-10-21
---

My company use Palo Alto Networks to offer a VPN service for their employees. Personally I prefer Openvpn software, of course, because it is Open Source.

I don't know, but the openconnect version installed by default in my `Fedora 26` haven't the protocol support for GLobal Protect of Palo Alto, so for that reason, I had to compile a custom version of this software. [Here] (https://github.com/dlenski/openconnect) is the procediment to compile it, and here are the my own steps to run it

## Requeriments

### Fedora Users

First at all, you need to install these packages 

```console
$ sudo dnf -f install libxml2-devel \
  zlib-devel \
  openssl-devel \
  pkg-config \
  p11-kit \
  libp11 \
  libproxy \
  trous \
  libproxy-devel \
  libstoken 
```

### Ubuntu 

```console
$ sudo apt-get install \
    build-essential gettext autoconf automake libproxy-dev \
    libxml2-dev libtool vpnc-scripts pkg-config \
    libgnutls-dev
```
> Tested in Ubuntu 16.04

## Compiling

After, you need to download the repo for dlenski github pages and compile it

```console
$ git clone https://github.com/dlenski/openconnect.git
$ cd openconnect
$ git checkout globalprotect
$ ./autogen.sh
$ ./configure
$ make
$ sudo make install
```

Load the shared libraries

```console
$ sudo ldconfig
```

Finally, you can test it!

```console
$ sudo /usr/local/sbin/openconnect --protocol=gp vpn.mycompany.com.br --dump -v
```

## Notes

To create an init script, [here](https://serverfault.com/questions/584163/supplying-password-to-openconnect-started-via-start-stop-daemon) you have some ideias.
