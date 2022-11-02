---
hideInToc: true
layout: iframe-right
url: https://keda.sh/docs/2.7/concepts/#architecture
---

# 事件驱动的伸缩器 🎢

Serverless 需要能伸缩到零

[KEDA](https://keda.sh/) 是一个基于 Kubernetes 的事件驱动自动缩放器。使用 KEDA，您可以根据需要处理的事件数量来驱动 Kubernetes 中任何容器的扩展。

- KEDA 是一个单一用途的轻量级组件，可以添加到任何 Kubernetes 集群中。
- KEDA 与 HPA（[Horizo​​ntal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)）等标准 Kubernetes 组件一起工作，可以在不覆盖或复制的情况下扩展功能。
- 使用 KEDA，您可以明确映射您想要使用事件驱动比例的应用程序，这使得 KEDA 成为与任意数量的任何其他 Kubernetes 应用程序或框架一起运行的灵活且安全的选项。