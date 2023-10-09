---
weight: 1
title: Collector
---

# Collector

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/weplanx/collector/release.yml?label=release&style=flat-square)](https://github.com/weplanx/collector/actions/workflows/release.yml)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/weplanx/collector/testing.yml?label=testing&style=flat-square)](https://github.com/weplanx/collector/actions/workflows/testing.yml)
[![Release](https://img.shields.io/github/v/release/weplanx/collector.svg?style=flat-square&include_prereleases)](https://github.com/weplanx/collector/releases)
[![Coveralls github](https://img.shields.io/coveralls/github/weplanx/collector.svg?style=flat-square)](https://coveralls.io/github/weplanx/collector)
[![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/weplanx/collector?style=flat-square)](https://github.com/weplanx/collector)
[![Go Report Card](https://goreportcard.com/badge/github.com/weplanx/collector?style=flat-square)](https://goreportcard.com/report/github.com/weplanx/collector)
[![GitHub license](https://img.shields.io/github/license/weplanx/collector?style=flat-square)](https://raw.githubusercontent.com/weplanx/collector/main/LICENSE)

Distribution lightly queue stream logset service

## Pre-requisite

- Nats cluster needs to enable JetStream
- MongoDB recommends version >= 5.0 so that time series collections can be used
- Services and applications should work together the same namespace

## Deploy

A collector service that subscribes to stream queues and then writes to logset.

![](/images/extend/collector.png)

{{<hint info>}}

If you use the time series collection, you need to manually create a database and then add a log-stream. 
Set the time series collection time field to `timestamp` and metadata field to `metaField`.
Nats Stream naming `${namespace}:logs:${key}` is consistent with database name `${key}`.

{{</hint>}}

The main container image is:

- ghcr.io/weplanx/collector:latest
- registry.cn-shenzhen.aliyuncs.com/weplanx/collector:latest

The case will use Kubernetes deployment orchestration, replicate deployment (modify as needed).

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
            - name: NAMESPACE
              value: <*** your namespace ***>
            - name: NATS_HOSTS
              value: <*** your nats hosts ***>
            - name: NATS_NKEY
              value: <*** your nats nkey***>
            - name: DATABASE_URL
              value: <*** your mongodb url ***>
            - name: DATABASE_NAME
              value: <*** your mongodb name ***>
```

## Environement

### MODE

- Working mode, default `debug`

### NAMESPACE <font color="red">*required</font>

- namespace, the name must be unique within the same Nats cluster

### NATS_HOSTS <font color="red">*required</font>

- Nats connection host, use `,` split

### NATS_NKEY <font color="red">*required</font>

- Nats NKEY authentication

### DATABASE_URL <font color="red">*required</font>

- MongoDB connection address

### DATABASE_NAME <font color="red">*required</font>

- MongoDB database name

# Client

The client for managing collector configuration, data transmission, and dispatching, installed in the application:

```shell
go get github.com/weplanx/collector
```

## Initialize

```golang
// Create the nats client and then create the jetstream context
if js, err = nc.JetStream(nats.PublishAsyncMaxPending(256)); err != nil {
    panic(err)
}

// Create the transfer client
if x, err = client.New(
    client.SetNamespace("example"),
    client.SetJetStream(js),
); err != nil {
    panic(err)
}
```

## Set

```golang
err := x.Set(context.TODO(), client.StreamOption{
    Key:         "system",
    Description: "system example",
})
```

## Update

```golang
err := x.Update(context.TODO(), client.StreamOption{
    Key:         "system",
    Description: "system example 123",
})
```

## Get Info

```golang
result, err := client.Get("system")
```

## Publish

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

## Remove

```golang
err := x.Remove("system")
```

# License

[BSD-3-Clause License](https://github.com/weplanx/collector/blob/main/LICENSE)
