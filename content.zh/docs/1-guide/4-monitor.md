---
weight: 4
title: 启用监控
---

# 启用监控

Weplanx 采用 [InfluxDB OSS v2](https://docs.influxdata.com/influxdb/v2/) 与 [Telegraf](https://docs.influxdata.com/telegraf/v1/) 进行服务监控日志的存储与采集，管理后台中已经内置相关的查询接口，只需要部署采集即可

## 创建配置

建议以 1 分钟为间隔进行收集，如使用公有云 MongoDB 和 Redis 无需再收集他们。

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: influx-system
  name: xxx-devops
data:
  telegraf.conf: |
    [agent]
      interval = "1m"
      round_interval = true
      metric_batch_size = 1000
      metric_buffer_limit = 10000
      collection_jitter = "0s"
      flush_interval = "1m"
      flush_jitter = "0s"
      precision = ""
      omit_hostname = false
    
    [[outputs.influxdb_v2]]
      urls = ["http://influxdb.influx-system.svc:8086"]
      token = "$INFLUX_TOKEN"
      organization = "example"
      bucket = "xxx-devops"
    
    [[inputs.mongodb]]
      servers = ["mongodb://xxx:xxx@mongodb.mongodb-system.svc:27017/?connect=direct"]
    
    [[inputs.redis]]
      servers = ["tcp://:xxx@redis.redis-system.svc:6379"]
    
    [[inputs.nats]]
      server = "http://nats.nats-system.svc:8222"
```

## 创建收集器

reload 是一个监听配置文件并自动更新部署的组件，具体查看 https://github.com/stakater/Reloader

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: influx-system
  name: xxx-devops
  annotations:
    configmap.reloader.stakater.com/reload: "xxx-devops"
spec:
  selector:
    matchLabels:
      app: xxx-devops
  template:
    metadata:
      labels:
        app: xxx-devops
    spec:
      containers:
        - image: telegraf:1.28-alpine
          imagePullPolicy: Always
          name: telegraf
          env:
            - name: INFLUX_TOKEN
              value: xxx
          volumeMounts:
            - name: config
              mountPath: /etc/telegraf
              readOnly: true
      volumes:
        - name: config
          configMap:
            name: xxx-devops
```