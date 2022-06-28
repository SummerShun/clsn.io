---
title: "Prometheus概述"
date: 2022-06-28T18:50:01+08:00
draft: true
author: 惨绿少年
type: post
Baidusubmit:
  - 1
categories:
  - 监控方案
  - Prometheus

---

### 概述
Prometheus是一套开源的监控、报警、时间序列数据库的组合，起始是由SoundCloud公司开发的。从2016年加入CNCF，2016年6月正式发布1.0版本，2017年底发布了基于全新存储层的2.0版本，能更好地与容器平台、云平台配合，到2018年8月毕业，现在已经成为Kubernetes的官方监控方案，社区活跃，第三方集成非常丰富。
   
### 特点

  Prometheus是一个开源的完整监控解决方案，其对传统监控系统的测试和告警模型进行了彻底的颠覆，形成了基于中央化的规则计算、统一分析和告警的新模型。 相比于传统监控系统Prometheus具有以下优点：

* **易于管理**：只有一个单独的二进制文件，不存在任何的第三方依赖，采用Pull的方式拉取数据

* **强大的数据模型**：每一条时间序列由指标名称(Metrics Name)以及一组标签(Labels)唯一标识
* **强大的查询语言PromQL**：内置了一个强大的数据查询语言PromQL，可以实现多种查询、聚合

* **高性能**：单实例可以处理数以百万的监控指标、每秒处理数十万的数据点
* **易扩展**：支持sharding和联邦集群，实现多数据中心
* **易集成**：支持多种语言的SDK进行应用程序数据埋点，社区有丰富插件
* **可视化**：自带Prometheus UI，可以进行查询与展示，Grafana也完整支持Prometheus。
* **开放性**：使用sdk采集的数据可以被其他监控系统使用，不一定非要用Prometheus
   
### 文章汇总
Prometheus目前已经成为了官方推荐的监控方案，HPA也支持其自定义的监控数据，为了方便部署管理，CoreOS也推出了[prometheus-operator](https://github.com/coreos/prometheus-operator)，将CRD引入其中，社区发展非常迅速，云厂商和主流公司也做了引入和支持。

Prometheus是一个完整的监控系统，内容很多，后续文章将结合实际场景，分开阐述各种用法：

* [Prometheus基本架构](https://github.com/yasongxu/container-monitor/blob/master/opensource/agent/prometheus/Prometheus%E5%9F%BA%E6%9C%AC%E6%9E%B6%E6%9E%84.md)
* [Prometheus部署方案](https://github.com/yasongxu/container-monitor/blob/master/opensource/agent/prometheus/Prometheus%E9%83%A8%E7%BD%B2%E6%96%B9%E6%A1%88.md)
* [Prometheus配置与服务发现](https://github.com/yasongxu/container-monitor/blob/master/opensource/agent/prometheus/Prometheus%E7%9A%84%E9%85%8D%E7%BD%AE%E4%B8%8E%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0.md)
* [PromQL查询解析](https://github.com/yasongxu/container-monitor/blob/master/opensource/agent/prometheus/PromQL%E6%9F%A5%E8%AF%A2%E8%A7%A3%E6%9E%90.md)
* Prometheus数据可视化
* Prometheus数据持久化
* Alertmanager告警处理
* Prometheus高可用方案
* Exporter推荐
* Prometheus Operator详解
* 核心组件原理分析

本文为容器监控实践系列文章，完整内容见：[container-monitor-book](https://yasongxu.gitbook.io/container-monitor/)

> 转载自 
> https://yasongxu.gitbook.io/container-monitor/ 

