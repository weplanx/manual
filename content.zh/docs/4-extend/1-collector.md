---
weight: 1
title: 收集服务
---

# 收集服务

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/weplanx/collector/release.yml?label=release&style=flat-square)](https://github.com/weplanx/collector/actions/workflows/release.yml)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/weplanx/collector/testing.yml?label=testing&style=flat-square)](https://github.com/weplanx/collector/actions/workflows/testing.yml)
[![Release](https://img.shields.io/github/v/release/weplanx/collector.svg?style=flat-square&include_prereleases)](https://github.com/weplanx/collector/releases)
[![Coveralls github](https://img.shields.io/coveralls/github/weplanx/collector.svg?style=flat-square)](https://coveralls.io/github/weplanx/collector)
[![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/weplanx/collector?style=flat-square)](https://github.com/weplanx/collector)
[![Go Report Card](https://goreportcard.com/badge/github.com/weplanx/collector?style=flat-square)](https://goreportcard.com/report/github.com/weplanx/collector)
[![GitHub license](https://img.shields.io/github/license/weplanx/collector?style=flat-square)](https://raw.githubusercontent.com/weplanx/collector/main/LICENSE)

分布式的轻量队列流收集服务

## 先决条件

- Nats 集群需启用 JetStream 
- MongoDB 建议版本 >= 5.0，以便可以使用时间序列集合
- 服务与应用要在同个 NATS 租户下一起工作

## 部署

用于订阅流队列然后写入数据集合的收集服务。

![](/images/extend/collector.png)

{{<hint info>}}

若使用时序集合，需手动创建数据库再新增数据流，将时序集合时间字段设置为 `timestamp` 元数据字段设置为 `metaField`，Nats Stream 的命名 `COLLECT_${key}` 对应数据库名 `${key}`。

{{</hint>}}

主要的容器镜像有：

- ghcr.io/weplanx/collector:latest
- registry.cn-shenzhen.aliyuncs.com/weplanx/collector:latest

该案例将使用 Kubernetes 部署编排，复制部署（根据需要进行修改）。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: collector
spec:
  selector:
    matchLabels:
      app: collector
  template:
    metadata:
      labels:
        app: collector
    spec:
      containers:
        - image: registry.cn-shenzhen.aliyuncs.com/weplanx/collector:latest
          imagePullPolicy: Always
          name: collector
          env:
            - name: MODE
              value: release
            - name: NATS_HOSTS
              value: <*** your nats hosts ***>
            - name: NATS_NKEY
              value: <*** your nats nkey***>
            - name: DATABASE_URL
              value: <*** your mongodb url ***>
            - name: DATABASE_NAME
              value: <*** your mongodb name ***>
```

## 环境变量

### MODE

- 工作模式，默认 `debug`

### NATS_HOSTS <font color="red">*必须</font>

- Nats 的连接主机，多台使用`,`分割

### NATS_NKEY <font color="red">*必须</font>

- Nats 的 NKEY 鉴权密钥

### DATABASE_URL <font color="red">*必须</font>

- MongoDB 的连接地址

### DATABASE_NAME <font color="red">*必须</font>

- MongoDB 的数据库名称

# 客户端

用于管理收集服务配置、数据传输和调度分发的客户端，在应用程序中安装：

```shell
go get github.com/weplanx/collector
```

## 初始化

```golang
// Create the nats client and then create the jetstream context
if js, err = nc.JetStream(nats.PublishAsyncMaxPending(256)); err != nil {
    panic(err)
}

// Create the transfer client
if x, err = client.New(js); err != nil {
    panic(err)
}
```

## 设置数据流

```golang
err := x.Set(context.TODO(), client.StreamOption{
    Key:         "system",
    Description: "system example",
})
```

## 更新数据流

```golang
err := x.Update(context.TODO(), client.StreamOption{
    Key:         "system",
    Description: "system example 123",
})
```

## 获取数据流详情

```golang
result, err := client.Get("system")
```

## 发布数据

```golang
err := x.Publish(context.TODO(), "system", client.Payload{
    Timestamp: time.Now(),
    Data: map[string]interface{}{
        "metadata": map[string]interface{}{
            "method":    method,
            "path":      string(c.Request.Path()),
            "user_id":   userId,
            "client_ip": c.ClientIP(),
        },
        "params":     string(c.Request.QueryString()),
        "body":       c.Request.Body(),
        "status":     c.Response.StatusCode(),
        "user_agent": string(c.Request.Header.UserAgent()),
    },
    XData: map[string]interface{}{},
})
```

## 移除数据流

```golang
err := x.Remove("system")
```

# License

[BSD-3-Clause License](https://github.com/weplanx/collector/blob/main/LICENSE)
