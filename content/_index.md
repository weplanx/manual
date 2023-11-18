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

## Case Overview

{{< tabs "Login & Center" >}}
{{< tab "Login-1" >}}
![login-1.png](/images/login-1.png)
{{< /tab >}}
{{< tab "Login-2" >}}
![login-2.png](/images/login-2.png)
{{< /tab >}}
{{< tab "Login-3" >}}
![login-3.png](/images/login-3.png)
{{< /tab >}}
{{< tab "Forget" >}}
![forget.png](/images/forget.png)
{{< /tab >}}
{{< tab "Center-1" >}}
![center-1.png](/images/center-1.png)
{{< /tab >}}
{{< tab "Center-2" >}}
![center-2.png](/images/center-2.png)
{{< /tab >}}
{{< tab "Center-3" >}}
![center-3.png](/images/center-3.png)
{{< /tab >}}
{{< tab "Center-4" >}}
![center-4.png](/images/center-4.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Project" >}}
{{< tab "Project-1" >}}
![project-1.png](/images/project-1.png)
{{< /tab >}}
{{< tab "Project-2" >}}
![project-2.png](/images/project-2.png)
{{< /tab >}}
{{< tab "Project-3" >}}
![project-3.png](/images/project-3.png)
{{< /tab >}}
{{< tab "Project-4" >}}
![project-4.png](/images/project-4.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Cluster" >}}
{{< tab "Cluster-1" >}}
![cluster-1.png](/images/cluster-1.png)
{{< /tab >}}
{{< tab "Cluster-2" >}}
![cluster-2.png](/images/cluster-2.png)
{{< /tab >}}
{{< tab "Cluster-3" >}}
![cluster-3.png](/images/cluster-3.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Endpoint" >}}
{{< tab "Endpoint-1" >}}
![endpoint-1.png](/images/endpoint-1.png)
{{< /tab >}}
{{< tab "Endpoint-2" >}}
![endpoint-2.png](/images/endpoint-2.png)
{{< /tab >}}
{{< tab "Endpoint-3" >}}
![endpoint-3.png](/images/endpoint-3.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Builder" >}}
{{< tab "Builder-1" >}}
![builder-1.png](/images/builder-1.png)
{{< /tab >}}
{{< tab "Builder-2" >}}
![builder-2.png](/images/builder-2.png)
{{< /tab >}}
{{< tab "Builder-3" >}}
![builder-3.png](/images/builder-3.png)
{{< /tab >}}
{{< tab "Builder-4" >}}
![builder-4.png](/images/builder-4.png)
{{< /tab >}}
{{< tab "Builder-5" >}}
![builder-5.png](/images/builder-5.png)
{{< /tab >}}
{{< tab "Builder-6" >}}
![builder-6.png](/images/builder-6.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Workflow" >}}
{{< tab "Workflow-1" >}}
![workflow-1.png](/images/workflow-1.png)
{{< /tab >}}
{{< tab "Workflow-2" >}}
![workflow-2.png](/images/workflow-2.png)
{{< /tab >}}
{{< tab "Workflow-3" >}}
![workflow-3.png](/images/workflow-3.png)
{{< /tab >}}
{{< tab "Workflow-4" >}}
![workflow-4.png](/images/workflow-4.png)
{{< /tab >}}
{{< tab "Workflow-5" >}}
![workflow-5.png](/images/workflow-5.png)
{{< /tab >}}
{{< tab "Workflow-6" >}}
![workflow-6.png](/images/workflow-6.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Queue" >}}
{{< tab "Queue-1" >}}
![queue-1.png](/images/queue-1.png)
{{< /tab >}}
{{< tab "Queue-2" >}}
![queue-2.png](/images/queue-2.png)
{{< /tab >}}
{{< tab "Queue-3" >}}
![queue-3.png](/images/queue-3.png)
{{< /tab >}}
{{< tab "Queue-4" >}}
![queue-4.png](/images/queue-4.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "IMessage" >}}
{{< tab "IMessage-1" >}}
![imessage-1.png](/images/imessage-1.png)
{{< /tab >}}
{{< tab "IMessage-2" >}}
![imessage-2.png](/images/imessage-2.png)
{{< /tab >}}
{{< tab "IMessage-3" >}}
![imessage-3.png](/images/imessage-3.png)
{{< /tab >}}
{{< tab "IMessage-4" >}}
![imessage-4.png](/images/imessage-4.png)
{{< /tab >}}
{{< tab "IMessage-5" >}}
![imessage-5png](/images/imessage-5.png)
{{< /tab >}}
{{< tab "IMessage-6" >}}
![imessage-6.png](/images/imessage-6.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Dataset" >}}
{{< tab "Dataset-1" >}}
![dataset-1.png](/images/dataset-1.png)
{{< /tab >}}
{{< tab "Dataset-2" >}}
![dataset-2.png](/images/dataset-2.png)
{{< /tab >}}
{{< tab "Dataset-3" >}}
![dataset-3.png](/images/dataset-3.png)
{{< /tab >}}
{{< tab "Dataset-4" >}}
![dataset-4.png](/images/dataset-4.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Monitor" >}}
{{< tab "Monitor-1" >}}
{{< hint warning >}}
APM and RUNTIME have used Elastic APM solution
{{< /hint >}}

![monitor-1.png](/images/monitor-1.png)
{{< /tab >}}
{{< tab "Monitor-2" >}}
![monitor-2.png](/images/monitor-2.png)
{{< /tab >}}
{{< tab "Monitor-3" >}}
![monitor-3.png](/images/monitor-3.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Integrated" >}}
{{< tab "Integrated-1" >}}
![integrated-1.png](/images/integrated-1.png)
{{< /tab >}}
{{< tab "Integrated-2" >}}
![integrated-2.png](/images/integrated-2.png)
{{< /tab >}}
{{< tab "Integrated-3" >}}
![integrated-3.png](/images/integrated-3.png)
{{< /tab >}}
{{< tab "Integrated-4" >}}
![integrated-4.png](/images/integrated-4.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Setting & Audit" >}}
{{< tab "Setting-1" >}}
![setting-1.png](/images/setting-1.png)
{{< /tab >}}
{{< tab "Setting-2" >}}
![setting-2.png](/images/setting-2.png)
{{< /tab >}}
{{< tab "Setting-3" >}}
![setting-3.png](/images/setting-3.png)
{{< /tab >}}
{{< tab "Setting-4" >}}
![setting-4.png](/images/setting-4.png)
{{< /tab >}}
{{< tab "Aduit" >}}
![audit.png](/images/audit.png)
{{< /tab >}}
{{< /tabs >}}

{{< tabs "Filebrowser" >}}
{{< tab "Picture-1" >}}
![picture-1.png](/images/picture-1.png)
{{< /tab >}}
{{< tab "Picture-2" >}}
![picture-2.png](/images/picture-2.png)
{{< /tab >}}
{{< tab "Picture-3" >}}
![picture-3.png](/images/picture-3.png)
{{< /tab >}}
{{< tab "Video" >}}
![video.png](/images/video.gif)
{{< /tab >}}
{{< /tabs >}}