---
weight: 2
title: 应用部署
---

# 应用部署

首先要上线相关的扩展服务，命名空间必须是一致的，设 `example` 为命名空间，为 MongoDB 创建数据库 `example`，按以下顺序进行部署：

{{<hint info>}}
镜像真实使用时需要明确指定版本，例如 `v1.0.0` 
{{</hint>}}

## 创建收集服务

设置你需要连接的数据库和 NATS 集群，也可以为其增加多个 replicas，[详情](/docs/4-extend/1-collector/)

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
              value: mongodb+srv://example:123456@exp.xxxx.mongodb.net/?readPreference=secondaryPreferred&tls=true&authSource=example
            - name: DATABASE_NAME
              value: example
```

## 创建定时调度

新增一个0号定时调度节点，定时调度 NODE 必须保持唯一，[详情](/docs/4-extend/1-collector/)

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

## 创建工作触发

工作触发节点也可以其增加多个 replicas，[详情](/docs/4-extend/1-collector/)

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

## 创建初始配置

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

## 创建初始化

挂载初始化配置，并填写相关的环境变量，启动初始化 Job

{{<hint info>}}
如果 Ingress 上层还存在 WAF 和 CDN，真实 IP 则会被覆盖，可以在 CDN 层的回源设置中将 X-Forwarded-For 更换为 X-Client-Ip
{{</hint>}}

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: server
spec:
  completions: 1
  parallelism: 1
  backoffLimit: 0
  ttlSecondsAfterFinished: 30
  template:
    spec:
      restartPolicy: Never
      containers:
        - name: server
          image: registry.cn-shenzhen.aliyuncs.com/weplanx/server:latest
          imagePullPolicy: Always
          env:
            - name: MODE
              value: release
            - name: CONSOLE
              value: https://console.xxx.com
            - name: XDOMAIN
              value: .xxx.com
            - name: IP
              value: X-Client-Ip
            - name: NAMESPACE
              value: example
            - name: KEY
              value: <*** your key ***>
            - name: DATABASE_URL
              value: mongodb+srv://example:123456@exp.xxxx.mongodb.net/?readPreference=secondaryPreferred&tls=true&authSource=example
            - name: DATABASE_NAME
              value: example
            - name: DATABASE_REDIS
              value: redis://...@redis.database.svc.cluster.local:6379
            - name: INFLUX_URL
              value: https://...
            - name: INFLUX_ORG
              value: example
            - name: INFLUX_TOKEN
              value: <*** your influx token ***>
            - name: INFLUX_BUCKET
              value: observability
            - name: NATS_HOSTS
              value: nats://nats.nats.svc.cluster.local:4222
            - name: NATS_NKEY
              value: <*** your nats nkey ***>
            - name: OTLP_ENDPOINT
              value: telegraf.default.svc.cluster.local:4317
          command: [ './server', 'setup' ]
          volumeMounts:
            - name: config
              mountPath: /app/config
              readOnly: true
      volumes:
        - name: config
          configMap:
            name: server
```

## 创建管理员

延续上个 Job 修改部分配置，再启动

- u 是用户名，必须是电子邮件
- p 是初始密码

```yaml {hl_lines=["50-54"]}
apiVersion: batch/v1
kind: Job
metadata:
  name: server
spec:
  completions: 1
  parallelism: 1
  backoffLimit: 0
  ttlSecondsAfterFinished: 30
  template:
    spec:
      restartPolicy: Never
      containers:
        - name: server
          image: registry.cn-shenzhen.aliyuncs.com/weplanx/server:latest
          imagePullPolicy: Always
          env:
            - name: MODE
              value: release
            - name: CONSOLE
              value: https://console.xxx.com
            - name: XDOMAIN
              value: .xxx.com
            - name: IP
              value: X-Client-Ip
            - name: NAMESPACE
              value: example
            - name: KEY
              value: <*** your key ***>
            - name: DATABASE_URL
              value: mongodb+srv://example:123456@exp.xxxx.mongodb.net/?readPreference=secondaryPreferred&tls=true&authSource=example
            - name: DATABASE_NAME
              value: example
            - name: DATABASE_REDIS
              value: redis://...@redis.database.svc.cluster.local:6379
            - name: INFLUX_URL
              value: https://...
            - name: INFLUX_ORG
              value: example
            - name: INFLUX_TOKEN
              value: <*** your influx token ***>
            - name: INFLUX_BUCKET
              value: observability
            - name: NATS_HOSTS
              value: nats://nats.nats.svc.cluster.local:4222
            - name: NATS_NKEY
              value: <*** your nats nkey ***>
            - name: OTLP_ENDPOINT
              value: telegraf.default.svc.cluster.local:4317
          command: [ 'sh' ]
          args:
            - "-c"
            - | 
              ./server user -u example@xx.com -p pass@VAN1234
          volumeMounts:
            - name: config
              mountPath: /app/config
              readOnly: true
      volumes:
        - name: config
          configMap:
            name: server
```

