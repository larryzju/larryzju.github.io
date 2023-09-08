---
title: K8s Leader Election 小记
layout: post
tags: k8s 计算机
---

Leader Election 在 k8s 中常用于实现多副本 HA 的场景。

不同于 load balancer 实现水平扩展。Leader Election 适应于同一时刻只能有一个活跃进程的单 active 多 standby 实现。例如 k8s controller。

多个 candidates 竞争 leader 角色，leader 周期性 renew lease。一旦失败，则有其它 candidate 顶上。

## Lease resource

k8s program 通过 k8s resource (configmap, endpoint 或者 lease CR) 进行 leader 选举。

Lease 有五个字段

1.  acquireTime: 当前 leader 首次获得管理权的时间
2.  renewTime: 当前 leader 最后一次续约的时间
3.  holderIdentity: 当前 leader 标识
4.  leaseDurationSeconds: 有效时长
5.  leasetransitions: 共经过了多少次 leader 切换

默认情况 leader/candidate 2s 一次续租/竞选。leader 15s 不更新则丢失 leader 身份。

实际实现有五种方式（controller 用 Leases CR）。对外统一以
`resourcelock.Interface` 接口提供 create/update/get 及 event send 功能。

1.  configmaps（废弃）
2.  endpoints （废弃）
3.  leases
4.  endpointsLease (endpoint + lease)
5.  configmapsLease (configmap + lease)

## client go

`k8s.io/client-go/tools/leaderelection` 中实现了 Leader Election 功能。

LeaderElector 结构有 `Run` 或 `RunOrDie` 方法。获取 leader 并 renew 之。

1.  循环尝试，2s 一次，直到成为 leader。
    1.  lease 不存在则创建之，直接成为 leader
    2.  已经存在，leader 是别人，且未过期，则等待下一次重试
    3.  否则成为 leader，更新 acquire time
2.  回调 OnStartedLeading
3.  renew leader。直到丢弃 leader，释放之 (release)
4.  回调 OnStoppedLeading

此外

- 当 leader 发生变化时回调 OnNewLeader
- lease `leaderTransition` 记录了 leader 切换次数

## controller runtime

Controller 启动时需要指定 `LeaderElectionID` ，对应于 leases CR 名称。

如果 Controller 以多副本运行，每个 controller pod 都生成随机 uuid，以
`${HOST}_${UUID}` 作为 identity 来竞争 leader。

只有获得 leader 身份的 controller 才会真正 reconcile CR。

Leader Election 功能可以禁用。方便本地开发。
