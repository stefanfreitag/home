---
title: Transferring Docker images without registry
date: 2018-07-13T20:38:38+02:00
---

Sometimes it can be very difficult or even impossible to access the public or
even a private Docker registry for pulling images. For such situations Docker
offers another way of transferring images to the target machines: the import and
export via the commands
[load](https://docs.docker.com/engine/reference/commandline/load/) and
[save](https://docs.docker.com/engine/reference/commandline/save/).

## Exporting the image

`docker save`

> Produces a tarred repository to the standard output stream. Contains all
> parent layers, and all tags + versions, or specified `repo:tag`, for each
> argument provided.

To prepare my HelloWorld image I do

```shell
$ docker save --output java8_jre_helloworld.tar stefanfreitag/java8_jre_helloworld:0.0.1
$ ls -lah  java8_jre_helloworld.tar 
-rw------- 1 stefan stefan 80M Jul 13 20:02 java8_jre_helloworld.tar
$ gzip java8_jre_helloworld.tar 
$ ls -lah  java8_jre_helloworld.tar.gz 
-rw------- 1 stefan stefan 54M Jul 13 20:02 java8_jre_helloworld.tar.gz
```

Aside of the export as tar, I also compress it. For the transport to the target
machines standard protocols like scp or (s)ftp could be used.

## Importing the image

`docker load`

> Load an image or repository from a tar archive (even if compressed with gzip,
> bzip2, or xz) from a file or STDIN. It restores both images and tags.

To import the image on the target machine I run

```shell
$ docker load --input java8_jre_helloworld.tar.gz 
Loaded image: stefanfreitag/java8_jre_helloworld:0.0.1</pre>
```

A quick `docker images`on the target machine shows

```shell
$ docker images
REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
stefanfreitag/java8_jre_helloworld   0.0.1               8826b823fa48        3 days ago          82MB
[...]
```
