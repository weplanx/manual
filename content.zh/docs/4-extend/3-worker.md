---
weight: 3
title: 工作触发
---

# 工作触发

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/weplanx/worker/release.yml?label=release&style=flat-square)](https://github.com/weplanx/worker/actions/workflows/release.yml)
[![Release](https://img.shields.io/github/v/release/weplanx/worker.svg?style=flat-square&include_prereleases)](https://github.com/weplanx/worker/releases)
[![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/weplanx/worker?style=flat-square)](https://github.com/weplanx/worker)
[![Go Report Card](https://goreportcard.com/badge/github.com/weplanx/worker?style=flat-square)](https://goreportcard.com/report/github.com/weplanx/worker)
[![GitHub license](https://img.shields.io/github/license/weplanx/worker?style=flat-square)](https://raw.githubusercontent.com/weplanx/worker/main/LICENSE)

分布式的消息事件回调工作节点

## 先决条件

- Nats 集群需启用 JetStream
- 服务与应用要在同个命名空间下一起工作

## 部署

工作节点同时订阅来自定时调度服务、消息队列或第三方事件的工作队列（非持久化），获得消息者触发事件并将结果转入日志流，节点可无限水平扩展分流压力

![](/images/extend/worker.png)

主要的容器镜像有：

- ghcr.io/weplanx/worker:latest
- registry.cn-shenzhen.aliyuncs.com/weplanx/worker:latest

该案例将使用 Kubernetes 部署编排，复制部署（根据需要进行修改）。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: worker
spec:
  selector:
    matchLabels:
      app: worker
  template:
    metadata:
      labels:
        app: worker
    spec:
      containers:
        - image: registry.cn-shenzhen.aliyuncs.com/weplanx/worker:latest
          imagePullPolicy: Always
          name: worker
          env:
            - name: MODE
              value: release
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

### NAMESPACE <font color="red">*必须</font>

- 应用命名空间，在同个 Nats 集群中命名必须唯一

### NATS_HOSTS <font color="red">*必须</font>

- Nats 的连接主机，多台使用`,`分割

### NATS_NKEY <font color="red">*必须</font>

- Nats 的 NKEY 鉴权密钥

# License

[BSD-3-Clause License](https://github.com/weplanx/worker/blob/main/LICENSE)
