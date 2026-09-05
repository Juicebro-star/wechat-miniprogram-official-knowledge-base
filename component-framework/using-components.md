---
title: "引用组件"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/using-components.html"
source_route: "/miniprogram/dev/framework/component-framework/using-components.html"
raw_html: "raw-html/component-framework/using-components.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/using-components.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/using-components.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/using-components.html`

# 引用组件

一个由 JSON 配置、WXML 模板、WXSS 样式、JavaScript 脚本组成的自定义组件编写完成后，它就可以被其他组件引用了。

## 基本引用方式

在引用组件时，需要在 JSON 配置的 `usingComponents` 中指定引用的标签名和它对应的组件路径。下面用一个具体的例子来说明组件的引用方法。

首先，我们先编写一个自定义组件，位于 `components/foo`。首先编写 `components/foo.json`：

```
{
  "component": true,
  "usingComponents": {}
}
```

然后编写 `components/foo.wxml`：

```
<view class="foo">
  foo
</view>
```

编写 `components/foo.wxss`：

```
.foo {
  color: blue;
}
```

最后编写 `components/foo.js`（以 Chaining 形式为例）：

```
export default Component()
  .register()
```

这样，它就可以在其他自定义组件（包括页面）内使用。如果想在 `pages/index/index` 中引用它，那就在 `pages/index/index.json` 中这样写：

```
{
  "usingComponents": {
    "foo": "/components/foo"
  }
}
```

其中，`"foo"` 表示引用节点名，只能包含字母、数字、连字符和下划线，不能以数字开头。`"/components/foo"` 表示引用组件的路径，可以是相对路径或绝对路径。

然后，可以像使用内置组件那样，在 WXML 里面使用 `<foo>` 标签：

```
<foo />
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/ZO8fLvmD8TbE)

## slot 节点

在自定义组件中，可以使用一个特殊的标签 `<slot />`，用于接收外部传入的节点。

例如，在 `components/foo.wxml` 中：

```
<view class="foo">
  <slot />
</view>
```

在 `pages/index/index.wxml` 中，可以为 `<foo>` 添加子节点：

```
<foo>
  <view> bar </view>
</foo>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/m881NvmM8TbZ)

这样，在 `<view class="foo">` 中的 `<slot />` 就会被替换成 `<view> bar </view>`。通过这个特性就可以将两个组件 WXML 结构相互穿插到一起。

slot 节点还有几种不同的模式，请参考 [slot 模式](slot) 。

