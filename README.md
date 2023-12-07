---
description: >-
  Weplanx is an open source project that explores the combination of DevOps and
  LowCode.
---

# 👋 Hi, there.

You can easily manage resources, workflows, queues and IM (instant messaging), and can also be customized as the basic service of any distribution system.

The technology stack includes Golang, TypeScript, MongoDB, Redis and Nats, etc.

Developed based on [Hertz](https://github.com/cloudwego/hertz) and [Ng-Zorro-Antd](https://github.com/NG-ZORRO/ng-zorro-antd).

As a basic service, Weplanx can provide:

* Basic functions included in conventional systems
* Distribution of dynamic configuration sync ups
* Can be friendly to support low-code mongodb RESTFul
* Angular and Golang development support libraries for business customization

Weplanx architecture for medium-light applications, but needs to run on Kubernetes.

[Rancher K3s](https://www.rancher.com/products/k3s) recommended for deploying lightweight K8s, also need to prepare these:

* Computing node
  * At least 3 nodes(2C4G) nodes form a K3s cluster, or 7 nodes are required for full self-host
* MongoDB
  * Recommended version > = 5.0 (supports Time series)
  * Production environment at least 2C4G replica cluster
  * Atlas or public cloud can be preferred, self-built recommendation Percona Distribution for MongoDB
* Redis
  * Recommended > = 256M
  * Redis Cloud or public cloud can be preferred
* Nats
  * Need to enable JetStream, [Helm](https://docs.nats.io/running-a-nats-service/nats-kubernetes) can be deployed to this cluster

### Case Overview

{% tabs %}
{% tab title="Login-1" %}
<figure><img src=".gitbook/assets/login-1.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Login-2" %}
<figure><img src=".gitbook/assets/login-2.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Login-3" %}
<figure><img src=".gitbook/assets/login-3.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Forget" %}
<figure><img src=".gitbook/assets/forget.png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Center-1" %}
<figure><img src=".gitbook/assets/center-1.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Center-2" %}
<figure><img src=".gitbook/assets/center-2.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Center-3" %}
<figure><img src=".gitbook/assets/center-3.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Center-4" %}
<figure><img src=".gitbook/assets/center-4.png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Project-1" %}
<figure><img src=".gitbook/assets/project-1.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Project-2" %}
<figure><img src=".gitbook/assets/project-2.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Project-3" %}
<figure><img src=".gitbook/assets/project-3 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Project-4" %}
<figure><img src=".gitbook/assets/project-4.png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Cluster-1" %}
<figure><img src=".gitbook/assets/cluster-1.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Cluster-2" %}
<figure><img src=".gitbook/assets/cluster-2.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Cluster-3" %}
<figure><img src=".gitbook/assets/cluster-3.png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Endpoint-1" %}
<figure><img src=".gitbook/assets/endpoint-1 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Endpoint-2" %}
<figure><img src=".gitbook/assets/endpoint-2 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Endpoint-3" %}
<figure><img src=".gitbook/assets/endpoint-3 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Builder-1" %}
<figure><img src=".gitbook/assets/builder-1 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Builder-2" %}
<figure><img src=".gitbook/assets/builder-2 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Builder-3" %}
<figure><img src=".gitbook/assets/builder-3 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Builder-4" %}
<figure><img src=".gitbook/assets/builder-4 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Builder-5" %}
<figure><img src=".gitbook/assets/builder-5 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Builder-6" %}
<figure><img src=".gitbook/assets/builder-6 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Workflow-1" %}
<figure><img src=".gitbook/assets/workflow-1 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Workflow-2" %}
<figure><img src=".gitbook/assets/workflow-2 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Workflow-3" %}
<figure><img src=".gitbook/assets/workflow-3 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Workflow-4" %}
<figure><img src=".gitbook/assets/workflow-4 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Workflow-5" %}
<figure><img src=".gitbook/assets/workflow-5 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Workflow-6" %}
<figure><img src=".gitbook/assets/workflow-6 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Queue-1" %}
<figure><img src=".gitbook/assets/queue-1 (1).png" alt=""><figcaption></figcaption></figure>


{% endtab %}

{% tab title="Queue-2" %}
<figure><img src=".gitbook/assets/queue-2 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Queue-3" %}
<figure><img src=".gitbook/assets/queue-3 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Queue-4" %}
<figure><img src=".gitbook/assets/queue-4 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="IMessage-1" %}
<figure><img src=".gitbook/assets/imessage-1 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="IMessage-2" %}
<figure><img src=".gitbook/assets/imessage-2 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="IMessage-3" %}
<figure><img src=".gitbook/assets/imessage-3.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="IMessage-4" %}
<figure><img src=".gitbook/assets/imessage-4 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="IMessage-5" %}
<figure><img src=".gitbook/assets/imessage-5 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="IMessage-6" %}
<figure><img src=".gitbook/assets/imessage-6 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Dataset-1" %}
<figure><img src=".gitbook/assets/dataset-1 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Dataset-2" %}
<figure><img src=".gitbook/assets/dataset-2 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Dataset-3" %}
<figure><img src=".gitbook/assets/dataset-3.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Dataset-4" %}
<figure><img src=".gitbook/assets/dataset-4 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Monitor-1" %}
{% hint style="warning" %}
APM and RUNTIME have used Elastic APM solution
{% endhint %}

<figure><img src=".gitbook/assets/monitor-1 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Monitor-2" %}
<figure><img src=".gitbook/assets/monitor-2 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Monitor-3" %}
<figure><img src=".gitbook/assets/monitor-3 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Integrated-1" %}
<figure><img src=".gitbook/assets/integrated-1 (1).png" alt=""><figcaption></figcaption></figure>


{% endtab %}

{% tab title="Integrated-2" %}
<figure><img src=".gitbook/assets/integrated-2 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Integrated-3" %}
<figure><img src=".gitbook/assets/integrated-3 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Integrated-4" %}
<figure><img src=".gitbook/assets/integrated-4 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Setting-1" %}
<figure><img src=".gitbook/assets/setting-1 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Setting-2" %}
<figure><img src=".gitbook/assets/setting-2 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Setting-3" %}
<figure><img src=".gitbook/assets/setting-3 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Setting-4" %}
<figure><img src=".gitbook/assets/setting-4 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Audit" %}
<figure><img src=".gitbook/assets/audit (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Picture-1" %}
<figure><img src=".gitbook/assets/picture-1 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Picture-2" %}
<figure><img src=".gitbook/assets/picture-2 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Picture-3" %}
<figure><img src=".gitbook/assets/picture-3 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Video" %}
<figure><img src=".gitbook/assets/video (1).gif" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}
