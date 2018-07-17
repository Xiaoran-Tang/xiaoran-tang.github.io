---
layout:             post
title:                 Elasticsearch
subtitle:           
date:      	        2018-07-17
author:             Shaw
header-img:     img/post-bg-debug.png
catalog: 	         true
tags:
        - 数据挖掘
---
>本文参考自 [Elasticsearch 架构以及源码概览](https://www.jianshu.com/p/24b90cecf161)

Elasticsearch 是最近两年异军突起的一个兼有搜索引擎和 NoSQL 数据库功能的开源系统，基于 Java/Lucene 构建。

Elasticsearch 看名字就能大概了解下它是一个弹性的搜索引擎。首先弹性隐含的意思是分布式，单机系统是没法弹起来的，然后加上灵活的伸缩机制，就是这里的 Elastic 包含的意思。它的搜索存储功能主要是 Lucene 提供的，Lucene 相当于其存储引擎，它在之上封装了索引，查询，以及分布式相关的接口。


![](https://raw.githubusercontent.com/xiaoran-tang/xiaoran-tang.github.io/master/img/Elasticsearch架构.png)

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
