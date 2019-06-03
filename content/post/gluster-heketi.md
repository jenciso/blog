---
title: Cleaning Gluster Heketi volumes
date: 2019-06-03
description: how to clean a gluster heketi environment
thumbnail: https://angristan.xyz/content/images/size/w2000/2018/10/glusterfs.png
tags:
  - gluster
  - heketi
---

## Background

I'm using [gluster + heketi](https://github.com/gluster/gluster-kubernetes) in my kubernetes cluster to give a persistent storage service on demand. Periodically, I need to clean up my gluster volumes because for some reason the heketi volumes is not equal with the gluster bricks.

## Delete the orphands bricks

Some bricks haven't the path correctly. So, you need to clean up using the `heketi` binary command (the server, not the client).

Steps:

* Scale down your heketi service

```console
$ kubectl scale deployment -n gluster-heketi heketi --replicas=0
```

* Find out your `heketidbstorage` mount point 

``` 
$ kubectl exec -it -n gluster-heketi heketi-5c88f4574d-jxkpl -- df -k
``` 

* Mount the gluster volume of `heketi.db`

``` 
$ sudo apt-get -y install glusterfs-client
$ sudo mount -t glusterfs 10.64.14.47:/heketidbstorage  /mnt
``` 
> `10.64.14.47` is one of my gluster servers

* Download the heketi binary

```console
$ wget https://github.com/heketi/heketi/releases/download/v9.0.0/heketi-v9.0.0.linux.amd64.tar.gz 
$ cd heketi
$ sudo ./heketi db delete-bricks-with-empty-path --dbfile=/mnt/heketi.db --all
$ umount /mnt
```
* Scale up your heketi service

```console
$ kubectl scale deployment -n gluster-heketi heketi --replicas=1
```

* Resync your devices in all the nodes

``` 
$ kubectl exec -it -n gluster-heketi heketi-5c88f4574d-jxkpl -- \ 
  heketi-cli device resync 4c2a876a7829f3660e5a08cd1c0a5b1c
```
> You need to do that in all your devices


* Check your heketi environment

``` 
$ kubectl exec -it -n gluster-heketi heketi-5c88f4574d-jxkpl heketi-cli topology info
``` 


---
Source: https://github.com/heketi/heketi/issues/926

