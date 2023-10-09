---
weight: 2
title: Deployment
---

# Deployment

First, to launch the relevant extension services, the namespace must be consistent, set `example` as the namespace, create a database `example` for MongoDB, and deploy in the following order:

{{<hint info>}}
When mirroring is actually used, the version needs to be explicitly specified, such as `v1.0.0`
{{</hint>}}

## Create Collector

Set up the database and NATS cluster you need to connect to, or add multiple replicas to it. [More](/docs/4-extend/1-collector/)

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

## Create Schedule

Add a `0` Schedule Node,  NODE definitions must remain unique. [More](/docs/4-extend/1-collector/)

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

## Create Worker

Also add multiple replicas. [More](/docs/4-extend/1-collector/)

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

## Create ConfigMap

Create a `ConfigMap`, define *default.values.yml*. 

The configuration name is the **Snake Case** of dynamic configuration, for example:

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

## Create Initializer

Mounting initialize configuration, and fill in the relevant environement variables to start initialize Job

{{<hint info>}}
If there are WAF and CDN in the upper layer of the Ingress, the real IP will be covered. You can replace X-Forwarded-For with X-Client-Ip in the back to the source setting of the CDN layer.
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

## Create Administrator

Continue the previous Job to modify part of the configuration, and then start

- u Username, must be email
- p Initial password

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

## Create App

Fill in the relevant environement variables, start the application, and add multiple replicas to it

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

## Apply Service

Apply Service for App Pods

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

## Set Middleware

Setting up Traefik security headers and cross-domain middleware for application Ingress

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

## Apply Ingress

Apply Ingress, integration middleware and services, and finally resolve the domain name api.xxx.com to the cluster

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

## Front-end static pages

There is no need for SEO, the SSR method is not considered.

The front-end uses static PWAs, so you need to clone the `console` project and install dependency.

```shell
git clone git@github.com:weplanx/console.git
cd console && npm install
```

Modify the configuration that needs to be deployed in the project *environments/environment.ts*, and execute the build

```shell
npm run build
```

After that, any static deployment method can be used https://angular.io/guide/deployment.

You can also use object storage, and then use the CDN back to the source to speed up its implementation.
