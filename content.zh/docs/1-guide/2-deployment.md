---
weight: 2
title: 应用部署
---

# 应用部署

## 初始化

首先要上线相关的扩展服务，命名空间必须是一致的，例如：example，为 MongoDB 创建数据库 `example`

### [收集服务](/docs/4-extend/1-collector/)

设置你需要连接的数据库和 NATS 集群，也可以为其增加多个 replicas

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
              value: example
            - name: NATS_HOSTS
              value: nats://nats.nats.svc.cluster.local:4222
            - name: NATS_NKEY
              value: <*** your nats nkey***>
            - name: DATABASE_URL
              value: mongodb+srv://example:123456@exp.xxxx.mongodb.net/?readPreference=secondaryPreferred&tls=true&authSource=weplanx
            - name: DATABASE_NAME
              value: example
```

### [定时调度](/docs/4-extend/1-collector/)

新增一个0号定时调度节点，定时调度 NODE 必须保持唯一

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
              value: example
            - name: NATS_HOSTS
              value: nats://nats.nats.svc.cluster.local:4222
            - name: NATS_NKEY
              value: <*** your nats nkey***>
```

### [工作触发](/docs/4-extend/1-collector/)

工作触发节点也可以其增加多个 replicas

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
              value: example
            - name: NATS_HOSTS
              value: nats://nats.nats.svc.cluster.local:4222
            - name: NATS_NKEY
              value: <*** your nats nkey***>
```

### 定义初始配置

新建一个 `ConfigMap`，定义 `default.values.yml`，配置名即动态配置的 **Snake Case**，例如：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: server
data:
  default.values.yml: |
    ip_address: http://service-xxx-xxx.gz.apigw.tencentcs.com/release/v4
    ip_secret_id: ...
    ip_secret_key: ...
    ipv6_address: http://service-xxx-xxx.gz.apigw.tencentcs.com/release/v4
    ipv6_secret_id: ...
    ipv6_secret_key: ...
    sms_secret_id: ...
    sms_secret_key: ...
    sms_app_id: ...
    sms_region: ap-guangzhou
    emqx_host: http://broker-dashboard.emqx.svc.cluster.local:18083/api/v5
    emqx_api_key: ...
    emqx_secret_key: ...
    dynamic_values:
      session_ttl: 1h
      login_ttl: 15m
      login_failures: 5
      ip_login_failures: 10
      ip_whitelist: [ ]
      ip_blacklist: [ ]
      pwd_strategy: 1
      pwd_ttl: 8760h
      cloud: tencent
      tencent_secret_id: ...
      tencent_secret_key: ...
      tencent_cos_bucket: public-...
      tencent_cos_region: ap-guangzhou
      tencent_cos_expired: 300
      tencent_cos_limit: 102400
      lark_app_id: ...
      lark_app_secret: ...
      lark_encrypt_key: ...
      lark_verification_token: ...
      redirect_url: https://api.xxx.com/lark
      email_host: smtp.larksuite.com
      email_port: 465
      email_username: ...
      email_password: ...
      rest_controls:
        users:
          keys: [ "_id", "email", "name", "avatar", "status", "phone", "totp", "lark", "sessions", "history", "create_time", "update_time" ]
          sensitives: ["phone", "totp", "lark"]
          status: true
        projects:
          status: true
        pictures:
          status: true
        videos:
          status: true
        categories:
          status: true
        clusters:
          keys: ["_id", "name", "kind", "create_time", "update_time"]
          status: true
        schedules:
          status: true
          event: true
        workflows:
          status: true
          event: true
        queues:
          status: true
          event: true
        imessages:
          status: true
          event: true
        logset_operates:
          status: true
        logset_logins:
          status: true
        logset_jobs:
          status: true
        x_orders: # For experiment
          status: true
      rest_txn_timeout: 3m

```
