---
title: "虚拟组件节点"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/virtual-host.html"
source_route: "/miniprogram/dev/framework/component-framework/virtual-host.html"
raw_html: "raw-html/component-framework/virtual-host.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/virtual-host.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/virtual-host.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/virtual-host.html`

# 虚拟组件节点

> 基础库 2.11.2 开始支持，低版本需做[兼容处理](../compatibility)。

默认情况下，自定义组件本身的那个节点是一个“普通”的节点，使用时可以在这个节点上设置 `class` `style`、动画、flex 布局等（就如同普通的 view 组件节点）。

```
<!-- 页面的 WXML -->
<view style="display: flex">
  <custom-component style="color: blue; flex: 1">蓝色、满宽的</custom-component>
</view>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/bBsAgwm582bu)

## 启用组件节点虚拟化

有时，自定义组件并不希望这个节点本身响应 flex 布局，而是希望自定义组件内部的第一层节点能够响应 flex 布局。还有些时候，自定义组件不希望这个节点本身是一个“普通“可以设置样式的节点。

这种时候，可以将这个自定义组件设置为“虚拟的”：

```
export default Component({
  options: { virtualHost: true },
})
```

这样，可以将 flex item 放入自定义组件内部：

```
<!-- 页面的 WXML -->
<view style="display: flex">
  <!-- 如果设置了 virtualHost ，节点上的样式将失效 -->
  <custom-component style="color: blue">不是蓝色的</custom-component>
</view>
```

```
<!-- custom-component.wxml -->
<view style="flex: 1"> 满宽的 </view>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Yg8WRvmV8TbT)

## 保留 class 和 style 能力

对于虚拟的组件节点，它的 `class` `style` 和动画将不再生效，但仍然可以：

- 将 `style` 定义成属性来获取 `style` 上设置的值；
- 将 `class` 定义成 [外部样式类](external-classes) 使得自定义组件 WXML 可以使用 `class`。

```
export default Component()
  .options({ virtualHost: true })
  .property('style', String)
  .externalClasses(['class'])
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/3N8wPvmq8Tbe)

