---
weight: 5
title: 启用 APM
---

# 启用 APM

Weplanx 采用 [Elastic Cloud on Kubernetes](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-overview.html) 进行全文索引与 APM 观测，无论使用自建还是公有云，首先在应用集群中增加 CRDS

```shell
# https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-deploy-eck.html

kubectl create -f https://download.elastic.co/downloads/eck/2.9.0/crds.yaml
```

## 创建 APM Server

如果是自建 ECK 则可以直接关联内部 elasticsearch 与 kibana，例如

```yaml
# https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-apm-eck-managed-es.html

apiVersion: apm.k8s.elastic.co/v1
kind: ApmServer
metadata:
  name: weplanx
  namespace: elastic-system
spec:
  version: 8.10.3
  count: 1
  config:
    apm-server:
      host: "0.0.0.0:8200"
      rum:
        enabled: true
      expvar:
        enabled: true
  http:
    tls:
      selfSignedCertificate:
        disabled: true
  podTemplate:
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
              - matchExpressions:
                  - key: kubernetes.io/hostname
                    operator: NotIn
                    values:
                      - main
  elasticsearchRef:
    name: elasticsearch
  kibanaRef:
    name: kibana
```

如果是从外部连接则可以是

```yaml
apiVersion: apm.k8s.elastic.co/v1
kind: ApmServer
metadata:
  name: weplanx
  namespace: elastic-system
spec:
  version: 8.10.3
  count: 1
  config:
    apm-server:
      host: "0.0.0.0:8200"
      rum:
        enabled: true
      expvar:
        enabled: true
      kibana:
        enabled: true
        host: https://kibana.xxx.com:5601
    output:
      elasticsearch:
        hosts: [ "https://elastic.xxx.com:9200" ]
        username: xxx
        password: xxx
        protocol: "https"
  http:
    tls:
      selfSignedCertificate:
        disabled: true
```

获取 APM TOKEN

```shell
# https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-apm-connecting.html

kubectl get secret/weplanx-apm-token -o go-template='{{index .data "secret-token" | base64decode}}' -n elastic-system
```

每次创建都会自动生成一个新的密钥和服务，将其接入应用 OpenTelemetry

- MODE: "release" 对应 APM 环境
- OTLP_ENDPOINT: "weplanx-apm-http.elastic-system.svc:8200" 接入地址
- OTLP_TOKEN: "<*** APM TOKEN ***>"

最后 kibana 中一定要启用 APM 集成功能，接入成功

![](/images/apm/example.png)