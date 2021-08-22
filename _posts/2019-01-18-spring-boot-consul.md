---
title: 'Spring Boot - Consul'
date: 2019-01-18T22:10:15+01:00
author: Stefan Freitag
tags:
  - Spring
  - Test
  - 8BIT
  - alignment
---

[Spring Boot](https://spring.io/projects/spring-boot) is a really cool
 framework and used in many services I wrote. Once a service is up and running,
 how do potential consumers receive information on the connection endpoint?
 Various options exist as answer to this question. Today I would like to
 prototype on the idea of service registration and discovery using
 [Consul](https://www.consul.io/).

## What is Consul?;

> Consul is a service mesh solution providing a full featured control plane with service discovery, configuration, and segmentation functionality. [...]
> The key features of Consul are:
>
> * **Service Discovery**: Clients of Consul can register a service, [...] and other clients can use Consul to discover providers of a given service. [...]
> * **Health Checking**: Consul clients can provide any number of health checks, either associated with a given service [&#8230;].

[Source](https://www.consul.io/intro/index.html)

In this post I will describe how to;

- setup a Consul service using Docker
- configure a Spring Boot application so that I registers with the Consul server

## Consul service setup

An official image for Consul is available at <a href="https://hub.docker.com/" target="_blank" rel="noopener">DockerHub</a>. I will use the latest release of the image:

```shell
$ docker pull consul
Using default tag: latest
latest: Pulling from library/consul
407ea412d82c: Pull complete 
c88bd7eadd45: Pull complete 
b4dbf0c150e4: Pull complete 
5970b88ac92c: Pull complete 
db9572188abf: Pull complete 
9119e44a3cca: Pull complete 
Digest: sha256:f911c43da6abad9c4d172ce58dec66c6742093a87ccfc419874f58dd7a1a0eb3
Status: Downloaded newer image for consul:latest
```

Based on the available documentation a "development mode" Consul container can
be created by

```shell
$ docker run -d --name=dev-consul -e CONSUL_BIND_INTERFACE=eth0 consul</pre>
```

After spinning up the container, its logs can be inspected

```shell

$ docker logs dev-consul
==&gt; Found address '172.17.0.2' for interface 'eth0', setting bind option...
==&gt; Starting Consul agent...
==&gt; Consul agent running!
           Version: 'v1.4.0'
           Node ID: '1f70feba-2d61-186a-d637-d8b32a4e25bf'
         Node name: 'f59abb77818b'
        Datacenter: 'dc1' (Segment: '&lt;all&gt;')
            Server: true (Bootstrap: false)
       Client Addr: [0.0.0.0] (HTTP: 8500, HTTPS: -1, gRPC: 8502, DNS: 8600)
      Cluster Addr: 172.17.0.2 (LAN: 8301, WAN: 8302)
           Encrypt: Gossip: false, TLS-Outgoing: false, TLS-Incoming: false

==&gt; Log data will now stream in as it occurs:

    2019/01/16 19:13:09 [DEBUG] agent: Using random ID "1f70feba-2d61-186a-d637-d8b32a4e25bf" as node ID
    2019/01/16 19:13:09 [INFO] raft: Initial configuration (index=1): [{Suffrage:Voter ID:1f70feba-2d61-186a-d637-d8b32a4e25bf Address:172.17.0.2:8300}]
    2019/01/16 19:13:09 [INFO] raft: Node at 172.17.0.2:8300 [Follower] entering Follower state (Leader: "")
    2019/01/16 19:13:09 [INFO] serf: EventMemberJoin: f59abb77818b.dc1 172.17.0.2
    2019/01/16 19:13:09 [INFO] serf: EventMemberJoin: f59abb77818b 172.17.0.2
    2019/01/16 19:13:09 [INFO] consul: Handled member-join event for server "f59abb77818b.dc1" in area "wan"
    2019/01/16 19:13:09 [DEBUG] agent/proxy: managed Connect proxy manager started
    2019/01/16 19:13:09 [INFO] consul: Adding LAN server f59abb77818b (Addr: tcp/172.17.0.2:8300) (DC: dc1)
    2019/01/16 19:13:09 [INFO] agent: Started DNS server 0.0.0.0:8600 (udp)
    2019/01/16 19:13:09 [INFO] agent: Started DNS server 0.0.0.0:8600 (tcp)
    2019/01/16 19:13:09 [INFO] agent: Started HTTP server on [::]:8500 (tcp)
    2019/01/16 19:13:09 [INFO] agent: started state syncer
    2019/01/16 19:13:09 [INFO] agent: Started gRPC server on [::]:8502 (tcp)
    2019/01/16 19:13:09 [WARN] raft: Heartbeat timeout from "" reached, starting election
    2019/01/16 19:13:09 [INFO] raft: Node at 172.17.0.2:8300 [Candidate] entering Candidate state in term 2
    2019/01/16 19:13:09 [DEBUG] raft: Votes needed: 1
    2019/01/16 19:13:09 [DEBUG] raft: Vote granted from 1f70feba-2d61-186a-d637-d8b32a4e25bf in term 2. Tally: 1
    2019/01/16 19:13:09 [INFO] raft: Election won. Tally: 1
    2019/01/16 19:13:09 [INFO] raft: Node at 172.17.0.2:8300 [Leader] entering Leader state
    2019/01/16 19:13:09 [INFO] consul: cluster leadership acquired
    2019/01/16 19:13:09 [INFO] consul: New leader elected: f59abb77818b
    2019/01/16 19:13:09 [INFO] connect: initialized primary datacenter CA with provider "consul"
    2019/01/16 19:13:09 [DEBUG] consul: Skipping self join check for "f59abb77818b" since the cluster is too small
    2019/01/16 19:13:09 [INFO] consul: member 'f59abb77818b' joined, marking health alive
    2019/01/16 19:13:09 [DEBUG] agent: Skipping remote check "serfHealth" since it is managed automatically
    2019/01/16 19:13:09 [INFO] agent: Synced node info
    2019/01/16 19:13:12 [DEBUG] agent: Skipping remote check "serfHealth" since it is managed automatically
    2019/01/16 19:13:12 [DEBUG] agent: Node info in sync
    2019/01/16 19:13:12 [DEBUG] agent: Node info in sync</pre>
```

As shown in the log a HTTP server is available at&nbsp;`http://172.17.0.2:8500`. Connecting to this IP address and port via web browser opens the Consul Web UI.

<figure id="attachment_6278" aria-describedby="caption-attachment-6278" style="width: 1024px" class="wp-caption aligncenter">[<img loading="lazy" class="size-large wp-image-6278" src="https://www.stefreitag.de/wp/wp-content/uploads/2019/01/Screenshot_20190116_201603-1024x321.png" alt="Consul Web UI" width="1024" height="321" />](https://www.stefreitag.de/wp/wp-content/uploads/2019/01/Screenshot_20190116_201603.png)<figcaption id="caption-attachment-6278" class="wp-caption-text">Consul Web UI</figcaption></figure>

## Preparing the Spring Boot application

First of all a new dependency needs to be added to the <a href="https://gradle.org/" target="_blank" rel="noopener">Gradle</a> project.

<pre class="lang:sh decode:true" title="Adding dependency to Spring Boot project">implementation('org.springframework.cloud:spring-cloud-starter-consul-discovery:2.0.0.RELEASE')</pre>

Next the information required to connect to the service registry/ Consul is entered in the `application.properties` or its <a href="https://en.wikipedia.org/wiki/YAML" target="_blank" rel="noopener">YAML</a> counterpart. The values to enter can be found in the container log.

<pre class="lang:default decode:true" title="Information on the Consul service connection">spring.cloud.consul.host=172.17.0.2
spring.cloud.consul.port=8500</pre>

Additionally the application name is specified. Spring Boot uses this as default value for the service name to register in Consul.

<pre class="lang:default decode:true" title="Setting application name in application.properties">spring.application.name=hello-consul
</pre>

That&#8217;s it. The Spring Boot application is ready for execution. In the logs a line related to Consul should be present. In my case it looks like

<pre class="lang:default decode:true" title="Snippet of the Spring Boot application log">2019-01-18 21:41:20,373 INFO  [main] org.springframework.cloud.consul.serviceregistry.ConsulServiceRegistry: Registering service with consul: NewService{id='hello-consul-8081', name='hello-consul', tags=[secure=false], address='stefan-ThinkPad-E570.fritz.box', port=8081, enableTagOverride=null, check=Check{script='null', interval='10s', ttl='null', http='http://stefan-ThinkPad-E570.fritz.box:8081/actuator/health', tcp='null', timeout='null', deregisterCriticalServiceAfter='null', tlsSkipVerify=null, status='null'}, checks=null}
</pre>

Time to refresh the Consul Web UI:

<figure id="attachment_6290" aria-describedby="caption-attachment-6290" style="width: 1024px" class="wp-caption aligncenter">[<img loading="lazy" class="size-large wp-image-6290" src="https://www.stefreitag.de/wp/wp-content/uploads/2019/01/Screenshot_20190118_215659-1024x369.png" alt="Consul Web UI with Service" width="1024" height="369" />](https://www.stefreitag.de/wp/wp-content/uploads/2019/01/Screenshot_20190118_215659.png)<figcaption id="caption-attachment-6290" class="wp-caption-text">Consul Web UI with Service</figcaption></figure>

Whow, that was easy! The detail information on the service hello-consul shows an issue. This is caused by my setup: the consul service can not reach the endpoint defined in the healthcheck (`http://stefan-ThinkPad-E570.fritz.box:8081/actuator/health`).

<figure id="attachment_6292" aria-describedby="caption-attachment-6292" style="width: 300px" class="wp-caption aligncenter">[<img loading="lazy" class="wp-image-6292 size-medium" src="https://www.stefreitag.de/wp/wp-content/uploads/2019/01/Screenshot_20190118_215837-300x286.png" alt="Servce Details in Consul" width="300" height="286" />](https://www.stefreitag.de/wp/wp-content/uploads/2019/01/Screenshot_20190118_215837.png)<figcaption id="caption-attachment-6292" class="wp-caption-text">Servce Details in Consul</figcaption></figure>

With the service now available in the service registry&#8230; how can consumers discover it? This will be answered in one of the next posts.&nbsp;