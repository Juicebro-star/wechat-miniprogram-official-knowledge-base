---
title: "组件间交互概述"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/interaction.html"
source_route: "/miniprogram/dev/framework/component-framework/interaction.html"
raw_html: "raw-html/component-framework/interaction.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/interaction.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/interaction.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/interaction.html`

# 组件间交互概述

不同的组件之间常常需要有逻辑上的交互。组件框架提供了多种组件间交互的方式。

## 父组件与子组件间的通信

两个有引用与被引用关系的组件，通常可以称为父组件和子组件。父组件通过 `usingComponents` 引用子组件，并在 WXML 中使用子组件的节点。

- 如果父组件想要传递数据给子组件、触发子组件的特定逻辑，最佳的方式是通过 [组件属性](properties) 来传递。
- 如果子组件想要传递数据给父组件、触发父组件的特定逻辑，最佳的方式是通过 [事件系统](event-system) 来传递。
- 如果想要在父组件和子组件之间同步数据，可以通过 [双向绑定](../view/two-way-bindings) 来同步。

slot 也可以用于向 slot 内容节点树中传递数据，请参考 [动态 slot](slot#动态-slot) 。

## 同一个 Shadow 树内节点之间的通信

在同一个 Shadow 树内，节点之间有时也需要有逻辑交互。

例如，想要自行实现 `<my-form>` 和 `<my-button>` 组件，它们是被页面这样使用的：

```
<!-- 页面的 WXML -->
<my-form>
  <my-button> Submit </my-button>
</my-form>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/ku7opvmQ81bl)

在 `<my-button>` 被点击时，需要触发在 Shadow 树上的 `<my-form>` 组件的提交行为。

这种情况下就需要使用 [组件间关系](../custom-component/relations) 来实现。

## 直接组件实例访问

有时，上述方式都不够灵活、难以满足复杂的组件交互要求。此时可以使用 [节点树访问](component-tree-visit) 的方式，直接获得想要的组件实例 `this`。

不过，这种方式如果不当使用，容易导致组件间过度耦合，因此不优先推荐。如果需要使用，建议配合 [trait behaviors](trait-behaviors) 来进行组件间逻辑解耦。

