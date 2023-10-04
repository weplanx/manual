---
bookToC: false
title: 简介
type: docs
---

# 👋 欢迎访问

Weplanx 是一个探索 DevOps 与 LowCode 结合的开源项目，你可以通过他轻松管理资源、工作流、队列和即时通讯等等，同样也可以作为任意分布式系统的基础服务进行定制。

作为基础服务，Weplanx 可以提供：

- 常规系统包括的基础功能
- 分布式的动态配置秒级分发与同步
- 低码可控且具备事件补偿的 REST 数据集接口
- 便于业务定制的 Angular 和 Golang 的开发辅助库

Weplanx 架构定位在中轻量级应用，但需要运行在 Kubernetes 上，考虑轻量可以选择 [Rancher K3s 方案](https://www.rancher.com/products/k3s) 进行部署，此外需要准备：

- 计算节点
  - 至少 3 个 2C4G 节点组成 K3s 集群，全自建需 7 个节点
- MongoDB
  - 推荐版本 >= 5.0（支持时序集合）
  - 生产环境至少 2C4G 副本集群，优先 Atlas 或公有云，自建推荐 [Percona Distribution for MongoDB](https://docs.percona.com/percona-distribution-for-mongodb/6.0/?_gl=1*1xck7lp*_gcl_au*NzQ1MjkwMTI4LjE2OTU4NjE5MDM.*_ga*OTA4OTcwMDcyLjE2OTU4NjE5MDU.*_ga_DXWV0B7PSN*MTY5NjQyOTY1Ni42LjEuMTY5NjQyOTY4NC4zMi4wLjA.)
- Redis
  - 按需即可，建议 >= 256M 1主1副开始
- Nats
  - 需启用 JetStream，[Helm 部署](https://docs.nats.io/running-a-nats-service/nats-kubernetes)至本集群即可

案例概览：

{{< tabs "gallery" >}}

{{< tab "登录 & 个人中心" >}}
![login.png](/images/login.png)
![forget.png](/images/forget.png)
![center.png](/images/center.png)
{{< /tab >}}

{{< tab "项目 & 集群" >}}
![projects.png](/images/projects.png)
![clusters.png](/images/clusters.png)
{{< /tab >}}

{{< tab "工作流" >}}
![workflows-1.png](/images/workflows-1.png)
![workflows-2.png](/images/workflows-2.png)
![workflows-3.png](/images/workflows-3.png)
![workflows-4.png](/images/workflows-4.png)
{{< /tab >}}

{{< tab "消息队列" >}}
![queues-1.png](/images/queues-1.png)
![queues-2.png](/images/queues-2.png)
{{< /tab >}}

{{< tab "即时通信" >}}
![imessages-1.png](/images/imessages-1.png)
![imessages-2.png](/images/imessages-2.png)
![imessages-3.png](/images/imessages-3.png)
{{< /tab >}}

{{< tab "数据集" >}}
![dataset-1.png](/images/dataset-1.png)
![dataset-2.png](/images/dataset-2.png)
![dataset-3.png](/images/dataset-3.png)
{{< /tab >}}


{{< tab "服务集成" >}}
![integrated-1.png](/images/integrated-1.png)
![integrated-2.png](/images/integrated-2.png)
![integrated-3.png](/images/integrated-3.png)
{{< /tab >}}

{{< tab "资源中心" >}}
![filebrowser.gif](/images/filebrowser.gif)
![filebrowser-1.png](/images/filebrowser-1.png)
![filebrowser-2.png](/images/filebrowser-2.png)
{{< /tab >}}

{{< tab "设置 & 审计日志" >}}
![settings-1.png](/images/settings-1.png)
![settings-2.png](/images/settings-2.png)
![settings-3.png](/images/settings-3.png)
![audit.png](/images/audit.png)
{{< /tab >}}

{{< /tabs >}}

{{< details title="应用观测（已弃用）" open=true >}}
{{< hint warning >}}
计划使用 Elastic APM 替代 InfluxDB OSS v2 定制接口
{{< /hint >}}
![observability-1.png](/images/observability-1.png)
![observability-2.png](/images/observability-2.png)
![observability-3.png](/images/observability-3.png)
![observability-4.png](/images/observability-4.png)
![observability-5.png](/images/observability-5.png)
{{< /details >}}
