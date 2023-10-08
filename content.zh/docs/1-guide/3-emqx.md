---
weight: 3
title: 启用即时通信
---

# 启用即时通讯

Weplanx 采用 [EMQX](https://www.emqx.io/)（大规模分布式 MQTT 消息服务器） 进行集成实现对即时通信的授权管理。项目中使用 EMQX Broker 开源版，当然企业级更推荐稳定强大的 EMQX Enterprise 。

## 部署 EMQX

安装 EMQX Operator 完毕后部署 Broker 即可，[参考文档](https://docs.emqx.com/zh/emqx-operator/latest/getting-started/getting-started.html)

```yaml
apiVersion: apps.emqx.io/v2beta1
kind: EMQX
metadata:
  namespace: emqx
  name: broker
spec:
  image: emqx:5.1
  updateStrategy:
    evacuationStrategy:
      connEvictRate: 1000
      sessEvictRate: 1000
      waitTakeover: 10
    initialDelaySeconds: 10
    type: Recreate
  coreTemplate:
    spec:
      volumeClaimTemplates:
        storageClassName: local-path
        resources:
          requests:
            storage: 128Mi
        accessModes:
          - ReadWriteOnce
```

## 创建 XAPI

在应用上线完毕后，创建 XAPI 服务用于内部系统，对应端口推荐 `:6000`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: xapi
  labels:
    app: xapi
spec:
  replicas: 1
  selector:
    matchLabels:
      app: xapi
  template:
    metadata:
      labels:
        app: xapi
    spec:
      containers:
        - name: server
          image: registry.cn-shenzhen.aliyuncs.com/weplanx/server:latest
          imagePullPolicy: Always
          env:
            - name: MODE
              value: release
            - name: ADDRESS
              value: :6000
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
          command: [ "./server", "xapi" ]
          ports:
            - containerPort: 6000
          volumeMounts:
            - name: config
              mountPath: /app/config
              readOnly: true
      volumes:
        - name: config
          configMap:
            name: server
```

## 创建内部服务

```yaml
apiVersion: v1
kind: Service
metadata:
  name: xapi
spec:
  ports:
    - port: 6000
      protocol: TCP
  selector:
    app: xapi
```

## 设置 HTTP 认证与授权

XAPI 集成 EMQX 接口可[查看](/docs/6-xapi/1-emqx/)，接下来需要访问 EMQX Dashboard

### 客户端认证

- 认证方式：Password-Based
- 数据源：HTTP 服务
- 请求方式：POST
- URL：http://xapi.emqx.svc.cluster.local:6000/emqx/auth
- 请求体：

```json
{
  "identity": "${username}",
  "token": "${password}"
}
```

### 客户端授权

- 数据源：HTTP 服务
- 请求方式：POST
- URL：http://xapi.emqx.svc.cluster.local:6000/emqx/acl
- 请求体：

```json
{
  "identity": "${username}",
  "topic": "${topic}"
}
```

## 设置数据桥接

XAPI 的数据桥接是针对开源版 Broker 消息发送做日志收集，如果是企业版自身是支持更多功能的，无需采用该方式。

- 数据桥接类型：HTTP 服务
- 请求体：

```json
{
  "client": "${clientid}",
  "topic": "${topic}",
  "payload": "${payload}"
}
```
