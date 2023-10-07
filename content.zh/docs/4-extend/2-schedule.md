---
weight: 2
title: 定时调度
---

# 定时调度

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/weplanx/schedule/release.yml?label=release&style=flat-square)](https://github.com/weplanx/schedule/actions/workflows/release.yml)
[![Release](https://img.shields.io/github/v/release/weplanx/schedule.svg?style=flat-square&include_prereleases)](https://github.com/weplanx/schedule/releases)
[![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/weplanx/schedule?style=flat-square)](https://github.com/weplanx/schedule)
[![Go Report Card](https://goreportcard.com/badge/github.com/weplanx/schedule?style=flat-square)](https://goreportcard.com/report/github.com/weplanx/schedule)
[![GitHub license](https://img.shields.io/github/license/weplanx/schedule?style=flat-square)](https://raw.githubusercontent.com/weplanx/schedule/main/LICENSE)

定时调度消息事件发布节点

## 先决条件

- Nats 集群需启用 JetStream
- 服务与应用要在同个命名空间下一起工作
- 每个节点定义 NODE 命名是唯一的，调度将通过它分配给对应节点

## 部署

定时调度服务原理是将任务消息同步发布给工作节点集群，多节点可提高任务数量与容灾

![](/images/extend/schedule.png)

主要的容器镜像有：

- ghcr.io/weplanx/schedule:latest
- registry.cn-shenzhen.aliyuncs.com/weplanx/schedule:latest

该案例将使用 Kubernetes 部署编排，复制部署（根据需要进行修改）。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: schedule-0
spec:
  selector:
    matchLabels:
      app: schedule
  template:
    metadata:
      labels:
        app: schedule
    spec:
      containers:
        - image: registry.cn-shenzhen.aliyuncs.com/weplanx/schedule:latest
          imagePullPolicy: Always
          name: schedule
          env:
            - name: MODE
              value: release
            - name: NODE
              value: "0"
            - name: NAMESPACE
              value: <*** your namespace ***>
            - name: NATS_HOSTS
              value: <*** your nats hosts ***>
            - name: NATS_NKEY
              value: <*** your nats nkey***>
```

## 环境变量

### MODE

- 工作模式，默认 `debug`

### NODE <font color="red">*必须</font>

- 节点命名

### NAMESPACE <font color="red">*必须</font>

- 应用命名空间，在同个 Nats 集群中命名必须唯一

### NATS_HOSTS <font color="red">*必须</font>

- Nats 的连接主机，多台使用`,`分割

### NATS_NKEY <font color="red">*必须</font>

- Nats 的 NKEY 鉴权密钥

# 客户端

用于管理调度服务配置的客户端，在应用程序中安装：

```shell
go get github.com/weplanx/schedule
```

## 初始化

```golang
// Create the nats client and then create the jetstream context
if js, err = nc.JetStream(nats.PublishAsyncMaxPending(256)); err != nil {
    panic(err)
}

// Create the schedule client
if x, err = client.New(
    client.SetNamespace("example"),
    client.SetNats(nc),
    client.SetJetStream(js),
    client.SetNode("1"),
); err != nil {
    panic(err)
}
```

## 设置调度

```golang
err := x.Set("api", common.ScheduleOption{
    Status: false,
    Jobs: []common.ScheduleJob{
        {
            Mode: "HTTP",
            Spec: "*/5 * * * * *",
            Option: common.HttpOption{
                Url: "https://api.example.com",
            },
        },
    },
})
```

## 状态

```golang
r, err := x.Ping()
```

## 列出所有调度标识

```golang
keys, err := x.Lists()
```

## 获取调度详情

```golang
jobs, err := x.Get("api")
```

## 启用或停用调度

```golang
err := x.Status("api", true)
err := x.Status("api", false)
```

## 移除调度

```golang
err := x.Remove("api")
```

# License

[BSD-3-Clause License](https://github.com/weplanx/schedule/blob/main/LICENSE)
