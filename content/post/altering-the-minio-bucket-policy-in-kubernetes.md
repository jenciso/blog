---
layout: post
comments: true
title: Altering the minio bucket policy in kubernetes
date: 2018-08-09
thumbnail: 'https://blog.alexellis.io/content/images/2017/01/minio_light_cir_sm-1.png'
tags:
  - minio
---
## Intro

If you want to create a public bucket, you can use the minio web interface to alter its bucket policy.

First you need to get the credentials. Ex:

```sh
✔ 21:21:09 [inspiron3647] ~ $ kubectl exec -it -n realiza \
storage-realiza-minio-66449995b5-q2qt7 -- env | egrep MINIO.*KEY=
MINIO_ACCESS_KEY=XJNCWECEDDSDSNXVEAAMKV
MINIO_SECRET_KEY=BcjweDZOVMECCI232443e3
✔ 21:21:10 [inspiron3647] ~ $
```

Create a port-forward to access into the web interface

```sh
✔ 21:21:20 [inspiron3647] ~ $ kubectl port-forward -n realiza \
storage-realiza-minio-66449995b5-q2qt7 9000:9000
✔ 21:21:20 [inspiron3647] ~ $
``` 

Then, you need to access via http://localhost:9000 and edit the policy and use the wildcard `*`. Like this image:

![Screenshot from 2018-08-09 21-22-18.png](/images/Screenshot from 2018-08-09 21-22-18.png)

