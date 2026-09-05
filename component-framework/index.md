---
title: "组件框架"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/"
source_route: "/miniprogram/dev/framework/component-framework/"
raw_html: "raw-html/component-framework/index.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/index.html`

# 组件框架

## 组件化

小程序的界面代码组织以 **组件** 作为基本单位。

一个页面可以由很多组件组合而成。这些组件分为两类：

- [基础组件](../view/component) 是不可再分解的基础功能单元，如 `<view>` `<image>`；
- 可以用若干组件拼成一个大的组件，这样的组件称为 [自定义组件](index) 。

换句话说，页面由基础组件和自定义组件组合而成，而其中的自定义组件又由基础组件和其他自定义组件组合而成。特别地：

- 页面实际上就是一个很大的自定义组件，也称为 **页面根组件**，所以页面具备所有自定义组件具有的特性；
- 自定义组件可以递归地引用自身（但无限递归会导致栈溢出，请控制好递归条件）。

自定义组件相关特性自小程序基础库版本 [1.6.3](../compatibility) 开始支持。

## 组件代码构成

每个自定义组件的代码由四部分构成：

- JSON 配置；
- [WXML 模板](../view/wxml/index)
- [WXSS 样式](../view/wxss)
- [JavaScript（或 TypeScript）脚本](scripting)

这四个文件必须放在同一文件路径下、仅有文件扩展名不同。

其中，JSON 配置中需要至少包含 `usingComponents` 或 `component` 其中一项。

对于页面根组件，通常可以这样写：

```
{
  "usingComponents": {}
}
```

对于非页面根组件的自定义组件，通常可以这样写：

```
{
  "component": true,
  "usingComponents": {}
}
```

`usingComponents` 表示这个组件依赖的其他组件，可参考 [组件引用](using-components) 文档。从另一个角度说，一个组件的 JSON 配置、WXML 模板、WXSS 样式、JavaScript 脚本写完后，它就可以被其他组件引用了。

`component` 字段并不是必需的。它只会影响一些组件框架细节表现；即使未写明，只要有 `usingComponents` 字段，它依然可以作为组件使用。不过，仍然建议为所有非页面根的组件添加 `component` 字段，作为标识。

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/RNhMdZmt8VaG "在开发者工具中预览效果")

## 组件框架选择

目前，有两个组件框架可选。

**exparser** 是传统的组件框架，对旧代码具有最佳的兼容性。但由于历史原因，它缺少部分新特性、性能也不是最优。

**glass-easel** 是新一代的组件框架，由 **更多特性** 和 **更优性能** 。如果想要深入了解它本身，可以参考 [glass-easel 开源项目](https://github.com/wechat-miniprogram/glass-easel/) 。

在使用传统的 WebView 渲染引擎时，每个页面都可以选择使用其中一个组件框架来渲染。默认情况下，使用 exparser 作为组件框架，如果想改用 glass-easel，需要在页面 JSON 配置中声明：

```
{
  "usingComponents": {},
  "componentFramework": "glass-easel",
  "glassEaselWebview": true
}
```

关于上述配置的详细说明，请参考 [迁移到 glass-easel](../custom-component/glass-easel/migration) 文档。

如果使用 [Skyline 渲染引擎](../runtime/skyline/introduction) ，就只能选用 glass-easel 作为组件框架。

其中的 `componentFramework` 字段可以放在 `app.json` 中使它全局生效。

页面中的自定义组件跟随其所在页面选用的组件框架。

尽管这两个组件框架有极高的相似性，它们之间仍有一些细微差异。可参考 [迁移到 glass-easel](../custom-component/glass-easel/migration) 来进行组件框架升级。

