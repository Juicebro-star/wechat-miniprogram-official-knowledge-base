---
title: "注册页面"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html"
source_route: "/miniprogram/dev/framework/app-service/page.html"
raw_html: "raw-html/app-service/page.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/app-service/page.html`

# 注册页面

对于小程序中的每个页面，都需要在页面对应的 `js` 文件中进行注册，指定页面的初始数据、生命周期回调、事件处理函数等。

## Page 构造器

简单的页面可以使用 `Page` 构造器进行注册。

```
// page/index/index.js
Page({
  data: {
    text: "Initial data"
  },
  onLoad: function(options) {
    // 页面创建时执行
  },
  // 事件响应函数
  viewTap: function() {
    this.setData({
      text: 'Updated data'
    })
  },
})
```

详细的参数含义和使用请参考 [Page 参考文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page) 。

此外，对于比较复杂的页面，更推荐使用 `Component` 构造器来注册页面，具体细节请参考 [组件脚本](../component-framework/scripting) 。

## 页面跳转

> 基础库 2.16.1 开始支持，低版本需做[兼容处理](../compatibility)。

小程序的多个页面间可以相互跳转。其中，最常见的是打开一个新页面，此时可以使用 `navigateTo` 接口。

```
// page/index/index.js
Page({
  // 事件响应函数
  viewTap: function() {
    // 打开一个新页面（路径相对于当前页面）
    this.router.navigateTo({
      url: '../cart/cart'
    })
  },
})
```

新打开的页面路径上，必须有对应的 `.json`、`.wxml`、`.js` 文件。其中 `js` 文件也必须注册页面。

相应地，在新的页面中，可以使用 `navigateBack` 接口返回之前的页面。

```
// page/cart/cart.js
Page({
  // 事件响应函数
  viewTap: function() {
    // 返回之前的页面
    this.router.navigateBack()
  },
})
```

详细的参数含义和使用请参考 [Router 参考文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Router) 。

