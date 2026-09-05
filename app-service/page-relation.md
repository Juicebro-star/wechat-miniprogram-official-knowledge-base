---
title: "页面间关系"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page-relation.html"
source_route: "/miniprogram/dev/framework/app-service/page-relation.html"
raw_html: "raw-html/app-service/page-relation.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page-relation.html](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page-relation.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/app-service/page-relation.html`

# 页面间关系

小程序的界面可以由多个页面组成。

页面之间可以相互跳转，如打开一个新页面，关闭一个已有页面等。这些页面切换的时机称为 [页面路由](route) 。

## 页面路径参数

最基本地，一个页面可以用 `router.navigateTo` 打开一个新页面。打开新页面时，可以携带路径参数。参数采用 URL 编码形式。

```
// page/index/index.js
Page({
  viewTap: function() {
    var id = 'PRODUCT#1'
    this.router.navigateTo({
      // 传入路径参数时，使用 encodeURIComponent 是个好习惯
      url: '../product/product?id=' + encodeURIComponent(id),
    })
  },
})
```

在新打开的页面中，通过 `this.options` 可以获取到页面的路径参数。

```
// page/product/product.js
Page({
  onLoad: function() {
    // this.options 中的路径参数需要 decodeURIComponent
    var id = decodeURIComponent(this.options.id) // id === 'PRODUCT#1'
  }
})
```

目前更推荐使用 [`Component` 构造器](../component-framework/scripting) 来构造页面。这样使用时，可以将路径参数定义在 `properties` 中。

```
// page/product/product.js
Component({
  properties: {
    id: String,
  },
  lifetimes: {
    attached() {
      // this.data.id === 'PRODUCT#1'
    },
  },
})
```

路径参数的本质是页面的启动状态。在实践中，如果页面内容是由某些数据决定的，那这些数据应当是一个路径参数。例如：

- 对于一个商品的详情页，页面所展示的商品由商品 ID 来决定，那么商品 ID 就应当是一个页面路径参数；
- 对于一个学生的个人信息页，页面所展示的是哪个学生由学号来决定，那么学号就应当是一个页面路径参数。

使用路径参数可以在新页面启动时，向新页面传递数据。但如果两个页面之间需要持续进行双向的数据通信，或者有些数据不适合写在路径参数中，那就需要用到 `EventChannel` 页面间通信。

## 页面间通信

> 基础库 2.7.3 开始支持，低版本需做[兼容处理](../compatibility)。

如果一个页面由另一个页面通过 `navigateTo` 打开，这两个页面间将建立一条数据通道：

- 被打开的页面可以通过 `this.getOpenerEventChannel()` 方法来获得一个 `EventChannel` 对象；
- `navigateTo` 的 `success` 回调中也包含一个 `EventChannel` 对象。

这两个 `EventChannel` 对象间可以使用 `emit` 和 `on` 方法相互发送、监听事件。

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/sYz82Kmy7Gfq "在开发者工具中预览效果")

