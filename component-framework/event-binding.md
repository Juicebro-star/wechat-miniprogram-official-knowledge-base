---
title: "事件绑定"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/event-binding.html"
source_route: "/miniprogram/dev/framework/component-framework/event-binding.html"
raw_html: "raw-html/component-framework/event-binding.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/event-binding.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/event-binding.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/event-binding.html`

# 事件绑定

常用的事件绑定类型是 `bind` 和 `catch`。它们的区别是，`catch` 会阻止事件冒泡，可参考 [事件](../view/wxml/event#阻止事件冒泡) 。

## 在事件绑定中使用数据绑定

在事件绑定中，可以使用数据绑定来动态指定事件处理函数。

```
<view bind:tap="{{ handler }}">Click me</view>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/n8EU0wmQ8fbj)

此时，`this.data.handler` 应当是一个字符串，表示事件处理方法函数名。如果它是个空字符串，则这个绑定会失效：可以利用这个特性来暂时禁用一些事件。

## 互斥事件绑定

自基础库版本 [2.8.2](../compatibility) 起，除 `bind` 和 `catch` 外，还可以使用 `mut-bind` 来绑定事件。一个 `mut-bind` 触发后，如果事件冒泡到其他节点上，其他节点上的 `mut-bind` 绑定函数不会被触发，但 `bind` 绑定函数和 `catch` 绑定函数依旧会被触发。

换而言之，所有 `mut-bind` 是“互斥”的，只会有其中一个绑定函数被触发。同时，它完全不影响 `bind` 和 `catch` 的绑定效果。

例如在下边这个例子中，点击 inner view 会先后调用 `handleTap3` 和 `handleTap2` ，点击 middle view 会调用 `handleTap2` 和 `handleTap1` 。

```
<view id="outer" mut-bind:tap="handleTap1">
  outer view
  <view id="middle" bind:tap="handleTap2">
    middle view
    <view id="inner" mut-bind:tap="handleTap3">
      inner view
    </view>
  </view>
</view>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/PgEPXwmO8eba)

## 事件的捕获阶段

自基础库版本 [1.5.0](../compatibility) 起， [触摸与点击事件](../view/tap) 支持捕获阶段。捕获阶段位于冒泡阶段之前，且在捕获阶段中，事件到达节点的顺序与冒泡阶段恰好相反。需要在捕获阶段监听事件时，可以采用 `capture-bind`、`capture-catch` 关键字，后者将中断捕获阶段和取消冒泡阶段。

在下面的代码中，点击 inner view 会先后调用 `handleTap2`、`handleTap4`、`handleTap3`、`handleTap1`。

```
<view id="outer" bind:touchstart="handleTap1" capture-bind:touchstart="handleTap2">
  outer view
  <view id="inner" bind:touchstart="handleTap3" capture-bind:touchstart="handleTap4">
    inner view
  </view>
</view>
```

如果将上面代码中的第一个 `capture-bind` 改为 `capture-catch`，将只触发 `handleTap2`。

```
<view id="outer" bind:touchstart="handleTap1" capture-catch:touchstart="handleTap2">
  outer view
  <view id="inner" bind:touchstart="handleTap3" capture-bind:touchstart="handleTap4">
    inner view
  </view>
</view>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/ZSECTwmM8ebe)

