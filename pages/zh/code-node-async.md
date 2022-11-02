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
  // Function Framework received and parsed "data"  
  console.log('Data received: %o', data);
  // Use "send" utility to broadcast data to outputs
  ctx.send(data);
};
```

> 💡 多个函数可以被放在同一个 JS 文件中，并在 CR 中动态指定

::right::

###### Function CR (Raw Manifest)

```yaml {all|9-10|11-13|17-19|20-26|27-} {maxHeight:'300px'}
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

> 🔍 OpenFunction 函数定义中的输入输出部分：可以直接引用 [Dapr Components](https://docs.dapr.io/reference/components-reference/) 的元数据定义
