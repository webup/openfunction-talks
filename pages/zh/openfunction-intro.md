---
hideInToc: true
layout: iframe-right
url: https://openfunction.dev/
---

# Open? Function！ ☁️

一个 [CNCF Sandbox](https://www.cncf.io/projects/openfunction/) 函数计算平台项目

[OpenFunction](https://openfunction.dev/) 的云原生特性：

- “云无关”：与云商的 BaaS 松耦合
  - 通过 [Dapr](https://dapr.io/)，简化了对于云商 BaaS 的集成
- 同时支持同步和异步函数
  - 同步函数 [Kubernetes Gateway API](https://gateway-api.sigs.k8s.io/) 支持的高级函数入口和流量管理
  - 异步函数可以直接从事件源消耗事件
- 支持直接从函数源码生成符合 [OCI](https://opencontainers.org/) 镜像
  - 基于 [Cloud Native Buildpacks](https://buildpacks.io/) 生态实现
- 在 0 和 N 之间灵活的自动缩放
  - 充分利用 [KEDA](https://keda.sh/)、[Knative](https://knative.dev/docs/)  
