---
title: "介绍"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html"
source_route: "/miniprogram/dev/framework/view/wxml/event.html"
raw_html: "raw-html/view/wxml/event.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/view/wxml/event.html`

# 介绍

当用户在视图层上进行操作的时候，例如点击了某个按钮、提交某个表单等，就会生成事件。此外，非用户触发的界面变化，也可能生成事件。常见的事件包括：

- [触摸与点击事件](../tap)
- [动画事件](../animation)
- 由各个组件生成的事件（仅对特定的组件有效）

## 事件处理

在 WXML 中，可以绑定事件处理函数。

最常见的用户事件是 `bind:tap`，表示用户点击了这个界面区域。

```
<view id="clickMe" bind:tap="handleTap"> Click me! </view>
```

在逻辑层，可以编写对应的事件处理函数。在事件处理函数中，可以获得事件的详细信息。

```
Page({
  handleTap: function(event) {
    event.type // 'tap'
    event.currentTarget.id // 'clickMe'
  }
})
```

每当用户在 view 上点击一次，`bind:tap` 对应的 `handleTap` 函数就会被调用一次。

事件处理函数收到的 `event` 对象有很多属性，具体请参考 [事件对象](../../component-framework/event-object) 。

绝大多数情况下，事件处理都采用上面这种方式。在特殊情况下，也可以使用 WXS 在视图层处理事件，具体请参考 [WXS 响应事件](../interactive-animation) 。

> `bind` 后的 `:` 多数情况下可以省略，但这是过时的写法，已不推荐。

## 事件冒泡

由用户点击触发的 `bind:tap` 事件属于 **冒泡事件** 。它会沿着节点树不断向祖先节点传导。

例如，对于下面这样的 WXML 结构：

```
<view id="outer" bind:tap="handleTap1">
  outer view
  <view id="middle" bind:tap="handleTap2">
    middle view
    <view id="inner" bind:tap="handleTap3">
      inner view
    </view>
  </view>
</view>
```

如果用户点击在 `<view id="inner">` 上，那么：

- `handleTap3` 函数就会被调用；
- 紧接着，事件会被冒泡到 `<view id="middle">`，`handleTap2` 会随之被调用；
- 最后，事件会被冒泡到 `<view id="outer">`，`handleTap1` 会随之被调用。

这就是冒泡事件的冒泡过程。

## 阻止事件冒泡

有些时候，需要在中间的某个节点上阻断事件的冒泡过程。例如，上述的 `handleTap3` 触发之后，往往就不希望 `handleTap2` 和 `handleTap1` 再被触发。这时需要用 `catch:` 代替 `bind:`：

```
<view id="outer" bind:tap="handleTap1">
  outer view
  <view id="middle" bind:tap="handleTap2">
    middle view
    <view id="inner" catch:tap="handleTap3">
      inner view
    </view>
  </view>
</view>
```

这样，如果用户点击在 `<view id="inner">` 上，那就只有 `handleTap3` 被触发；如果用户点击在 `<view id="middle">` 上，那么 `handleTap2` 和 `handleTap1` 会被依次触发。

在实践中，如果事件处理函数本身的逻辑是排他的，那无论其他节点如何，写 `catch:` 都是个比较好的选择。

除了 `bind:` 和 `catch:`，还有其他绑定前缀可以选用，具体请参考 [事件绑定](../../component-framework/event-binding) 。

