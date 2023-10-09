---
weight: 2
title: Schedule
---

# Schedule

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/weplanx/schedule/release.yml?label=release&style=flat-square)](https://github.com/weplanx/schedule/actions/workflows/release.yml)
[![Release](https://img.shields.io/github/v/release/weplanx/schedule.svg?style=flat-square&include_prereleases)](https://github.com/weplanx/schedule/releases)
[![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/weplanx/schedule?style=flat-square)](https://github.com/weplanx/schedule)
[![Go Report Card](https://goreportcard.com/badge/github.com/weplanx/schedule?style=flat-square)](https://goreportcard.com/report/github.com/weplanx/schedule)
[![GitHub license](https://img.shields.io/github/license/weplanx/schedule?style=flat-square)](https://raw.githubusercontent.com/weplanx/schedule/main/LICENSE)

Schedule message event publishing node

## Pre-requisite

- Nats cluster needs to enable JetStream
- Services and applications should work together the same namespace
- Each node defines a NODE name that is unique, through which Schedule will be assigned to the node

## Deploy

The principle of the Schedule service is to publish the job message sync up to the worker node cluster. 
Multi-node can improve the number of tasks and disaster recovery.

![](/images/extend/schedule.png)

The main container image is:

- ghcr.io/weplanx/schedule:latest
- registry.cn-shenzhen.aliyuncs.com/weplanx/schedule:latest

The case will use Kubernetes deployment orchestration, replicate deployment (modify as needed).

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

## Environement

### MODE

- Working mode, default `debug`

### NODE <font color="red">*required</font>

- 节点命名

### NAMESPACE <font color="red">*required</font>

- namespace, the name must be unique within the same Nats cluster

### NATS_HOSTS <font color="red">*required</font>

- Nats connection host, use `,` split

### NATS_NKEY <font color="red">*required</font>

- Nats NKEY authentication

# Client

The client end for managing Schedule configuration, installed in the application:

```shell
go get github.com/weplanx/schedule
```

## Initialize

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

## Set

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

## Status

```golang
r, err := x.Ping()
```

## List Keys

```golang
keys, err := x.Lists()
```

## Get Info

```golang
jobs, err := x.Get("api")
```

## Start or Stop

```golang
err := x.Status("api", true)
err := x.Status("api", false)
```

## Remove

```golang
err := x.Remove("api")
```

# License

[BSD-3-Clause License](https://github.com/weplanx/schedule/blob/main/LICENSE)
