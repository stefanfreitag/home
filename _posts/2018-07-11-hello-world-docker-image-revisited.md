---

title: 'Hello World Docker Image - revisited'
date: 2018-07-11T21:34:40+02:00
author: Stefan Freitag
layout: post
---
In an older blog post I showed you how to create a simple
[Docker](https://www.docker.com/) image. Now it is time to have a look at
it again.

```plain
$ docker images
REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
[...]
stefanfreitag/hello-world            0.0.1               aab3ed0ed86f        3 months ago        726MB
openjdk                              &lt;none&gt;              891c9734d5ab        3 months ago        726MB
[...]
```

As you can see the created image has a size of 726 MByte - for a simple
"Hello World" that seems quite a lot.

When publishing the image, its size does not really matter, but it is crucial
when creating containers. First of all the image needs to be transferred from
the repository to the target machine (e.g. a [Kubernetes](https://kubernetes.io/)
node or a [Docker Swarm](https://docs.docker.com/engine/swarm/) worker).
Secondly the image is stored on the target machine and the smaller the images
are the more you can store.

The idea for improving the situation is hidden in the first line of the Dockerfile

```plain
FROM openjdk:8
COPY HelloWorld.class /root
WORKDIR /root
CMD ["java", "HelloWorld"]
```

For the execution of the HelloWorld application we do not need a
[JDK](https://en.wikipedia.org/wiki/Java_Development_Kit) at all. Having access
to a JRE is sufficient, so the first line can be replaced.

```plain
FROM openjdk:jre-alpine
COPY HelloWorld.class /root
WORKDIR /root
CMD ["java", "HelloWorld"]
```

The [jre-alpine](https://github.com/docker-library/openjdk/blob/dd54ae37bc44d19ecb5be702d36d664fed2c68e4/8/jre/alpine/Dockerfile) maps to a [Alpine Linux](https://alpinelinux.org/)
and JRE version 8. With the updated Dockerfile a new image can be created.

```plain
REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
[...]
stefanfreitag/java8_jre_helloworld   0.0.1               8826b823fa48        47 hours ago        82MB
stefanfreitag/java8_jre_helloworld   latest              8826b823fa48        47 hours ago        82MB
openjdk                              jre-alpine          9462e6d3786a        5 days ago          82MB
[...]
```

Instead of the aforementioned 726 MByte the size of the image now is 82
MByte - that is approx. 12% of the original size.
