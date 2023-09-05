---
hideInToc: true
download: 'https://github.com/webup/openfunction-talks/raw/main/2023-from-0-to-1/2023-from-0-to-1.pdf'
# try also 'default' to start with a default theme
# theme: light-icons
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
  ## OSPP @ 2023.09.05

  Learn more at [OpenFunction](https://openfunction.dev/)
# persist drawings in exports and build
drawings:
  persist: false
---

# OSPP 2023

从 0 到 1 的 夏天 

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/webup/openfunction-talks/tree/main/2023-from-0-to-1" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
  <a href="https://github.com/webup/openfunction-talks/raw/main/2023-from-0-to-1/2023-from-0-to-1.pdf" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
hideInToc: true
layout: intro
---

# Wenlong Dong

<div class="leading-8 opacity-80">
UESTC, Grade 1 master <br>
DSCL(Distributed Storage and Computing Lab)<br>
2023 OSPP Openfunction Contributor<br>
第四届全球数据库大赛赛道1：云原生共享内存数据库性能优化冠军<br>
</div>

<div class="my-10 grid grid-cols-[40px,1fr] w-min gap-y-4">
  <div class="inline-elements">
  <ri-github-line class="opacity-50"/>&#20&#20
  <div><a href="https://github.com/yi-ge-dian" target="_blank">yi-ge-dian</a></div>
  </div>
  
  <div class="inline-elements">
  <ri-mail-send-line class="opacity-50"/>&#20&#20
  <div><a href="mailto:haili.zhang@uisee.com" target="_blank">wenldong666@gmail.com</a></div>
  </div>

  <div class="inline-elements">
  <ri-article-line class="opacity-50"/>&#20&#20
  <div><a href="https://onepoint.link" target="_blank">onepoint</a></div>
  </div>
<img src="https://s2.loli.net/2023/09/01/HVyZwu8hFNEWmvS.jpg" class="rounded-full w-40 abs-tr 
mt-16 mr-12"/>

</div>

---
hideInToc: true
---

# Table of Content

<Toc listClass="!list-disc" maxDepth="2" />


---
layout: center
class: text-center
level: 1
---

# 如何了解到开源之夏

通过实验室师兄的介绍，我才有幸了解到“开源之夏”这项活动。

---
layout: center
class: quote
level: 2
---

## 为什么选择了 <b class="text-amber-500">KubeSphere</b>
<br>
<br>
<br>
之前有了解过青云，是一家在云服务领域非常有实力的公司，推出了许多优秀的开源项目，充满了浓厚的兴趣。
<br>
<br>
希望自己能够亲身参与到这些开源项目中，与开发者一起共同推动开源事业的发展。

---
layout: center
class: quote
level: 2
---

## 为什么是 <b class="text-amber-500">OpenFunction</b>
<br>
<br>
<br>
OpenFunction 是国内开源的 Serverless 平台
<br>
<br>
一方面可以为开源社区做贡献，另一方面对于自己的研究也有很大的帮助。
<br>
<br>
用户使用 OpenFunction，可以专注于编写核心业务逻辑，而无需担心底层基础设施的运维问题。



---
layout: center
class: text-center
---

# 从 0 到 1, 该如何做

尽早开始，留有余地

---
class: quote
level: 2
---

<br>

## 在项目开始前做了什么

###### 好多新东西，不停的学习😴


* Node.js Learning
* TypeScript Learning
* Dapr Learning
* KinD Learning

<style>
li {
  @apply !text-pink-500;
}
</style>



<v-click>

## Practice <b class="text-amber-500">More and More</b>

</v-click>

---
layout: center
class: quote
level: 2
---

## 在开发过程中做了什么

<br>

###### 摸清项目的整体架构，快速开发💪

<br>

* 拥抱 <b class="text-amber-500">AI</b>，AIGC的时代已经来临，我们应该善于利用它。对于刚学习一门新语言的人来说，询问AI是解决问题的快速途径。

* 和老师多沟通。在最开发状态管理功能时，由于和老师沟通的少了，当自己写完一版代码时，才发现与实现的要求产生了偏差。

* <b class="text-amber-500">Debug</b> 是最快速上手一个项目的方法，可以清晰地掌握整个项目的运行过程。

* 化繁为简，例如，在编写 <b class="text-amber-500">e2e</b> 测试时，完整的项目流程可能包含许多步骤，可以逐步实现并最后运行完整的测试。


---
layout: center
class: text-center
---

# 用户该如何使用状态管理功能
以 Node.js 函数为例 🌰

---
level: 2
---

## Save and Get

<div>

###### index.mjs

```js {all|4|9}
// Async function state save
async function tryKnativeAsyncStateSave(ctx, data) {
  console.log('✅ Function receive request: %o', data);
  await ctx.state.save(data);
}
// Async function state get
async function tryKnativeAsyncStateGet(ctx, data) {
  console.log('✅ Function receive request: %o', data);
  await ctx.state.get(data);
}
```

</div>

<div>

###### package.json

```json{all|4|5}
{
  "main": "index.mjs",
  "scripts": {
    "save": "functions-framework --target=tryKnativeAsyncStateSave --signature=openfunction",
    "get": "functions-framework --target=tryKnativeAsyncStateGet --signature=openfunction",
  },
  "dependencies": {
    "@openfunction/functions-framework": "^0.6.1"
  }
}
```
</div>
<v-click>

</v-click>

<style>
.slidev-code {
  @apply !rounded-none;
}
</style>


---
level: 2
---

## GetBulk and Delete

<div>

###### index.mjs

```js {all|4|9}
// Async function state getBulk
async function tryKnativeAsyncStateGetBulk(ctx, data) {
  console.log('✅ Function receive request: %o', data);
  await ctx.state.getBulk(data);
}
// Async function state delete
async function tryKnativeAsyncStateDelete(ctx, data) {
  console.log('✅ Function receive request: %o', data);
  await ctx.state.delete(data);
}
```

</div>

<div>

###### package.json

```json{all|4|5}
{
  "main": "index.mjs",
  "scripts": {
    "save": "functions-framework --target=tryKnativeAsyncStateGetBulk --signature=openfunction",
    "get": "functions-framework --target=tryKnativeAsyncStateDelete --signature=openfunction",
  },
  "dependencies": {
    "@openfunction/functions-framework": "^0.6.1"
  }
}
```
</div>
<v-click>

</v-click>

<style>
.slidev-code {
  @apply !rounded-none;
}
</style>

---
level: 2
---

## Query and Transaction

<div>

###### index.mjs

```js {all|4|9}
// Async function state query
async function tryKnativeAsyncStateQuery(ctx, data) {
  console.log('✅ Function receive request: %o', data);
  await ctx.state.query(data);
}
// Async function state transaction
async function tryKnativeAsyncStateTransaction(ctx, data) {
  console.log('✅ Function receive request: %o', data);
  await ctx.state.transaction(data);
}
```

</div>

<div>

###### package.json

```json{all|4|5}
{
  "main": "index.mjs",
  "scripts": {
    "save": "functions-framework --target=tryKnativeAsyncStateQuery --signature=openfunction",
    "get": "functions-framework --target=tryKnativeAsyncStateTransaction --signature=openfunction",
  },
  "dependencies": {
    "@openfunction/functions-framework": "^0.6.1"
  }
}
```
</div>
<v-click>

</v-click>

<style>
.slidev-code {
  @apply !rounded-none;
}
</style>

---
layout: center
class: text-center
---

# 有什么感受

thinking🤔️ and thinking🤔️


---
layout: center
hideInToc: true
class: quote
level: 2
---

## 💡来了

1. 开源项目的开发和参加比赛的感受是不同的。在开源项目中，我们需要考虑后续的更新和维护的需求，甚至在定义结构体等方面都需要仔细考虑🤔️。而在比赛中，架构设计和字段定义非常自由🎉，所关注的是如何充分利用资源，以获得最高分数🎁。

2. 仔细阅读开发者的文档📄，也是之前提到的问题在状态管理功能开发时，一方面没有和导师及时沟通，另一方面也没仔细的看文档，当时麻烦了海立老师很久。

3. 一些好用的东西

- [开源最佳实践](https://linuxsuren.github.io/open-source-best-practice/)，如何做好开源。
- [k9s](https://k9scli.io/), 能够非常方便的监视 k8s 集群。
- [termius](https://termius.com/), 快速连接到集群，可以自定义一些脚本，方便的👊执行重复性命令，支持文件上传等功能。
- [nocalhost](https://nocalhost.dev/), 一个云原生的开发工具


---
layout: quote
---

# 最后😭

<br>

1. 非常感谢  <b class="text-amber-500">KubeSphere</b> 社区能够提供这次机会，让我们高校的学生参与到项目中去，收获匪浅。

2. 很感谢海立导师的辛苦指导、方阗老师提供的帮助以及各位老师的辛苦付出。

3. 祝  <b class="text-amber-500">KubeSphere</b> 越做越好！


---
src: ../../pages/end.md
---
