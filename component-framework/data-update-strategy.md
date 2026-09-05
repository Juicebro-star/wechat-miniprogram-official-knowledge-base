---
title: "数据更新策略"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/data-update-strategy.html"
source_route: "/miniprogram/dev/framework/component-framework/data-update-strategy.html"
raw_html: "raw-html/component-framework/data-update-strategy.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/data-update-strategy.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/data-update-strategy.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/data-update-strategy.html`

# 数据更新策略

在将组件数据应用到 WXML 模板上时，组件框架会用到一些更新算法。

## 数据更新算法

目前，组件框架可能使用两种更新算法：虚拟树更新、绑定映射表更新。

### 虚拟树更新

在执行虚拟树更新时，需要对整个 Shadow 树进行深度优先遍历，找到哪些数据绑定需要更新。

本质上来说，这是个深度优先遍历的过程，它的算法性能与 Shadow 树中的节点总量、节点复杂程度显著正相关。想要提升性能，建议合理划分组件、减少 Shadow 树中的节点数量。

### 绑定映射表更新

在 glass-easel 组件框架中，如果只是对个别数据字段进行小更新，组件框架会尝试使用绑定映射表更新。这种更新方式不需要遍历组件树，而是依靠一些编译期的额外信息来直接找到需要更新的数据绑定表达式。在更新的数据量比较小时，这样会显著提升性能。

然而，组件框架并不能高效判断哪种更新方式最合适。所以，目前 glass-easel 组件框架会基于如下策略来决定某次更新采用的算法。

1. 如果满足以下所有条件，那就采用绑定映射表更新：

- 单独更新一个数据字段；
- 这个数据字段没有被用于 `wx:if` `wx:for` `let:` 节点及它们的子孙节点。

2. 否则，采用虚拟树更新。

## 子组件更新的触发

无论采用哪种更新算法，它们的更新范围都在当前组件实例 Shadow 树范围内。

但在此过程中，如果某个自定义组件的属性被更新了，那会继而触发这个自定义组件实例的数据更新（递归地）。对于这个自定义组件实例 Shadow 树，组件框架会再次选用合适的数据更新算法来更新它。

对于组件 `Object` 和 `Array` 类型的属性，组件框架可能无法很好判断它们是不是被更新了。此时建议使用 [属性值比较器](properties#属性值比较器) 来辅助框架判断。

## WXS 的运行方式

对于 WXML 模板中包含的 WXS 代码，不同组件框架的运行方式完全不同。

在 glass-easel 组件框架中，WXS 代码运行在逻辑层中。在每次更新时，组件框架会判断有哪些数据绑定表达式需要更新，只有这些数据绑定表达式内的 WXS 函数会被调用。例如：

```
<wxs module="foo">
  exports.f1 = function (a) {
    return a
  }
  exports.f2 = function (a) {
    return a
  }
</wxs>

<view> {{ foo.f1(s) }} </view>
<view> {{ foo.f2(t) }} </view>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/LA75Nvm780bn)

对于上面这个 WXML 模板，如果 `setData` 更新字段中只有 `s` 而没有 `t`，那么只有 `f1` 会被调用，`f2` 不会被调用。

在 exparser 组件框架中，WXS 代码运行同时运行在逻辑层和视图层中。每次更新时，组件框架会将 Shadow 树中的 WXS 函数分别在逻辑层和视图层各执行一次。

此外，无论对于哪个组件框架，[WXS 事件响应函数](../view/interactive-animation) 都是仅运行在视图层中的，与其它 WXS 代码全局量并不互通。

