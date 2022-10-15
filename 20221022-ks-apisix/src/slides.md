---
hideInToc: true
download: 'https://github.com/webup/openfunction-talks/raw/main/20221022-ks-apisix/20221022-ks-apisix.pdf'
# try also 'default' to start with a default theme
# theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
# highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## OpenFunction Presentation @ 2022.10.22

  Learn more at [OpenFunction](https://openfunction.dev/)
# persist drawings in exports and build
drawings:
  persist: false
---

# OpenFunction 

“云无关的” 函数计算框架的应用探索

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/webup/openfunction-talks/tree/main/20221022-ks-apisix" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
  <a href="https://github.com/webup/openfunction-talks/raw/main/20221022-ks-apisix/20221022-ks-apisix.pdf" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
hideInToc: true
layout: intro
---

# 张海立

<div class="leading-8 opacity-80">
KubeSphere Ambassador, CNCF OpenFunction TOC Member<br>
驭势科技 UISEE&copy; 云平台研发总监，开源爱好者<br>
云原生专注领域: Kubernetes, DevOps, FaaS, Observability, Service Mesh<br>
</div>

<div class="my-10 grid grid-cols-[40px,1fr] w-min gap-y-4">
  <ri-github-line class="opacity-50"/>
  <div><a href="https://github.com/webup" target="_blank">webup</a></div>
  <ri-twitter-line class="opacity-50"/>
  <div><a href="https://twitter.com/zhanghaili0610" target="_blank">zhanghaili0610</a></div>
  <ri-mail-send-line class="opacity-50"/>
  <div><a href="mailto:haili.zhang@uisee.com" target="_blank">haili.zhang@uisee.com</a></div>
  <ri-article-line class="opacity-50"/>
  <div><a href="https://yuque.com/serviceup" target="_blank">ServiceUP · 语雀</a></div>
</div>

<img src="https://s2.loli.net/2022/05/08/XRTdnohGA9rlewu.png" class="rounded-full w-40 abs-tr mt-16 mr-12"/>

---
hideInToc: true
layout: quote
class: 'ml-30'
---

## 1⃣️ CNCF OpenFunction 函数计算框架简介

## 2⃣️ OpenFunction 的一些应用场景探讨

## 3⃣️ OpenFunction 函数网关的演进及展望

<style>
.slidev-layout h2 {
  @apply leading-20;
}
</style>

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

---
layout: iframe
url: https://www.youtube.com/embed/YWZsXdAXFO8?start=580
---

---
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/aWmQ2ljcbrB0R0nqvdoVZEojQWRhSYxthan97wncdULJmUalBkURa06aNAfx
---

# “云无关”的价值

Cloud Agnostic —— 与云商的 BaaS 解耦

<b class="text-amber-500">"Serverless computing = FaaS + BaaS."</b> [^1]

- 云函数是函数计算的主体
- 各类后端服务是函数实现业务的重要依托

🔒 云商通过提供 “托管的”（Hosted）函数计算框架（FaaS）和各类中间件服务（BaaS），把开发者锁定在托管的云平台上。

🤔 即使函数计算框架是跨语言跨平台的，谁又来填补各云商平台 BaaS 服务的接口差异？

[^1]: [Cloud Programming Simplified: A Berkeley View on Serverless Computing](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2019/EECS-2019-3.pdf). Feb 10, 2019.

<style>
.footnotes-sep {
  @apply mt-10 opacity-10;
}

.footnotes {
  @apply text-sm opacity-75;
}

.footnotes ol {
  @apply list-decimal;
}

.footnote-backref {
  display: none;
}
</style>

---
hideInToc: true
layout: image
image: https://www.cncf.io/wp-content/uploads/2022/02/p9_Repor-01-10-1.svg
class: bg-slate-50 bg-origin-content
---

# Serverless 平台的战争?

[CNCF Annual Survey 2021](https://www.cncf.io/reports/cncf-annual-survey-2021/)

"使用 Serverless 技术的受访者，75% 选择了托管平台"

<style>
h1,p {
  @apply !text-red-500;
}
</style>

---
hideInToc: true
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/mOmUABwfsD0d6cgwGRyzt8vih5uoeXldJO4hyWdUUAjl2g7YeWeQf44kkQxh
---

# 破局之道 🎩

借助 [Dapr](https://dapr.io/) 来集成各云商平台的 BaaS 服务

![Dapr Building Blocks](https://docs.dapr.io/images/overview.png)

> Dapr provides you with APIs that abstract away the complexity of common challenges developers encounter regularly when building distributed applications. These API building blocks can be leveraged as the need arises - use one, several or all to develop your application faster and deliver your solution on time.

---
layout: image
image: https://openfunction.dev/openfunction-0.5-architecture.png
---

---
layout: iframe
url: https://embeds.onemodel.app/d/iframe/RZ3djGBjCkM8p2aOjZnvf0UNWPRA5zPVxbk5d6AHl5sowHNhYk6qQLMvGmMB
---

---
layout: section
---

# 举几个函数的例子 🌰

---
layout: two-cols-header
---

# 一个 Node.js 同步函数

::left::

###### index.mjs

```js
// Standard Express style HTTP sync function
export const tryKnative = (req, res) => {
  res.send(`Hello, ${req.query.u || 'World'}!`);
};
```

> 🔍 参见 Express 的 [request](https://devdocs.io/express-request/) 和 [response](https://devdocs.io/express-response/) 对象的使用

<br>

###### package.json

```json {4,7}
{
  "main": "index.mjs",
  "scripts": {
    "start": "functions-framework --target=tryKnative"
  },
  "dependencies": {
    "@openfunction/functions-framework": "^0.6.0"
  }
}
```

::right::

###### Function CR (Raw Manifest)

```yaml {7-}
apiVersion: core.openfunction.io/v1beta1
kind: Function
metadata:
  name: sample-node-knative
spec:
  version: v2.0.0
  image: '<image-repo>/<image-name>:<image-tag>'
  serving:
    runtime: knative
    template:
      containers:
        - name: function
          imagePullPolicy: IfNotPresent
    params:
      FUNCTION_TARGET: tryKnative
      DEBUG: common:*,ofn:*
```

> 🔍 参见 OpenFunction [Function CRD](https://openfunction.dev/docs/reference/component-reference/function-spec/) 定义

---
layout: two-cols-header
---

# 一个 Node.js 异步函数

::left::

###### index.mjs

```js {6-}
// Standard Express style HTTP sync function
export const tryKnative = (req, res) => {
  res.send(`Hello, ${req.query.u || 'World'}!`);
};

// Async function
export const tryAsync = (ctx, data) => {
  console.log('Data received: %o', data);
  ctx.send(data);
};
```

> 💡 多个函数可以被放在同一个 JS 文件中，并在 CR 中动态指定

<br>

###### package.json

```json {4}
{
  "main": "index.mjs",
  "dependencies": {
    "@openfunction/functions-framework": "^0.6.0"
  }
}
```

::right::

###### Function CR (Raw Manifest)

```yaml {all|9-10|11-13|17-19|20-26|27-} {maxHeight:'400px'}
apiVersion: core.openfunction.io/v1beta1
kind: Function
metadata:
  name: sample-node-async
spec:
  version: v2.0.0
  image: '<image-repo>/<image-name>:<tag>'
  serving:
    # default to knative
    runtime: async
    annotations:
      # default to "grpc"
      dapr.io/app-protocol: http
    template:
      containers:
        - name: function
    params:
      # default to FUNC_NAME value
      FUNCTION_TARGET: tryAsync
    inputs:
      - name: mqtt-input
        component: mqtt-in
    outputs:
      - name: mqtt-output
        component: mqtt-out
        operation: create
    bindings:
      mqtt-in:
        type: bindings.mqtt
        version: v1
        metadata:
          - name: consumerID
            value: '{uuid}'
          - name: url
            value: tcp://admin:public@emqx:1883
          - name: topic
            value: in
      mqtt-out:
        type: bindings.mqtt
        version: v1
        metadata:
          - name: consumerID
            value: '{uuid}'
          - name: url
            value: tcp://admin:public@emqx:1883
          - name: topic
            value: out
```

---
layout: two-cols-header
---

# 一个可以触发异步函数的 Node.js 同步函数

::left::

###### index.mjs

```js {6-}
// Standard Express style HTTP sync function
export const tryKnative = (req, res) => {
  res.send(`Hello, ${req.query.u || 'World'}!`);
};

// OpenFunction standard function which could 
// set bridge between sync and async function
export const tryKnativeAsync = async (ctx, data) => {
  console.log('Data received: %o', data);

  // Forward HTTP body to Dapr outputs
  await ctx.send(data);

  // Send something back as HTTP response 
  // when necessary
  // ctx.res.send(data);
};
```

> 💡 [OpenFunction 函数形态](https://openfunction.dev/docs/concepts/function_signatures/) 可以同时支持同步和异步函数

::right::

###### Function CR (Raw Manifest)

```yaml {9,12|13-} {maxHeight:'360px'}
apiVersion: core.openfunction.io/v1beta1
kind: Function
metadata:
  name: sample-node-knative-async
spec:
  version: v2.0.0
  image: '<image-repo>/<image-name>:<image-tag>'
  serving:
    runtime: knative
    params:
      FUNCTION_TARGET: tryKnativeAsync
      FUNCTION_SIGNATURE_TYPE: openfunction
    outputs:
      - name: mqtt-output
        component: mqtt-out
        operation: create
    bindings:
      mqtt-out:
        type: bindings.mqtt
        version: v1
        metadata:
          - name: consumerID
            value: '{uuid}'
          - name: url
            value: tcp://admin:public@emqx:1883
          - name: topic
            value: out
```

---
layout: center
class: text-center
---

# <ant-design-book-outlined class="opacity-70"/> 案例分享：数据桥接

OpenFunction 简化数据预处理

---
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/N5oMmfF57CUpxQcku8CmJY3i43in8jO1rXLvDiilFxYK15vJuC8vLtayvd1X
---

###### 同步：GitLab Webhook ➡️ ChatOps

```go {9-} {maxHeight:'200px'}
import (
  // ...
  "github.com/OpenFunction/functions-framework-go/functions"
  "github.com/xanzy/go-gitlab"
)

var GitlabClient *Client

func init() {
  functions.HTTP("HelloWorld", SendBeginUnittest, functions.WithFunctionPath("/sendmsg"))
  GitlabClient, _ = gitlab.NewClient("4g9gZCsex36Z9FFXspwJ", gitlab.WithBaseURL("https://gitlab.uisee.ai/"))
}

func SendBeginUnittest(w http.ResponseWriter, r *http.Request) {
  // ...
  projectMergeRequestList, _, err := GitlabClient.MergeRequests.ListProjectMergeRequests(projectId, listGroupMergeRequestsOptions, nil)
  mergeRequest := projectMergeRequestList[0]

  msg := "大佬又提交代码了!! 666666666"
  mergeRequestDiscussion := &CreateMergeRequestDiscussionOptions{ Body: &msg }
  res, _, err := GitlabClient.Discussions.CreateMergeRequestDiscussion(projectId, mergeRequest.IID, mergeRequestDiscussion)
}
```
<br>

###### 异步：消息队列实时数据 ➡️ Prometheus 指标

```go {7-} {maxHeight:'200px'}
import (
  ofctx "github.com/OpenFunction/functions-framework-go/context"
  "github.com/prometheus/client_golang/prometheus"
)

var speedGauge prometheus.Gauge

func init() {
  speedGauge = prometheus.NewGauge(prometheus.GaugeOpts{
    Namespace: namespace,
    Subsystem: subsystem,
    Name:      "speed",
    Help:      "vehicle speed",
  })
}

func trackingHandler(ctx ofctx.Context, vehicleTracking VehicleTracking) (ofctx.Out, error) {
  setAndPushGauge(speedGauge, vehicleTracking.Params.Speed)
  return ctx.ReturnOnSuccess(), nil
}
```

---
layout: center
class: text-center
---

# <ant-design-book-outlined class="opacity-70"/> 案例分享：实时数据处理

OpenFunction 在无人驾驶业务中的一种应用

---
layout: iframe
url: https://embeds.onemodel.app/d/iframe/abvQuuS6QoSkz1MPovXXslweJcsCPoPG73anxZoLu58lFe3UCIS58rytKe8H
---

---
layout: iframe
url: https://embeds.onemodel.app/d/iframe/QSsJOLQszQIxATltAMwPEpcjjLiWa6aZAuHM890P0eBdXN7lXsXehj1Lg5GA
---

---
layout: section
---

# 聊一聊函数网关 🚪

如何让同步函数可以更方便和更灵活的被访问？

---
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/Riy2RD5nFBNIsJGiJe0s00I6K6pKOVeDDMbyRQ0u0MssWFXllUDizq9EyjnK
---

# 基于 Ingress 的网关

OpenFunction [0.5.0](https://github.com/OpenFunction/OpenFunction/releases/tag/v0.5.0) - 0.6.0 中可使用

<v-click>

🚧 每个函数对应的 Service 不便于外部直接访问，如 `sample-serving-d55zz-ksvc-gd64w`

💡 直接的想法：由 [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) 统一函数访问入口

</v-click>

<v-click>

🧑‍💻 通过 [Domain](https://v0-6.openfunction.dev/docs/concepts/domain/) CR 为 [Knative](https://knative.dev/) 同步函数提供 Ingress 访问入口：`http://<domain_name>.<domain_ns>/<function_ns>/<function_name>`

</v-click>

<v-click>

😵 这个方案可用，但也存在一些问题：

- 强依赖于 Knative 的网关（OpenFunction 默认随 Knative 安装使用 [Kourier](https://github.com/knative-sandbox/net-kourier)）
  - Kourier Domain 配置比较麻烦
- 不易适配未来要引入的其它同步函数运行时
   
</v-click>

---
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/gr6NUqnRPeMvc7hf3m35PHGPgPW0D3Ab8qfmJxxS4E0v74uv46pLhURGbeFp
---

# 基于 Gateway 的网关

OpenFunction [0.7.0](https://github.com/OpenFunction/OpenFunction/releases/tag/v0.7.0) 开始可使用

<v-click>

💡 进一步的想法：直接使用 [K8s Gateway API](](https://gateway-api.sigs.k8s.io/)) 

> 更强大更灵活的 Kubernetes 原生网关，越来越多的
> 
> [社区支持](https://gateway-api.sigs.k8s.io/implementations/)：[Contour](https://projectcontour.io/) ✅，[Istio](https://istio.io/)，[Apache APISIX](https://github.com/apache/apisix-ingress-controller) 等等

</v-click>

<v-click>

🧑‍💻 构建新的 [OpenFunction Gateway](https://openfunction.dev/docs/concepts/networking/introduction/) 体系

> Domain 体系从 OpenFunction 0.7.0 开始被移除

</v-click>

<v-click>

🚀 OpenFunction Gateway 的一些特性：

- 开发者可以自定义函数入口，如 `{{.Name}}.{{.NS}}.{{.Domain}}`
- 开发者可以自定义每个函数的路由规则
- 未来可以支持多个函数版本的流量切分

</v-click>

---
layout: iframe-right
url: https://apisix.apache.org/docs/ingress-controller/getting-started/
---

# Apache APISIX®

使用 [APISIX Ingress Controller](https://github.com/apache/apisix-ingress-controller) 作为函数网关

- APISIX Ingress Controller 已经从 [1.5.0](https://github.com/apache/apisix-ingress-controller/releases/tag/1.5.0) 版本开始 [初步支持 Kubernetes Gateway API](https://github.com/apache/apisix-ingress-controller/issues/644)：
- 需要通过 `enable_gateway_api=true` 开启
- OpenFunction Gateway 所需要的 [`HTTPRouteFilter`](https://gateway-api.sigs.k8s.io/references/spec/#gateway.networking.k8s.io/v1beta1.HTTPRouteFilter) [计划](https://github.com/apache/apisix-ingress-controller/issues/644#issuecomment-1275546825) 在 1.6.0 中支持

💪 APISIX 强大而丰富的插件生态也正在助力 OpenFunction 的应用和发展：

- “最近，APISIX 又新增了不少生态插件，其中就包括与 OpenFunction 集成的无服务插件 `openfunction`“ 👉 详见 [集成细节](https://mp.weixin.qq.com/s/b5QHbnFUd-r2VwXfySKJkg)


---
src: ../../pages/end.md
---