## 创建应用

填写相关的环境变量，启动应用，也可以为其增加多个 replicas

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: server
  labels:
    app: server
spec:
  replicas: 1
  selector:
    matchLabels:
      app: server
  template:
    metadata:
      labels:
        app: server
    spec:
      containers:
        - name: server
          image: registry.cn-shenzhen.aliyuncs.com/weplanx/server:latest
          imagePullPolicy: Always
          env:
            - name: MODE
              value: release
            - name: CONSOLE
              value: https://console.xxx.com
            - name: XDOMAIN
              value: .xxx.com
            - name: IP
              value: X-Client-Ip
            - name: NAMESPACE
              value: example
            - name: KEY
              value: <*** your key ***>
            - name: DATABASE_URL
              value: mongodb+srv://example:123456@exp.xxxx.mongodb.net/?readPreference=secondaryPreferred&tls=true&authSource=example
            - name: DATABASE_NAME
              value: example
            - name: DATABASE_REDIS
              value: redis://...@redis.database.svc.cluster.local:6379
            - name: INFLUX_URL
              value: https://...
            - name: INFLUX_ORG
              value: example
            - name: INFLUX_TOKEN
              value: <*** your influx token ***>
            - name: INFLUX_BUCKET
              value: observability
            - name: NATS_HOSTS
              value: nats://nats.nats.svc.cluster.local:4222
            - name: NATS_NKEY
              value: <*** your nats nkey ***>
            - name: OTLP_ENDPOINT
              value: telegraf.default.svc.cluster.local:4317
          command: [ "./server", "api" ]
          ports:
            - containerPort: 3000
          volumeMounts:
            - name: config
              mountPath: /app/config
              readOnly: true
      volumes:
        - name: config
          configMap:
            name: server
```

## 设置服务

为应用 Pod 设置 Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: server
spec:
  ports:
    - port: 3000
      protocol: TCP
  selector:
    app: server
```

## 设置中间件

为应用 Ingress 设置 Traefik 安全头与跨域中间件

```yaml
apiVersion: traefik.io/v1alpha1
kind: Middleware
metadata:
  name: security
spec:
  headers:
    frameDeny: true
    browserXssFilter: true
---
apiVersion: traefik.io/v1alpha1
kind: Middleware
metadata:
  name: cors
spec:
  headers:
    accessControlAllowMethods:
      - GET
      - POST
      - PUT
      - PATCH
      - DELETE
    accessControlAllowHeaders:
      - Origin
      - Content-Length
      - Content-Type
      - X-Page
      - X-Pagesize
      - X-Xsrf-Token
    accessControlExposeHeaders:
      - X-Total
    accessControlAllowOriginList:
      - https://console.xxx.com
    accessControlMaxAge: 7200
    accessControlAllowCredentials: true
    addVaryHeader: true
```

## 设置入口

设置 Ingress，接入中间件与服务，最后将域名 `api.xxx.com` 解析至该集群

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: api
  annotations:
    traefik.ingress.kubernetes.io/router.entrypoints: web,websecure
    traefik.ingress.kubernetes.io/router.middlewares: default-security@kubernetescrd,default-cors@kubernetescrd
spec:
  rules:
    - host: api.xxx.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: server
                port:
                  number: 3000
```
