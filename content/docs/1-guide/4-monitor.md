---
weight: 4
title: Enable Monitor
---

# Enable Monitor

Weplanx adopts [InfluxDB OSS v2](https://docs.influxdata.com/influxdb/v2/) and [Telegraf](https://docs.influxdata.com/telegraf/v1/) management background has built-in related query interface, only need to deploy the collection can be

## Apply ConfigMap

It is recommended to collect them at 1 minute intervals, such as using public clouds MongoDB and Redis without collecting them.

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

## Apply Telegraf

Reload is a component that listens to configuration files and automatically updates deployments. For details, see https://github.com/stakater/Reloader

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