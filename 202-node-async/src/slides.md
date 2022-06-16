---
hideInToc: true
download: 'https://github.com/webup/openfunction-talks/raw/main/202-node-async/202-node-async.pdf'
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

# OpenFunction 202

Node.js Async Function Quickstart

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/webup/openfunction-talks/tree/main/202-node-async" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
  <a href="https://github.com/webup/openfunction-talks/raw/main/202-node-async/202-node-async.pdf" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
hideInToc: true
layout: intro
---

# Haili Zhang

<div class="leading-8 opacity-80">
KubeSphere Ambassador, CNCF OpenFunction TOC Member.<br>
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

<Toc listClass="!list-disc" maxDepth="2" />

---

# Prerequsites

Use [`ofn`](https://github.com/OpenFunction/cli) [^1] [^2] CLI tool to deploy OpenFunction.

* Install OpenFunction with Async Runtime only [^3]

```bash
$ ofn install --async
```

* Install OpenFunction with Async Runtime and function build framework

```bash
$ ofn install --async --shipwright
```

[^1]: Add `--region-cn` option in case you have limited access to gcr.io or github.com
[^2]: Use `--dry-run` to peek the components and their versions to be installed by the current command
[^3]: Please refer <Link to="7" title="this" /> to learn how to build function image at local

---
hideInToc: true
layout: center
class: text-center
---

# Your First Async Function

A brief walkthrough

---
title: Your First Async Function
---

<div class="grid grid-cols-2 gap-x-8"><div>

# Async <MarkerVersion version="0.4.1+" />

```js
function (ctx, data) {}
```
<br>

<div v-click>

###### Parameters

* `ctx`: OpenFunction [context](https://github.com/OpenFunction/functions-framework-nodejs/blob/master/src/openfunction/function_context.ts) object
  * `ctx.send(payload, output?)`: Send `payload` to all or one specific `output` of Dapr Output [Binding](https://docs.dapr.io/reference/components-reference/supported-bindings/) or Pub [Broker](https://docs.dapr.io/reference/components-reference/supported-pubsub/)
* `data`: Data recieved from Dapr Input Binding or Sub Broker

###### Notice

* `ctx.send` CAN be invoked where necessary, when you have certain outgoing data to send

</div>

</div><div>

# (HTTP) Sync

```js
function (req, res) {}
```

<br>

<div v-click>

###### Parameters

* `req`: Express standard [request](https://devdocs.io/express-request/) object 
* `res`: Express standard [response](https://devdocs.io/express-response/) object 
  * `res.send(body)`: Use this method to send HTTP response in most common cases

###### NOTICE
* Response process SHOULD be explictly ended with `res.send()`, `res.json()`, `res.end()` and alike methods

</div>

</div></div>

---
title: A sample async function
level: 2
---

# A sample function: `tryAsync`

<div class="grid grid-cols-2 gap-x-4">

<v-clicks :every='2'>

<div>

###### index.mjs

```js
// Async function
export const tryAsync = (ctx, data) => {
  console.log('Data received: %o', data);
  ctx.send(data);
};

// HTTP sync function
export const tryKnative = (req, res) => {
  res.send(`Hello, ${req.query.u || 'World'}!`);
};
```

<br>

</div><div>

###### package.json

```json
{
  "main": "index.mjs",
  "scripts": {
    "start": "functions-framework --target=tryKnative"
  },
  "dependencies": {
    "@openfunction/functions-framework": "^0.4.1"
  }
}
```

</div>

<div class="col-span-2">

###### Notice

* Serveral async and sync functions CAN be placed in ONE SINGLE JavaScript file
  * Target function CAN be assigned when applying Function CR <Link to="11" title="manifest" />
* In `package.json`, `sciprts` and `dependencies` sections could be omitted
  * [@openfucntion/openfunction-framework](https://www.npmjs.com/package/@openfunction/functions-framework/v/0.4.1) lib would be automatically added during build
  * `start` script is highly recommeneded for local development 

</div>

</v-clicks>

</div>

---
title: Build Function Image via Pack <MarkerOptional />
level: 2
---

# Build Function Image via Pack <MarkerOptional />

Local build is recommended if your Kubernetes nodes have limited access to GitHub or Docker Hub.

1. Install Cloud Native Buildpacks project's [Pack](https://buildpacks.io/docs/tools/pack/) CLI tool

1. Use `pack` tool to build your function image at local [^1]

    ```bash {all|1|2|3|all}
    pack build -B openfunction/builder-node:v2-16.13 \    # Builder image, `16` is the latest version   
               -e FUNC_NAME=tryKnative \                  # Default entry point function 
               -p src \                                   # Path of source files to be built
               <image-repo>/<image-name>:<tag>
    ```

1. Push function image to target container repository (e.g. Docker Hub)

    ```bash
    docker push <image-repo>/<image-name>:<tag>
    ```

[^1]: `pack` tool would download builder image during the build process

---
layout: center
class: text-center
---

# <icomoon-free-lab class="opacity-70"/> Lab: [MQTT](https://www.emqx.com/en/mqtt) Forwarder

Use async function to bridge MQTT messages among topic channels

---
title: Set up MQTT Broker <MarkerOptional />
level: 2
---

# Set up MQTT Broker <MarkerOptional />

In this lab, we will use [EMQX](https://www.emqx.io/) as the broker infrastructure. Learn [full steps](https://www.emqx.com/en/blog/rapidly-deploy-emqx-clusters-on-kubernetes-via-helm).

1. Add EMQX Helm Chart repository

    ```bash
    helm repo add emqx https://repos.emqx.io/charts
    helm repo update
    ```

1. Search available charts of EMQX 

    ```plain
    helm search repo emqx

    NAME               CHART VERSION APP VERSION DESCRIPTION
    emqx/emqx          4.4.3         4.4.3       A Helm chart for EMQX
    emqx/emqx-ee       4.4.3         4.4.3       A Helm chart for EMQ X
    ```

  1. Deploy single replica of EMQX, and expose NodePort service
  
     ```bash
     helm install emqx emqx/emqx --set replicaCount=1 --set service.type=NodePort
     ```


---
hideInToc: true
layout: image
image: images/emqx-dashboard.png
class: text-center
---

###### EQMX Dashboard Screenshot

---
title: "Lab 1: MQTT Input and Output Binding"
level: 2
layout: two-cols
---

```yaml {all|9-10|11-13|17-19|20-26}
apiVersion: core.openfunction.io/v1beta1
kind: Function
metadata:
  name: sample-node-async-bindings
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

```

::right::

<v-click>

```yaml
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

> * Dapr Component - Bindings - [MQTT](https://docs.dapr.io/reference/components-reference/supported-bindings/mqtt/)
> * OpenFunction - Function CRD - [DaprIO](https://openfunction.dev/docs/reference/component-reference/function-spec/#daprio)
> * Check full [sample](https://github.com/OpenFunction/samples/tree/main/functions/async/mqtt-io-node) codes

</v-click>

<style>
.slidev-code {
  @apply !rounded-none;
}
</style>

---
title: Deploy Function 
level: 3
---

# Lab 1: MQTT Input and Output Binding

* Apply function <Link to="10" title="manifest"/>, and check running states

  ```bash
  $ kubectl apply -f async-bindings.yaml
  function.core.openfunction.io/sample-node-async-bindings created

  $ kubectl get fn
  NAME                         BUILDSTATE   SERVINGSTATE   BUILDER   SERVING         URL      AGE
  sample-node-async-bindings   Skipped      Running                  serving-8f7xc            140m

  $ kubectl get po
  NAME                                                   READY   STATUS    RESTARTS   AGE
  serving-8f7xc-deployment-v200-l78xc-564c6b5bf7-vksg7   2/2     Running   0          141m
  ```

* Furthermore, check whether `function` container output correct logs

  ```bash 
  $ kubectl logs -c function serving-8f7xc-deployment-v200-l78xc-564c6b5bf7-vksg7
  ...
  [Dapr-JS] Listening on 8080
  [Dapr-JS] Letting Dapr pick-up the server (Maximum 60s wait time)
  [Dapr-JS] - Waiting till Dapr Started (#0)
  [Dapr-JS] Server Started
  ```

---
title: Trigger Event
level: 3
layout: image-right
image: images/mqttx-in-out.png
---

# Lab 1: Trigger Event

See also: [MQTT X](https://mqttx.app/) desktop client

* Connect EQMX server via NodePort mapped to `tcp:1883`
* Publish `{"msg": "hello"}` to `in` topic
  * Payload received in `out` topic 👇 👉
* Check the `function` container log

  ```bash
  $ kubectl logs -c function serving-8f7xc-deployment-v200-l78xc-564c6b5bf7-vksg7
  ...
  [Dapr-JS] Listening on 8080
  [Dapr-JS] Letting Dapr pick-up the server (Maximum 60s wait time)
  [Dapr-JS] - Waiting till Dapr Started (#0)
  [Dapr-JS] Server Started
  Data received: { msg: 'hello' }
  ```

---
title: "Lab 2: MQTT Pub and Sub"
level: 2
layout: two-cols
---

```yaml {all|9-10|11-13|15-17|18-25}
apiVersion: core.openfunction.io/v1beta1
kind: Function
metadata:
  name: sample-node-async-pubsub
spec:
  version: v2.0.0
  image: '<image-repo>/<image-name>:<tag>'
  serving:
    # default to knative
    runtime: async
    annotations:
      # default to "grpc"
      dapr.io/app-protocol: http
    template: ...
    params:
      # default to FUNC_NAME value
      FUNCTION_TARGET: tryAsync
    inputs:
      - name: mqtt-sub
        component: mqtt-pubsub
        topic: sub
    outputs:
      - name: mqtt-pub
        component: mqtt-pubsub
        topic: pub

```

::right::

<v-click>

```yaml
    pubsub:
      mqtt-pubsub:
        type: pubsub.mqtt
        version: v1
        metadata:
          - name: consumerID
            value: '{uuid}'
          - name: url
            value: tcp://admin:public@emqx:1883
          - name: qos
            value: 1
```

> * Dapr Component - Pub/Sub Brokers - [MQTT](https://docs.dapr.io/reference/components-reference/supported-pubsub/setup-mqtt/)
> * OpenFunction - Function CRD - [DaprIO](https://openfunction.dev/docs/reference/component-reference/function-spec/#daprio)
>   * `topic` field is required for pubsub component
> * Check full [sample](https://github.com/OpenFunction/samples/tree/main/functions/async/mqtt-io-node) codes

</v-click>

<style>
.slidev-code {
  @apply !rounded-none;
}
</style>

---
title: Deploy Function
level: 3
---

# Lab 2: MQTT Pub and Sub

* Apply function <Link to="14" title="manifest"/>, and check running states

  ```bash
  $ kubectl apply -f async-pubsub.yaml
  function.core.openfunction.io/sample-node-async-pubsub created

  $ kubectl get fn
  NAME                         BUILDSTATE   SERVINGSTATE   BUILDER   SERVING         URL      AGE
  sample-node-async-pubsub     Skipped      Running                  serving-2qfkl            140m

  $ kubectl get po
  NAME                                                   READY   STATUS    RESTARTS   AGE
  serving-2qfkl-deployment-v200-6cshf-57c8b5b8dd-ztmbf   2/2     Running   0          141m
  ```

* Furthermore, check whether `function` container output correct logs

  ```bash
  $ kubectl logs -c function serving-2qfkl-deployment-v200-6cshf-57c8b5b8dd-ztmbf
  ...
  [Dapr-JS] Listening on 8080
  [Dapr-JS] Letting Dapr pick-up the server (Maximum 60s wait time)
  [Dapr-JS] - Waiting till Dapr Started (#0)
  [Dapr API][PubSub] Registered 1 PubSub Subscriptions
  [Dapr-JS] Server Started
  ```

---
title: Trigger Event
level: 3
layout: image-right
image: images/mqttx-pub-sub.png
---

# Lab 2: Trigger Event

* Connect EQMX server via NodePort mapped to `tcp:1883`
* Publish a [CloudEvents](https://cloudevents.io/) event to `pub` topic
  * CloudEvents payload got in `sub` 👉
  * Pure data recieved in async function 👇
* Check the `function` container log

  ```bash
  $ kubectl logs -c function serving-2qfkl-deployment-v200-6cshf-57c8b5b8dd-ztmbf
  ...
  [Dapr-JS] Listening on 8080
  [Dapr-JS] Letting Dapr pick-up the server (Maximum 60s wait time)
  [Dapr-JS] - Waiting till Dapr Started (#0)
  [Dapr API][PubSub] Registered 1 PubSub Subscriptions
  [Dapr-JS] Server Started
  Data received: { orderId: '100' }
  ```

---
hideInToc: true
layout: center
class: text-center
---

# <uim-rocket class="text-3xl"/> Learn More

<bi-discord /> [Discord](https://discord.gg/awpAh8W5xW) · <bi-slack /> [Slack](https://cloud-native.slack.com/archives/C03ETDMD3LZ)

<br>

[OpenFunction](https://openfunction.dev/) · [Node.js Functions Framework](https://github.com/OpenFunction/functions-framework-nodejs)
