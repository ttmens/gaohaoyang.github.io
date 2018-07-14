---
layout: post
title:  "Agile、CI/CD、DevOps关系与实践"
date:   2018-07-14 22:36:54
categories: work
tags: software CI/CD
---

* content
{:toc}

最近一直在做CI／CD相关的工具，在各个模块均达到了一定的自动化程度。本文结合Agile以及DevOps简单说明一下它们之间的关系。进一步，将分享一下我们的CI／CD是如何实现的。




## Agile、CI/CD、DevOps关系

之前一直将Agile、CI/CD、以及DevOps混在一起讨论，最近看到网上的一张图，很简明的阐述了他们之间的关系。在原图的基础上，进一步加入了CI/CD中持续交付和持续部署的关系。

![image1](http://p92xby2e1.bkt.clouddn.com/ci-cd-1.png)

## CI／CD实践

日常工作之一，是给XXXX自动化工厂搭建CI/CD平台。原有的工具平台繁杂混乱，例如，管理worker集群的工具存在两种：ansible和puppet。结合实际情况，整合冗余工具后，搭建如下图所示CI/CD工具链。

![image1](http://p92xby2e1.bkt.clouddn.com/ci-cd-2.png)

整个工具链通过jenkins串起来，对外提供的能力主要包括如下几个方面：

1. CI层面，对外提供自动编译、代码静态检查、以及基础的web测试功能

2. CD层面，对外提供自动化的docker镜像打包、以及发布部署功能

## 后续的一些思考以及功能的演进

在实际的构建过程中，CI／CD之间并不能完全分开，例如在CI中包含的测试功能，是建立在CD将代码打包部署的基础上的。

同样，对于持续部署，由于XXXX自动化工厂的特殊性，其属于C／S架构，同时在C端存在大量的硬件在构建任务，后台服务无法像web服务那样，对外提供统一的功能。所以，在使用docker镜像时，并不能同时启多个容器，去对外提供高可靠性的服务。此时，我们更多的是想着如何构建合适的冷热补丁发布方式，这一部分已经有一些初步的生产环境使用经验，待功能成熟后会进一步分析。
