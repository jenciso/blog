---
layout: post
comments: true
title: Altering the minio bucket policy in kubernetes
date: 2018-08-09
thumbnail: https://www.worksonarm.com/wp-content/uploads/2017/01/Minio-Logo.jpg
tags:
  - minio
categories:
  - cloud
---
## Intro

If you want to create a public bucket, you can use the minio web interface to alter its bucket policy.

First you need to get the credentials. Ex:

```shell
$ kubectl exec -it -n realiza storage-realiza-minio-6644999-lvtvp env | egrep MINIO.*KEY=  
MINIO_ACCESS_KEY=XJNCWECEEMSNXVSWSC
MINIO_SECRET_KEY=BcjweDZOVMECCIfiCY
$ 
```

Create a port-forward to access into the web interface

```console
$ kubectl port-forward -n realiza storage-realiza-minio-66449995b5-q2qt7 9000:9000
``` 

Then, you need to access via http://localhost:9000 and edit the policy and use the wildcard `*`. Like this image:

![Screenshot from 2018-08-09 21-22-18.png](/images/Screenshot from 2018-08-09 21-22-18.png)

