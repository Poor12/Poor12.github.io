---
layout:     post
title:      多集群联邦项目对比
date:       2023-08-30
catalog: 	true
tags:
    - 集群联邦
---

本文对比主流的多集群联邦项目，分析他们的功能优劣势。

*Note*: 各项目调研的版本如下：Kubeadmiral(截止至9.1的main分支), Karmada(release-1.7), OCM(v0.11.0), Clusternet(v0.16.0)

| 功能\项目           | Kubeadmiral                     | Karmada                                                                       | OCM                                               | Clusternet                          |
|-----------------|---------------------------------|-------------------------------------------------------------------------------|---------------------------------------------------|-------------------------------------|
| 集群注册多模式         | N                               | Y, Pull模式基于bootstrap token的注册方式                                               | N，仅支持Pull模式，支持基于bootstrap token的注册方式              | Y                                   |
| 聚合API管理多集群工作负载  | N                               | Y，提供两套多集群API，aggregated API需要在endpoint上指定集群名，Proxy API不需要                     | Y，依赖社区的Cluster-gateway插件，类似Karmada的Aggregated API | Y                                   |
| 实例拆分调度          | Y                               | Y                                                                             | N                                                 | Y                                   |
| 依赖下发            | Y                               | Y                                                                             | N                                                 | N                                   |
| 集群亲和性调度         | Y                               | Y                                                                             | Y                                                 | Y                                   |
| 差异化策略           | Y, jsonpath                     | Y, 封装了常用的Overriders                                                           | N                                                 | Y, jsonpath, 支持对于Helm Chart的value覆盖 |
| 状态收集            | Y，支持收集到collectedStatus          | Y，支持收集到Work和Rb                                                                | Y，支持收集到manifestWork的status字段                      | Y, 支持收集到manifestStatus              |
| 状态聚合            | Y，支持聚合到资源模版                     | Y，通过资源解析器框架聚合到资源模版                                                            | N                                                 | Y，支持聚合到AggregatedStatus自定义的CRD中     |
| 联邦资源配额          | N                               | Y                                                                             | N                                                 | N                                   |
| 命令行工具支持         | N                               | Y                                                                             | Y                                                 | Y                                   |
| 多集群服务支持         | N                               | Y，支持上游的mcs-api，且实现了单独的MultiClusterService和MultiClusterIngress API对接lb和ingress | N                                                 | Y，支持上游的mcs-api                      |
| 集群组支持           | N                               | Y，调度时提供集群组调度的语义                                                               | Y，提供ManagedClusterSet API定义                       | Y，调度时提供集群组调度的语义                     |
| Helm chart分发    | N                               | 结合Flux CD                                                                     | N                                                 | Y，自己定义了用于Helm Chart的CRD             |
| 多集群插件           | N                               | N                                                                             | Y，自研插件框架，支持插件的全生命周期管理，社区提供了若干常用插件的实现              | N                                   |
| 重调度             | 支持可配置的重调度行为                     | 不可配置，默认在资源模版、策略以及集群Generation变化时重新加入队列                                        | 默认在资源模版、Placement变化时重调度                           | 默认在资源模版、Placement变化时重调度             |
| 可扩展的调度器框架       | Y，支持插件扩展，支持SchedulerProfile配置插件 | Y，支持插件扩展，支持同时配置多调度器                                                           | Y，支持插件扩展                                          | Y，支持插件扩展                            |
| 集群故障迁移          | N                               | Y                                                                             | Y，插件，额外安装                                         | Y                                   | N
| 应用故障迁移          | Y，支持实例级别的故障迁移                   | Y，支持应用和实例的故障迁移，实例的故障迁移基于Descheduler，应用的故障迁移基于资源解析器                            | N                                                 | N                                   |
| 支持上游Cluster API | N                               | Y                                                                             | N                                                 | Y                                   |
| 多云搜索            | N                               | Y, 插件，额外安装                                                                    | N                                                 | N                                   |
| 多集群HPA          | N                               | Y，支持FederatedHPA和CronFederatedHPA                                             | N                                                 | N                                   |
| 策略的高阶使用         | 不涉及，策略直接通过label关联负载             | 支持pp的优先级声明以及抢占                                                                | 不涉及，ManifestWork直接关联Work                          | 支持OverridePolicy的优先级                |
| 周边项目集成          | Argo Workflow                   | Argo/Flux CD，OpenKruise，Istio等                                                | Argo CD，Istio                                     | Submariner                          |
| 可扩展的集群声明        | N                               | N                                                                             | Y                                                 | N                                   |
| 权限管理            | N                               | Impersonate实现统一认证                                                             | 依赖插件managed-serviceaccount统一管理                    | N                                   |
| K8s原生API支持(CRD) | 依赖FTC                           | Y                                                                             | Y                                                 | Y                                   |
| 成员集群资源管理策略      | 通过FTC配置资源的路径                    | 通过资源解析器解决联邦与成员集群的冲突                                                           | 通过UpdateStrategy配置更新策略                            | N                                   |

