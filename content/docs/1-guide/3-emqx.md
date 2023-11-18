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

After the application is launched, create an XAPI for use in the internal, View [details](/docs/1-guide/2-deployment/#create-app)

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

Apply Service

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

## Set HTTP Authentication and Authorization

The XAPI integrated EMQX API has [these](/en/docs/6-xapi/1-emqx/), next you need to access the EMQX Dashboard

### Authentication

- Mechanism: Password-Based
- Backend: HTTP Server
- Method: POST
- URL: http://server.default.svc:6000/emqx/auth
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
- URL: http://server.default.svc:6000/emqx/acl
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
- Name(must): logset
- Method: POST
- URL: http://server.default.svc:6000/emqx/bridge
- Body:

```json
{
  "client": "${clientid}",
  "topic": "${topic}",
  "payload": "${payload}"
}
```
