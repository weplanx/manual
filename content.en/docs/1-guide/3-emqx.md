---
weight: 3
title: Enable EMQX
---

# Enable EMQX

Weplanx adopts [EMQX](https://www.emqx.io/) for integration to realize authorization management for real-time messaging.

The open source version of EMQX Broker is used in the project.

Of course, the stable and powerful EMQX Enterprise is recommended for enterprise level.

## Deploy

After installing EMQX Operator, deploy Broker, refer to the [documentation](https://docs.emqx.com/en/emqx-operator/latest/getting-started/getting-started.html)

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

## Create XAPI

After the application is launched, create an XAPI for use in the internal, port recommendation `:6000`

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

## Apply Service

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

## Set HTTP Authentication and Authorization

The XAPI integrated EMQX API has [these](/en/docs/6-xapi/1-emqx/), next you need to access the EMQX Dashboard

### Authentication

- Mechanism: Password-Based
- Backend: HTTP Server
- Method: POST
- URL：http://xapi.emqx.svc.cluster.local:6000/emqx/auth
- Body:

```json
{
  "identity": "${username}",
  "token": "${password}"
}
```

### Authorization

- Backend: HTTP Server
- Method: POST
- URL：http://xapi.emqx.svc.cluster.local:6000/emqx/acl
- Body:

```json
{
  "identity": "${username}",
  "topic": "${topic}"
}
```

## Data Bridges

The data bridge of XAPI is to collect logs for the open source version of Broker message sending. 

If the enterprise version itself supports more functions, there is no need to use this method.

- Type of Data Bridge: HTTP Server
- Body:

```json
{
  "client": "${clientid}",
  "topic": "${topic}",
  "payload": "${payload}"
}
```
