---
layout:             post
title:                 Elasticsearch
subtitle:           弹性搜索
date:      	        2018-07-17
author:             Shaw
header-img:     img/post-bg-debug.png
catalog: 	         true
tags:
        - 数据挖掘
---
>本文参考自 [Elasticsearch 架构以及源码概览](https://www.jianshu.com/p/24b90cecf161)、[elasticsearch和lucene的关系](https://blog.csdn.net/qq_15175765/article/details/78861947) 以及 [使用ELK架构收集、存储、分析日志](https://zhuanlan.zhihu.com/p/21333411) 

Elasticsearch 是最近两年异军突起的一个兼有搜索引擎和 NoSQL 数据库功能的开源系统，基于 Java/Lucene 构建。

Elasticsearch 看名字就能大概了解下它是一个弹性的搜索引擎。首先弹性隐含的意思是分布式，单机系统是没法弹起来的，然后加上灵活的伸缩机制，就是这里的 Elastic 包含的意思。它的搜索存储功能主要是 Lucene 提供的，Lucene 相当于其存储引擎，它在之上封装了索引，查询，以及分布式相关的接口。

基本概念
-

中文 | 英文 | 解释
-|-|-
集群 | Cluster |一组拥有共同 cluster name 的节点。
节点 | Node | 集群中的一个 Elasticearch 实例。
索引 | Index | 相当于关系数据库中的 database 概念，一个集群中可以包含多个索引。这是个逻辑概念。
主分片 | Primary shard | 索引的子集，索引可以切分成多个分片，分布到不同的集群节点上。分片对应的是 Lucene 中的索引。
副本分片 | Replica shard | 每个主分片可以有一个或者多个副本。
类型 | Type | 相当于数据库中的 table 概念。Mapping是针对 Type 的。同一个索引里可以包含多个 Type。
格式 | Mapping | 相当于数据库中的 schema，用来约束字段的类型，不过 Elasticsearch 的 mapping 可以自动根据数据创建。
文档 | Document) | 相当于数据库中的 row。
字段 | Field |相当于数据库中的 column。
分配 | Allocation | 将分片分配给某个节点的过程，包括分配主分片或者副本。如果是副本，还包含从主分片复制数据的过程。

Elasticsearch 与 Lucene 的关系
-
Lucene 是当前最先进、功能最强大的搜索库。但是，直接基于 Lucene 进行的开发将会非常复杂：不仅 API 复杂（实现一些简单的功能，需要写大量的Java代码），而且需要深入理解原理（各种索引结构）。

而 Elasticsearch 基于 Lucene，隐藏了复杂性，提供简单易用的 RESTful API 接口、Java API 接口（以及其他语言的api接口）。具体特征有：

（1）分布式的文档存储引擎，支持PB级数据
（2）分布式的搜索引擎和分析引擎
（3）开箱即用，优秀的默认参数，不需要任何额外设置
（4）完全开源


关于 Elasticsearch，有一个传说：有一个程序员失业了，陪着自己妻子去英国伦敦学习厨师课程。程序员在失业期间想给妻子写一个菜谱搜索引擎，觉得 Lucene 实在太复杂了，就开发了一个封装了 Lucene 的开源项目：Compass。后来程序员找到了工作，是做分布式的高性能项目的，觉得 Compass 不够，就写了 Elasticsearch，让 Lucene 变成分布式的系统。

Elasticsearch 与日志的关系
-
在传统的应用运行环境中，收集、分析日志往往是非常必要且常见的，一旦应用出现问题、或者需要分析应用行为时，管理员将通过一些传统工具，手动查看日志进行检查。然而这种做法的适用范围仅限于少量的主机和日志文件类型。

随着应用系统的复杂性与日俱增，日志的分析也越来越重要，常见的需求包括，团队开发过程中可能遇到一些和日志有关的问题：

- 开发没有生产环境服务器权限，需要通过系统管理员获取详细日志，沟通成本高
- 系统可能是由多个不同语言编写、分布式环境下的模块组成，查找日志费时费力
- 日志数量巨大，查询时间很长

对于日志来说，最常见的需求就是收集、查询、显示，正对应 Logstash、ElasticSearch、Kibana 的功能。ELK（ElasticSearch + Logstash + Kibana）架构专为收集、分析和存储日志所设计。同时可以通过开源项目 Kibana 对日志进行查看、搜索、分析、可视化：

![](https://raw.githubusercontent.com/xiaoran-tang/xiaoran-tang.github.io/master/img/Kibana.png)

除了查看日志外，它自身的组件架构支持通过代理对不同服务器的日志流进行管理，并最终传送至ElasticSearch中。

ELK架构
-

![](https://raw.githubusercontent.com/xiaoran-tang/xiaoran-tang.github.io/master/img/ELK.png)

从上图的架构可以看出，Logstash 在各个应用中设置一个 Shipper 用来收集并转化为统一格式的日志数据，然后将其转入 Message Broker (通常是 Redis)，然后日志中心的 Indexer (也是 Logstash)负责从 Broker 中读取日志流，并转存至 ElasticSearch 中建立索引，以便日后快速查看、搜索、分析。Kibana 是一个功能强大、丰富的 Web 界面，它从 ElasticSearch 中读取数据，支持各种查询、展示。