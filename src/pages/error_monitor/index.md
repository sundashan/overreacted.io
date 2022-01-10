---
title: 错误监控
date: 2021-09-09 16:55:49
spoiler: 省略.
cta: 'js'
---

这边用的是sentry，很久之前简单用过，但是复杂的没加进来，最近又拿出来看了看

### 1. 地址
```
地址：https://sentry.io
```
### 2. 安装,去官网文档中选择，根据项目类型不同，安装不同，比如vue的如下
```jsx
yarn add @sentry/vue @sentry/tracing
```

```jsx
// main.js文件中,vue2和vue3不大一样,具体可以看官网
import * as Sentry from "@sentry/vue";
import { Integrations } from "@sentry/tracing";

Sentry.init({
  Vue,
  dsn: "https://ba1310a8c52248aba183ade5c8984faa@o218396.ingest.sentry.io/5951817",
  release:"v1.0.1",
  integrations: [
    new Integrations.BrowserTracing({
      routingInstrumentation: Sentry.vueRouterInstrumentation(router),
      tracingOrigins: ["localhost", "my-site-url.com", /^\//],
    }),
  ],
  tracesSampleRate: 1.0,
});
```

### 3. 新建.sentryclirc
```jsx
[defaults]
url=https://sentry.io/
org=xxx
project=xxx
[auth]
token=xxx
// token的获取位置是Settings——>Account——>API——>Auth Tokens
// 默认没有令牌,需要创建,project:write选项一定要勾
--log-level=[info|debug]
```

### 4. 配置sourceMap,没有配置sourceMap的时候,错误定位不能具体到某一行,整体看到的是一个bundle文件
```jsx
webpack.config.js文件
const SentryWebpackPlugin = require("@sentry/webpack-plugin");
module.exports = {
    devtool: 'source-map'
    plugins: [
        new SentryWebpackPlugin({
            release:"v1.0.1", // 这边的版本号对应下面👇创建的
            include: ".",
            ignoreFile: ".sentrycliignore",
            ignore: ["node_modules", "webpack.config.js"],
            configFile: "sentry.properties",
            urlPrefix:"~/static/js"
        })
    ]
}
```
release可能会报错,需要先
```jsx
    yarn add @sentry/cli
    sentry-cli releases -o <org名字> -p <project名字> new v1.0.1
```

### 5. 清除sourceMap
```jsx
yarn add @sentry/webpack-plugin
// 具体使用也是在webpack里面引入
```

### 6. 错误类型
待补充

### 7. 手动上报
待补充