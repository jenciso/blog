---
layout: post
published: true
featured: false
comments: true
title: How setup Haproxy with zero downtime reload from code source
thumbnail: https://cdn.haproxy.com/wp-content/uploads/2015/10/HAProxy_Logo-2-1.png
tags:
  - haproxy
---
## Intro

HAProxy 1.8 and Linux kernel >=3.9 then you can take advantage of a new reload mechanism which can transfer sockets from the old process to the new process without dropping any connections.

This enables dynamic scalability and continuous integration all the way into production datacenters.

Here are the steps to install the new version of haproxy 1.8.14, that include reload daemon when the config file (haproxy.cfg) need to be changed.


## Steps 

* Download the latest haproxy version

```sh 
$ export HAPROXY_MAJOR=1.8
$ export HAPROXY_VERSION=1.8.14

$ wget -O haproxy.tar.gz \
"https://www.haproxy.org/download/${HAPROXY_MAJOR}/src/haproxy-${HAPROXY_VERSION}.tar.gz"
```

* Installing the prerequisites packages to compile

For ubuntu 16.04:

```
$ sudo apt-get update && sudo apt-get install -y ca-certificates gcc \
  libc6-dev liblua5.3-dev libpcre3-dev libssl-dev make wget \
  zlib1g-dev libsystemd-dev
``` 
For Centos 7.5:

```
$ yum install -y inotify-tools wget tar gzip make gcc perl pcre-devel zlib-devel iptables \
openssl openssl-devel openssl-libs systemd-devel
```

* Compiling

```console
$ tar xvfz haproxy.tar.gz
$ cd haproxy-1.8.14/
```

For ubuntu:
```
make TARGET=linux2628 USE_GETADDRINFO=1 USE_ZLIB=1 USE_REGPARM=1 USE_OPENSSL=1 \
USE_LUA=1 USE_SYSTEMD=1 USE_PCRE=1 USE_PCRE_JIT=1 USE_NS=1

make install
```

For Centos 7.5:
```
make TARGET=linux2628 USE_GETADDRINFO=1 USE_ZLIB=1 USE_REGPARM=1 USE_OPENSSL=1 \
USE_SYSTEMD=1 USE_PCRE=1 USE_PCRE_JIT=1 USE_NS=1

make install
``` 

* verify the compile options, you have to have the `USE_SYSTEMD=1` option

```c
HA-Proxy version 1.8.14-52e4d43 2018/09/20
Copyright 2000-2018 Willy Tarreau <willy@haproxy.org>

Build options :
  TARGET  = linux2628
  CPU     = generic
  CC      = gcc
  CFLAGS  = -O2 -g -fno-strict-aliasing -Wdeclaration-after-statement -fwrapv -fno-strict-overflow -Wno-unused-label
  OPTIONS = USE_GETADDRINFO=1 USE_ZLIB=1 USE_REGPARM=1 USE_OPENSSL=1 USE_LUA=1 USE_SYSTEMD=1 USE_PCRE=1 USE_PCRE_JIT=1 USE_NS=1

Default settings :
  maxconn = 2000, bufsize = 16384, maxrewrite = 1024, maxpollevents = 200

Built with OpenSSL version : OpenSSL 1.0.2g  1 Mar 2016
Running on OpenSSL version : OpenSSL 1.0.2g  1 Mar 2016
OpenSSL library supports TLS extensions : yes
OpenSSL library supports SNI : yes
OpenSSL library supports : TLSv1.0 TLSv1.1 TLSv1.2
Built with Lua version : Lua 5.3.1
Built with transparent proxy support using: IP_TRANSPARENT IPV6_TRANSPARENT IP_FREEBIND
Encrypted password support via crypt(3): yes
Built with multi-threading support.
Built with PCRE version : 8.38 2015-11-23
Running on PCRE version : 8.38 2015-11-23
PCRE library supports JIT : yes
Built with zlib version : 1.2.8
Running on zlib version : 1.2.8
Compression algorithms supported : identity("identity"), deflate("deflate"), raw-deflate("deflate"), gzip("gzip")
Built with network namespace support.

Available polling systems :
      epoll : pref=300,  test result OK
       poll : pref=200,  test result OK
     select : pref=150,  test result OK
Total: 3 (3 usable), will use epoll.

Available filters :
	[SPOE] spoe
	[COMP] compression
	[TRACE] trace
```

References:

https://fabianlee.org/2017/10/16/haproxy-zero-downtime-reloads-with-haproxy-1-8-on-ubuntu-16-04-with-systemd/

https://midiroot.com/post/haproxy-getting-started/

https://www.haproxy.com/blog/truly-seamless-reloads-with-haproxy-no-more-hacks/

https://engineeringblog.yelp.com/2015/04/true-zero-downtime-haproxy-reloads.html

https://thegeeksalive.com/how-to-setup-haproxy-http-load-balancer-on-centos/
