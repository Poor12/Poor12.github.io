---
layout:     post
title:      多集群联邦项目对比
date:       2023-08-30
catalog: 	true
tags:
    - 集群联邦
---

本文对比主流的多集群联邦项目，分析他们的功能优劣势。

*Note*: 各项目调研的版本如下：Kubeadmiral(截止至9.1的main分支), Karmada(release-1.7), OCM(v0.9.0), Clusternet(v0.11.0)

| 功能\项目           | Kubeadmiral                     | Karmada                                                                       | OCM                          | Clusternet                          |
|-----------------|---------------------------------|-------------------------------------------------------------------------------|------------------------------|-------------------------------------|
| 集群注册多模式         | N                               | Y                                                                             | N，仅支持Pull模式                  | Y                                   |
| 聚合API管理多集群工作负载  | N                               | Y                                                                             | Y，依赖社区的Cluster-gateway插件     | Y                                   |
| 实例拆分调度          | Y                               | Y                                                                             | N                            | Y                                   |
| 依赖下发            | Y                               | Y                                                                             | N                            | N                                   |
| 集群亲和性调度         | Y                               | Y                                                                             | Y                            | Y                                   |
| 差异化策略           | Y, jsonpath                     | Y, 封装了常用的Overriders                                                           | N                            | Y, jsonpath, 支持对于Helm Chart的value覆盖 |
| 状态收集            | Y，支持收集到collectedStatus          | Y，支持收集到Work和Rb                                                                | Y，支持收集到manifestWork的status字段 | Y, 支持收集到manifestStatus              |
| 状态聚合            | Y，支持聚合到资源模版                     | Y，通过资源解析器框架聚合到资源模版                                                            | N                            | Y，支持聚合到AggregatedStatus自定义的CRD中     |
| 联邦资源配额          | N                               | Y                                                                             | N                            | N                                   |
| 命令行工具支持         | N                               | Y                                                                             | Y                            | Y                                   |
| 多集群服务支持         | N                               | Y，支持上游的mcs-api，且实现了单独的MultiClusterService和MultiClusterIngress API对接lb和ingress | N                            | Y，支持上游的mcs-api                      |
| 集群组支持           | N                               | Y，调度时提供集群组调度的语义                                                               | Y，提供ManagedClusterSet API定义  | Y，调度时提供集群组调度的语义                     |
| Helm chart分发    | N                               | 结合Flux CD                                                                     | N                            | Y，自己定义了用于Helm Chart的CRD             |
| 多集群插件           | N                               | N                                                                             | Y，自研插件框架，社区提供了若干常用插件的实现      | N                                   |
| 重调度             | 支持可配置的重调度行为                     | 不可配置，默认在集群Generation变化时重新加入队列                                                 | N                            | N                                   |
| 可扩展的调度器框架       | Y，支持插件扩展，支持SchedulerProfile配置插件 | Y，支持插件扩展，支持同时配置多调度器                                                           |                              |                                     |
| 默认调度能力          |                                 |                                                                               |                              |                                     |
| 集群故障迁移          |                                 | Y                                                                             | Y，插件，额外安装                    |                                     |
| 应用故障迁移          |                                 |                                                                               |                              | N                                   |
| 支持Cluster API   |                                 |                                                                               |                              |                                     |
| 多云搜索            |                                 |                                                                               |                              |                                     |
| 多集群HPA          |                                 |                                                                               |                              |                                     |
| 策略的高阶使用         |                                 |                                                                               |                              |                                     |
| 周边项目集成          |                                 |                                                                               |                              |                                     |
| 可扩展的集群声明        |                                 |                                                                               |                              |                                     |
| 权限管理            |                                 |                                                                               |                              |                                     |
| K8s原生API支持(CRD) |                                 |                                                                               |                              |                                     |
| 成员集群资源管理策略      |                                 |                                                                               |                              |                                     |

