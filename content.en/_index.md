---
bookToC: false
title: Home
type: docs
---

## 👋 Hi, there.

Weplanx is an open source project that explores the combination of DevOps and LowCode.

You can easily manage resources, workflows, queues and IM (instant messaging), and can also be customized as the basic service of any distribution system.

The technology stack includes Golang, TypeScript, MongoDB, Redis and Nats, etc. 

Developed based on [Hertz](https://github.com/cloudwego/hertz) and [Ng-Zorro-Antd](https://github.com/NG-ZORRO/ng-zorro-antd).

As a basic service, Weplanx can provide:

- Basic functions included in conventional systems
- Distribution of dynamic configuration sync ups
- Can be friendly to support low-code mongodb RESTFul
- Angular and Golang development support libraries for business customization

Weplanx architecture for medium-light applications, but needs to run on Kubernetes. 

[Rancher K3s](https://www.rancher.com/products/k3s) recommended for deploying lightweight K8s, also need to prepare these:

- Computing node
  - At least 3 2C4G nodes form a K3s cluster, or 7 nodes are required for full self-host
- MongoDB
  - Recommended version > = 5.0 (supports Time series)
  - Production environment at least 2C4G replica cluster
  - Atlas or public cloud can be preferred, self-built recommendation Percona Distribution for MongoDB
- Redis
  - Recommended > = 256M
  - Redis Cloud or public cloud can be preferred
- Nats
  - Need to enable JetStream, [Helm](https://docs.nats.io/running-a-nats-service/nats-kubernetes) can be deployed to this cluster

Case Overview

{{< tabs "gallery" >}}

{{< tab "Login & Center" >}}
![login.png](/images/login.png)
![forget.png](/images/forget.png)
![center.png](/images/center.png)
{{< /tab >}}

{{< tab "Project & Cluster" >}}
![projects.png](/images/projects.png)
![clusters.png](/images/clusters.png)
{{< /tab >}}

{{< tab "Workflow" >}}
![workflows-1.png](/images/workflows-1.png)
![workflows-2.png](/images/workflows-2.png)
![workflows-3.png](/images/workflows-3.png)
![workflows-4.png](/images/workflows-4.png)
{{< /tab >}}

{{< tab "Queue" >}}
![queues-1.png](/images/queues-1.png)
![queues-2.png](/images/queues-2.png)
{{< /tab >}}

{{< tab "IMessage" >}}
![imessages-1.png](/images/imessages-1.png)
![imessages-2.png](/images/imessages-2.png)
![imessages-3.png](/images/imessages-3.png)
{{< /tab >}}

{{< tab "Dataset" >}}
![dataset-1.png](/images/dataset-1.png)
![dataset-2.png](/images/dataset-2.png)
![dataset-3.png](/images/dataset-3.png)
{{< /tab >}}


{{< tab "Integrated" >}}
![integrated-1.png](/images/integrated-1.png)
![integrated-2.png](/images/integrated-2.png)
![integrated-3.png](/images/integrated-3.png)
{{< /tab >}}

{{< tab "Filebrowser" >}}
![filebrowser.gif](/images/filebrowser.gif)
![filebrowser-1.png](/images/filebrowser-1.png)
![filebrowser-2.png](/images/filebrowser-2.png)
{{< /tab >}}

{{< tab "Setting & Audit" >}}
![settings-1.png](/images/settings-1.png)
![settings-2.png](/images/settings-2.png)
![settings-3.png](/images/settings-3.png)
![audit.png](/images/audit.png)
{{< /tab >}}

{{< /tabs >}}

{{< details title="Observability(deprecated)" open=true >}}
{{< hint warning >}}
Plan to use Elastic APM to replace InfluxDB OSS v2 custom
{{< /hint >}}
![observability-1.png](/images/observability-1.png)
![observability-2.png](/images/observability-2.png)
![observability-3.png](/images/observability-3.png)
![observability-4.png](/images/observability-4.png)
![observability-5.png](/images/observability-5.png)
{{< /details >}}
