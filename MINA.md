---
title: "框架"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/MINA.html"
source_route: "/miniprogram/dev/framework/MINA.html"
raw_html: "raw-html/MINA.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/MINA.html](https://developers.weixin.qq.com/miniprogram/dev/framework/MINA.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/MINA.html`

# 框架

小程序开发框架的目标是通过尽可能简单、高效的方式让开发者可以在微信中开发具有原生 APP 体验的服务。

整个小程序框架系统分为两部分：**[逻辑层](app-service/index)**（App Service）和 **[视图层](view/)**（View）。小程序提供了自己的视图层描述语言 `WXML` 和 `WXSS`，以及基于 `JavaScript` 的逻辑层框架，并在视图层与逻辑层间提供了数据传输和事件系统，让开发者能够专注于数据与逻辑。

数据绑定将逻辑层与视图层关联起来。当做数据修改的时候，只需要在逻辑层修改数据，视图层就会做相应的更新。

通过这个简单的例子来看：

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/l0gLEKmv6gZa "在开发者工具中预览效果")

```
<!-- 视图层中的模板 -->
<view> Hello {{name}}! </view>
<button bindtap="changeName"> Click me! </button>
```

```
// 逻辑层中的页面
Page({
  // 初始数据
  data: {
    name: 'Weixin'
  },
  // 事件处理函数
  changeName: function(e) {
    // 改变数据
    this.setData({
      name: 'MINA'
    })
  }
})
```

- 通过框架将逻辑层数据中的 `name` 与视图层的 `name` 进行了绑定，所以在页面一打开的时候会显示 `Hello Weixin!`；
- 当点击按钮的时候，视图层会发送 `changeName` 的事件给逻辑层，逻辑层找到并执行对应的事件处理函数；
- 回调函数触发后，逻辑层执行 `setData` 的操作，将 `data` 中的 `name` 从 `Weixin` 变为 `MINA`，因为该数据和视图层已经绑定了，从而视图层会自动改变为 `Hello MINA!`。

除了数据绑定以外，框架还提供了页面管理功能、一套基础的组件、丰富的微信原生 API。

- 框架维护着整个小程序的页面路由，可以做到页面间的无缝切换，并给以页面完整的生命周期。
- 不同基础组件自身逻辑也各不相同，可以通过组合基础组件，创建出强大的小程序。
- 框架还提供了丰富的微信原生 API，它们具备微信提供的强大能力，如获取用户信息，本地存储，支付功能等。

