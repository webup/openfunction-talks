---
hideInToc: true
download: 'https://github.com/webup/openfunction-talks/raw/main/203-node-async/203-node-mixed.pdf'
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
  ## OpenFunction Tutorials
  
  Hands-on tutorial on Node.js Functions Framework

  Learn more at [OpenFunction](https://openfunction.dev/)
# persist drawings in exports and build
drawings:
  persist: false
---

# OpenFunction 203

Triggering Async Function From HTTP Request

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/webup/openfunction-talks/tree/main/203-node-mixed" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
  <a href="https://github.com/webup/openfunction-talks/raw/main/203-node-mixed/203-node-mixed.pdf" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
layout: intro
hideInToc: true
---

# Haili Zhang

<div class="leading-8 opacity-80">
KubeSphere Ambassador, CNCF OpenFunction TOC Memeber.<br>
Cloud Platform Director of UISEE&copy; Technology.<br>
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

<img src="https://s2.loli.net/2022/05/08/XRTdnohGA9rlewu.png" class="rounded-full w-40 abs-tr mt-16 mr-12"/>

---
hideInToc: true
---

# Table of Content

<Toc listClass="!list-disc"/>

---
title: Tricky Part of Async Function
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/aiOvN37uC1OEIrKh6yGADkuuyLPuaGFG4bc4OTl6TrL3h10zvCt2MbG020YB
---

## Tricky Part of Async Func

<br>

###### Pros

* By leveraging Dapr, async function could nearly connect with everthing ...
* Check [Dapr Bindings](https://docs.dapr.io/reference/components-reference/supported-bindings/)
* Check [Dapr PubSub Brokers](https://docs.dapr.io/reference/components-reference/supported-pubsub/)

<br>

###### Cons

* NEED ***events*** to pull the trigger
  * Usually coming in from external systems, or interface with external systems
  * e.g. message queue, cron, K8s events
* Also rather unfriendly for testing
  * We need a MQTT client tool to serve as the event publisher, remember [that](https://openfunction-talks.netlify.app/2022/202-node-async/13)?


---
title: HTTP Comes to the Rescue
layout: iframe-right
url: https://embeds.onemodel.app/d/iframe/QDvoHGiYvG9o2SjZB7bP0VJ63fT2UfM4ovw7jhWX2VA2rSwgUJ4MuwKA2TRQ
---

## HTTP Comes to the Rescue

<br>

###### Idea

* Use HTTP sync function as trigger
  * `POST` data to one sync function
  *  Sync function bridge data to event hub
  *  With async function subscribed to hub  

<br>

###### Solution

* Enable Dapr output binding and pub in sync function runtime [^1]
* Check the figure aside as an example 👉

[^1]: Feature introduced in [OpenFunction 0.6.0](https://github.com/OpenFunction/OpenFunction/releases/tag/v0.6.0) along with [Node.js Function Framework 0.5.0](https://github.com/OpenFunction/functions-framework-nodejs/releases/tag/v0.5.0)

---
layout: statement
hideInToc: true
---

# Invoke Async in A Sync Way!

---
title: Invoke Async in A Sync Way
layout: two-cols
---

## Same function signature

Exactly the same async function [signature](https://openfunction-talks.netlify.app/2022/202-node-async/5).

```js
function (ctx, data) {}
```

> `data`: Request [body](https://devdocs.io/express/index#req.body) data, parsed as JSON by default

<br>

###### Extensions

* `ctx.req`: The pass-through Express standard HTTP [request](https://devdocs.io/express-request/) object
* `ctx.res`: The pass-through Express standard HTTP [response](https://devdocs.io/express-response/) object
* These two methods are optional for use, even no response made by `ctx.res` would NOT block the HTTP request handler

::right::

## Specific signature type

New type `openfunction` ready to work.

* OpenFunction Function CRD:

```yaml{9-}
apiVersion: core.openfunction.io/v1beta1
kind: Function
metadata:
  name: func-name
spec:
  serving:
    # default to knative
    runtime: knative
    params:
      # default to http
      FUNCTION_SIGNATURE_TYPE: openfunction
```

* CLI: `--signature-type=openfunction`
* ENV: `FUNCTION_SIGNATURE_TYPE=openfunction`

---
layout: center
class: text-center
---

# <icomoon-free-lab class="opacity-70"/> Lab: HTTP Trigger MQTT Bindings

---
title: Sample Function and Manifest
level: 2
layout: two-cols
---

# Sample Function

Another plain forwarder

```js
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

> Remember to build the function, either when applying Function CR manifest or directly via [Pack](https://openfunction-talks.netlify.app/2022/202-node-async/7) CLI tool

::right::

# Sample CR Manifest

Check out full content in [http-trigger.yml](https://github.com/OpenFunction/samples/blob/main/functions/knative/with-output-binding-node/http-trigger.yaml)

```yaml{4-7|8-}
spec:
  image: '<image-repo>/<image-name>/<image-tag>'
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
title: Deploy Sample Function
level: 2
---

# Deploy Sample Function

* Apply function <Link to="8" title="manifest"/>, and check running states

  ```bash
  $ kubectl apply -f http-trigger.yaml
  function.core.openfunction.io/sample-node-http-trigger created

  $ kubectl get fn
  NAME                         BUILDSTATE   SERVINGSTATE   BUILDER   SERVING         URL                                                         AGE
  sample-node-http-trigger     Skipped      Running                  serving-7d75d   http://openfunction.io/default/sample-node-http-trigger     6d

  $ kubectl get deploy
  NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
  ...
  serving-7d75d-ksvc-xn768-v200-deployment   0/0     0            0           6d14h
  ...
  ```

<br>

> Knative runtime would autoscale the deployment replica to `0`, if no incoming request for a period of time.


---
title: HTTP~ Fire!
level: 2
layout: image-right
image: images/mqttx-out.jpg
---

# HTTP~ Fire!

Send request to domain ingress.

* `POST` data to target function URL:
  
  ```bash
  curl -d '{"hello":"ofn"}' \
       -H 'Content-Type: application/json' \
       http://openfunction.io/default/sample-node-http-trigger
  ```

* Wait a second (may be few more seconds for cold start), you would get results:
  * From terminal: `{"hello":"ofn"}`
  * From [MQTT X](https://mqttx.app/) [^1] desktop client 👉

[^1]: See also: OpenFunction 202 - [Set up MQTT Broker](https://openfunction-talks.netlify.app/2022/202-node-async/9)

---
layout: center
class: text-center
hideInToc: true
---

# <uim-rocket class="text-3xl"/> Learn More

<bi-discord /> [Discord](https://discord.gg/awpAh8W5xW) · <bi-slack /> [Slack](https://cloud-native.slack.com/archives/C03ETDMD3LZ)

<br>

[OpenFunction](https://openfunction.dev/) · [Node.js Functions Framework](https://github.com/OpenFunction/functions-framework-nodejs)
