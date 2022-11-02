---
hideInToc: true
download: 'https://github.com/webup/openfunction-talks/raw/main/20221104-ecnu/220221104-ecnu.pdf'
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
  ## OpenFunction Presentation @ 2022.11.04

  Learn more at [OpenFunction](https://openfunction.dev/)
# persist drawings in exports and build
drawings:
  persist: false
---

# 浅谈 FaaS 与 DevOps 

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/webup/openfunction-talks/tree/main/20221104-ecnu" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
  <a href="https://github.com/webup/openfunction-talks/raw/main/20221104-ecnu/220221104-ecnu.pdf" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
hideInToc: true
---

# 张海立

<div class="leading-8 opacity-80">
KubeSphere Ambassador, CNCF OpenFunction TOC Member<br>
驭势科技 UISEE&copy; 云平台研发总监，开源爱好者<br>
云原生专注领域: Kubernetes, DevOps, FaaS, Observability, Service Mesh<br>
</div>

<div class="my-10 grid grid-cols-[40px,1fr] w-min gap-y-4">
  <ri-wechat-line class="opacity-50"/>
  <div><a href="https://s2.loli.net/2022/10/22/wjYvKo4EWagbJ9L.jpg" target="_blank">zhanghaili</a></div>
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

<style>
.slidev-layout h1 {
  @apply text-6xl mt-10;
}
</style>

---
hideInToc: true
layout: quote
---

# 如何把一个 [GitHub](https://github.com/OpenFunction/functions-framework-nodejs) 仓库私有化到本地 GitLab ？

从一个假想的 DevOps 环境迁移的场景说起

---
hideInToc: true
---

###### 这有何难！

- GitLab 本身就支持从 GitHub 导入并创建项目

<br>
<v-click>

###### 那么，[这么多](https://github.com/OpenFunction/functions-framework-nodejs/tree/master/.github) GitHub App 和 Bot 怎么迁移？
- 有的可以根据 Issue 中的文字自动打标签
- 有的可以根据已合并的 PR 信息自动生成 Release Note
- 有的可以通过评论修改 `README.md` 文件来添加协作者信息
- 有的可以定时检查各个 Issue 或 PR 是否长期无人处理并打上过期标签

</v-click>
<br>
<v-click>

###### 这也没什么难的，DIY 一套呗？！

- 你可能需要创建一个或多个 <u>微服务</u>
- 你可能需要熟悉 GitLab Webhook 和 REST API 来打通服务的 <u>输入和输出</u>
- 你可能还需要一个定时任务或者异步框架来处理 <u>异步</u> 的任务流
- 你可能还需要关心一下这个/些服务的 <u>运行开销</u>，不处理任务时候能不常驻吗
- 甚至，如果维护这个/些服务的小伙伴离开团队了，另一个你会使用相同 <u>开发语言</u> 来持续迭代吗

</v-click>

---
hideInToc: true
layout: quote
---

# FaaS, Function as a ~~Service~~ Silver bullet?

让我们从功能角度来看 FaaS 可以如何赋能 DevOps

---
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/aWmQ2ljcbrB0R0nqvdoVZEojQWRhSYxthan97wncdULJmUalBkURa06aNAfx
---

# Serverless vs. FaaS

人们常把 Serverless 和 FaaS 混为一谈

<b class="text-amber-500">"Serverless computing = FaaS + BaaS."</b> [^1]

- 云函数是函数计算的主体
- 各类后端服务是函数实现业务的重要依托

🔒 云商通过提供 “托管的”（Hosted）函数计算框架（FaaS）和各类中间件服务（BaaS），把开发者锁定在托管的云平台上。

<v-click>

🤔 这里有个潜在的 <u>本地化及迁移</u> 的问题，谁来填补各云商平台 BaaS 服务的接口差异？

</v-click>

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
src: ../../pages/zh/serverless-wars.md
class: bg-slate-50 bg-origin-content
---

---
src: ../../pages/zh/openfunction-intro.md
---

---
src: ../../pages/zh/dapr-intro.md
---

---
hideInToc: true
layout: iframe
url: //player.bilibili.com/player.html?bvid=BV1xz4y167XA&cid=277928385&page=1
---

---
src: ../../pages/zh/keda-intro.md
---

---
layout: image
image: https://openfunction.dev/openfunction-0.5-architecture.png
---

---
layout: iframe
url: https://embeds.onemodel.app/d/iframe/RZ3djGBjCkM8p2aOjZnvf0UNWPRA5zPVxbk5d6AHl5sowHNhYk6qQLMvGmMB
---

---
hideInToc: true
layout: quote
---

# 举几个 Node.js 函数的例子 🌰

OpenFunction 现已支持 Go, Node.js, Python, Java, .Net 等[多种语言](https://openfunction.dev/docs/getting-started/quickstarts/)

---
src: ../../pages/zh/code-node-sync.md
---

---
src: ../../pages/zh/code-node-async.md
---

---
hideInToc: true
layout: quote
---

# OpenFunction 案例探索

让我们看几个现实可用的 FaaS 赋能场景

---
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/N5oMmfF57CUpxQcku8CmJY3i43in8jO1rXLvDiilFxYK15vJuC8vLtayvd1X
---

###### 同步：GitLab Webhook ➡️ ChatOps

```go {9-}
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

---
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/N5oMmfF57CUpxQcku8CmJY3i43in8jO1rXLvDiilFxYK15vJuC8vLtayvd1X
---

###### 异步：消息队列实时数据 ➡️ Prometheus 指标

```go {7-} 
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
  // Helper to push data to gauge
  setAndPushGauge(speedGauge, vehicleTracking.Params.Speed)
  return ctx.ReturnOnSuccess(), nil
}
```

---
layout: iframe
url: https://embeds.onemodel.app/d/iframe/QSsJOLQszQIxATltAMwPEpcjjLiWa6aZAuHM890P0eBdXN7lXsXehj1Lg5GA
---

---
layout: iframe
url: https://openfunction.dev/blog/2022/10/28/announcing-openfunction-0.8.0-speed-up-function-launching-with-dapr-proxy/
---

---
src: ../../pages/end.md
---
