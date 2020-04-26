---
title: >-
  CentOS8 Docker Compose容器互联出现java.net.NoRouteToHostException: Host is unreachable
toc: true
date: 2020.02.19 19:17:52
tags:
---
今天在CentOS8下使用Docker Compose部署Spring Cloud项目时出现Host is unreachable问题，在网上找寻半天，终于找到一个场景类似的问题（[https://www.cnblogs.com/jojo-feed/p/10669296.html](https://www.cnblogs.com/jojo-feed/p/10669296.html)）。

这个问题有几种出现的场景：

>1.因为CentOS7以后使用了firewall，而不是iptables，docker默认网络配置是桥接，会建立一个docker0网桥，并会为docker0创建iptables的规则，而不会为firewall创建规则，但是这种情况只适用于直接使用docker，而不是使用docker compose的情况（大部分人都是这个场景，网上答案也大多数是这个场景）
2.使用docker compose ，使用docker compose的时候，每个docker-compose项目，默认都会创建一个网桥，这时候必须单独为该网桥设置firewall规则。
