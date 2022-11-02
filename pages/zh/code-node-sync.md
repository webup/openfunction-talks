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

```yaml {all|8-15|16-17|18-} {maxHeight:'300px'}
apiVersion: core.openfunction.io/v1beta1
kind: Function
metadata:
  name: sample-node-knative
spec:
  version: v2.0.0
  image: '<image-repo>/<image-name>:<image-tag>'
  build:
    builder: openfunction/builder-node:v2-16.15
    env:
      FUNC_NAME: tryKnative
    srcRepo:
      url: https://github.com/OpenFunction/samples.git
      sourceSubPath: functions/async/mqtt-io-node
      revision: main
  # app port default to "8080"
  port: 8080
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
