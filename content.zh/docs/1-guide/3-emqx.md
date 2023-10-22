---
weight: 3
title: 启用 EMQX
---

# 启用 EMQX

Weplanx 采用 [EMQX](https://www.emqx.io/)（大规模分布式 MQTT 消息服务器） 进行集成实现对即时通信的授权管理。项目中使用 EMQX Broker 开源版，当然企业级更推荐稳定强大的 EMQX Enterprise 。

## 部署

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

在应用上线完毕后，创建 XAPI 服务用于内部系统，详情查看[应用部署](/zh/docs/1-guide/2-deployment/#创建应用)

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
        - name: api
          image: registry.cn-shenzhen.aliyuncs.com/weplanx/server:latest
          imagePullPolicy: Always
          envFrom:
            - configMapRef:
                name: env
          command: [ "./server", "api" ]
          ports:
            - containerPort: 3000
          volumeMounts:
            - name: config
              mountPath: /app/config
              readOnly: true
        - name: xapi
          image: registry.cn-shenzhen.aliyuncs.com/weplanx/server:latest
          imagePullPolicy: Always
          envFrom:
            - configMapRef:
                name: env
          command: [ "./server", "xapi" ]
          ports:
            - containerPort: 6000
          volumeMounts:
            - name: config
              mountPath: /app/config
              readOnly: true
        - name: openapi
          image: registry.cn-shenzhen.aliyuncs.com/weplanx/server:latest
          imagePullPolicy: Always
          envFrom:
            - configMapRef:
                name: env
          command: [ "./server", "openapi" ]
          ports:
            - containerPort: 9000
          volumeMounts:
            - name: config
              mountPath: /app/config
              readOnly: true
      volumes:
        - name: config
          configMap:
            name: server
```

创建内部服务

```yaml
apiVersion: v1
kind: Service
metadata:
  name: server
spec:
  ports:
    - name: api
      port: 3000
      protocol: TCP
    - name: xapi
      port: 6000
      protocol: TCP
    - name: openapi
      port: 9000
      protocol: TCP
  selector:
    app: server
```

## 设置 HTTP 认证与授权

XAPI 集成 EMQX 接口可[查看](/docs/6-xapi/1-emqx/)，接下来需要访问 EMQX Dashboard

### 客户端认证

- 认证方式：Password-Based
- 数据源：HTTP 服务
- 请求方式：POST
- URL：http://server.default.svc:6000/emqx/auth
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
- URL：http://server.default.svc:6000/emqx/acl
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
- 名称：logset，必须
- 请求方式：POST
- URL：http://server.default.svc:6000/emqx/bridge
- 请求体：

```json
{
  "client": "${clientid}",
  "topic": "${topic}",
  "payload": "${payload}"
}
```
