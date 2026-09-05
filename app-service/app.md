---
title: "注册小程序"
type: source_document
layer: L4
owner: external
claim_type: fact
status: archived
domain: mini_program
project: wechat_miniprogram_docs
source_type: official_web
visibility: public
retrieved: "2026-09-05"
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/app.html"
source_route: "/miniprogram/dev/framework/app-service/app.html"
raw_html: "raw-html/app-service/app.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/app.html](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/app.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/app-service/app.html`

# 注册小程序

小程序可以在 `app.js` 中调用 `App` 方法注册小程序实例。`app.js` 是整个小程序中最先被执行的 JavaScript 文件。

```
// app.js
App()
```

在 `App` 中可以提供一些参数，详细的参数含义和使用请参考 [App 参考文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App) 。不过，现在并不推荐使用 `App` 的参数，而是使用其他方式代替。

通常，可以将全局事件响应函数放在 `app.js` 中，例如：

```
App()

wx.onPageNotFound((res) => {
  // 当页面不存在时触发
})
```

全局变量和 JavaScript 模块并不推荐放在 `app.js` 中，而是推荐使用 [模块化](module) 的方式来维护。

