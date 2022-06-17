---
hideInToc: true
title: OpenFunction 101
# try also 'default' to start with a default theme
theme: light-icons
# random image from a curated Unsplash collection by Anthony
layout: intro
image: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
# highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## OpenFunction Tutorials
  
  OpenFunction overview introduction

  Learn more at [OpenFunction](https://openfunction.dev/)
# persist drawings in exports and build
drawings:
  persist: false
---

  <div class="absolute pt-6 left-12">
    <span @click="next" class="p-1 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 hover:opacity-90 opacity-60 flex justify-center items-center">
      Press Space for next page  <light-icon icon="arrow-narrow-right" size="24px"/>
    </span>
  </div>

  <div class="mb-4 absolute bottom-4 left-12 text-left">
    <span class="text-6xl text-primary-lighter text-opacity-80" style="font-weight:500;" >
      OpenFunction 101 <light-icon icon="bike"/>
    </span>
    <div class="text-9xl text-white text-opacity-60" style="font-weight:600;" >
      OVERVIEW
    </div>
  </div>

---
hideInToc: true
layout: image-left
image: 'https://s2.loli.net/2022/05/08/XRTdnohGA9rlewu.png'
equal: true
---

# Haili Zhang

<div class="leading-8 opacity-80">
CNCF OpenFunction TOC Member, KubeSphere Ambassador.<br>
Cloud Platform Director of UISEE&copy;, a leading autonomous driving company.<br>
Cloud Native focuses: Kubernetes, DevOps, Observability, Service Mesh, Serverless.<br>
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

---
hideInToc: true
---

# Table of Content

<Toc listClass="!list-disc" maxDepth="2" />

---
title: Learnings from CNCF and Datadog Reports
layout: two-cols
---

# CNCF Survey 2021

* Container Adoption and Kubernetes have <b class="text-amber-500">truly gone mainstream</b> – usage has risen across organizations globally, particularly in large businesses.
* According to CNCF’s respondents, <b class="text-amber-500">96% of organizations are either using or evaluating Kubernetes</b> – a record high since our surveys began in 2016.
* Kubernetes has demonstrated impressive growth over the past 12 months with <b class="text-amber-500">5.6 million developers</b> using Kubernetes today.

::right::

<Tweet id="1491819509484822536" class="w-2/3 m-auto" />

---
hideInToc: true
layout: two-cols
---

# Serverless Boosts 🚀

"As the popularity of FaaS products like AWS Lambda continues to grow, we have also seen a significant increase in adoption of other types of serverless technologies offered by Azure, Google Cloud, and AWS."

![Serverless adoption by cloud provider](https://imgix.datadoghq.com/img/state-of-serverless/2022-serverless-report-charts_FACT-1.png)

::right::

<Tweet id="1532453256647131153" class="w-2/3 m-auto"/>

---
hideInToc: true
layout: image
image: https://www.cncf.io/wp-content/uploads//2022/02/p9_Repor-01-10-1.svg
class: bg-slate-50 bg-origin-content
---

# Serverless Platform War?

"Hosted platforms (75%) are the most popular"

<style>
h1,p {
  @apply !text-red-500;
}
</style>

---

# Why need cloud agnostic Serverless platform?

Kubernetes brings the possibility of cloud-agnostic: multiple, distributed cloud.

<br>

But it’s difficult to be cloud-agnostic for Serverless
* Each cloud provider has its own Serverless platform
* Further more, these platforms are coupled with their own cloud backend services

<br>

Is it possible to build a cloud agnostic Serverless platform? 🤔

---
title: How to build a cloud agnostic Serverless platform?
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/aWmQ2ljcbrB0R0nqvdoVZEojQWRhSYxthan97wncdULJmUalBkURa06aNAfx
---

# What is Serverless?

Let's make core concepts clear first.

<b class="text-amber-500">"Serverless computing = FaaS + BaaS."</b> [^1]

* Cloud functions — packaged as FaaS (Function as a Service) offerings — represent the core of serverless computing
* Cloud platforms also provide specialized serverless frameworks that cater to specific app requirements as BaaS (Backend as a Service) offerings

[^1]: [Cloud Programming Simplified: A Berkeley View on Serverless Computing](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2019/EECS-2019-3.pdf). Feb 10, 2019.

<style>
.footnotes-sep {
  @apply mt-5 opacity-10;
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
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/mOmUABwfsD0d6cgwGRyzt8vih5uoeXldJO4hyWdUUAjl2g7YeWeQf44kkQxh
---

# How to construct?

We need adapt various backend services.

[Dapr](https://dapr.io/) <b class="text-amber-500">decouples</b> the distributed apps with <b class="text-amber-500">underlying backend services</b>.

![Dapr Building Blocks](https://docs.dapr.io/images/building_blocks.png)

> Dapr provides you with APIs that abstract away the complexity of common challenges developers encounter regularly when building distributed applications. These API building blocks can be leveraged as the need arises - use one, several or all to develop your application faster and deliver your solution on time.

---
hideInToc: true
layout: iframe-right
url: https://keda.sh/docs/2.7/concepts/#architecture
---

# How to construct?

We need achieve "zero-scale".

[KEDA](https://keda.sh/) is a Kubernetes-based Event Driven Autoscaler. With KEDA, you can <b class="text-amber-500">drive the scaling of any container in Kubernetes based on the number of events</b> needing to be processed.

* KEDA is a single-purpose and lightweight component that can be added into any Kubernetes cluster. 
* KEDA works alongside standard Kubernetes components like the Horizontal Pod Autoscaler and can extend functionality without overwriting or duplication.

---
title: 'OpenFunction: Less codes. More Functions!'
layout: image-left
image: https://github.com/benjaminhuo/artwork/raw/master/projects/openfunction/icon/color/openfunction-icon-color.png
equal: true
---

[OpenFunction](https://openfunction.dev/) is a cloud-native open source FaaS (Function as a Service) platform aiming to let you focus on your business logic without having to maintain the underlying runtime environment and infrastructure.

<b class="text-amber-500">You can concentrate on developing business-related source code in the form of functions.</b>

---
layout: iframe
url: https://www.youtube.com/embed/YWZsXdAXFO8?start=580
---

---
title: Architecture Overview of OpenFunction
layout: center-image
image: https://openfunction.dev/openfunction-0.5-architecture.png
---

<div class="mb-4">
  <span class="text-3xl text-primary dark:text-slate-300 font-medium">Architecture Overview of OpenFunction</span>
</div>

---
layout: iframe
url: https://embeds.onemodel.app/d/iframe/RZ3djGBjCkM8p2aOjZnvf0UNWPRA5zPVxbk5d6AHl5sowHNhYk6qQLMvGmMB
---

---
layout: center
class: text-center
---

# <ant-design-book-outlined class="opacity-70"/> Use Case: Online Data Processing

Using OpenFunction in Autonomous Driving

---
layout: iframe
url: https://embeds.onemodel.app/d/iframe/abvQuuS6QoSkz1MPovXXslweJcsCPoPG73anxZoLu58lFe3UCIS58rytKe8H
---

---
hideInToc: true
---

# Why need a cloud agnostic Serverless platform?

### Why Cloud Agnostic

* Different customers require to use different cloud providers
* Some customers’ vehicle data is sensitive and are required to be isolated from public clouds

<br>

### Why Serverless

* Autonomous driving has so many application scenarios, there're requirements to use different processing logics for different scenarios even for the same data source
* Data types and processing modules are complex, multi-language support is required
* Massive events need to be handled in real time
* Data processing logics tend to be changed frequently based on rapidly changing requirements

---
layout: iframe
url: https://embeds.onemodel.app/d/iframe/QSsJOLQszQIxATltAMwPEpcjjLiWa6aZAuHM890P0eBdXN7lXsXehj1Lg5GA
---

---
hideInToc: true
layout: two-cols
class: m-3
---

# Dapr Matters

* Data from different sensors and processing modules· are complex
* Different cloud providers have different backend services that may result in duplicated implementations over the same data processing logic
* Pub/Sub and Bindings make it easy to set up event-driven architecture for data processing with async functions

::right::

# KEDA Matters

* AI drivers need to retain human working habits to a certain extent, so there are obvious peaks and valleys in traffic based on working schedules
* For security reasons, data refresh frequently and sensors produce massive data, so workload should be scalable
* The usage of compute resources directly affects project cost

---
hideInToc: true
layout: center-image
---

<div class="mb-0">
  <span class="text-3xl text-primary uppercase font-semibold dark:text-primary ">Explore More</span>
</div>
<div class="mb-0">
  <a href="https://openfunction.dev/" target="_blank">Documentation</a> |
  <a href="https://github.com/openfunction" target="_blank">Contribution</a>
</div>

<style>
a {
  @apply m-1 hover:opacity-70;
}
</style>
