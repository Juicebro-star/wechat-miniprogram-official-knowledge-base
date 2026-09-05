---
title: "界面渲染架构"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/view/rendering.html"
source_route: "/miniprogram/dev/framework/view/rendering.html"
raw_html: "raw-html/view/rendering.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/view/rendering.html](https://developers.weixin.qq.com/miniprogram/dev/framework/view/rendering.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/view/rendering.html`

# 界面渲染架构

小程序的界面以若干页面组成。在每个页面渲染的时候，小程序基础库内部的两个模块会发挥重要的作用。

- **组件框架** 提供了 `Page` `setData` 等重要的 JavaScript 接口，并结合 WXML 来构建界面结构。
- **渲染引擎** 将界面结构最终绘制成用户界面。

## 组件框架

组件框架为小程序提供了页面相关的 JavaScript 接口。与它相关的主要特性包括（但不限于）：

- [页面注册](../app-service/page)
- [自定义组件](../component-framework/index)
- [事件系统](../component-framework/event-system)
- [WXML](wxml/index)

小程序基础库提供了两个可选的组件框架。选择不同的组件框架时，`Page` 等接口的具体实现、WXML 的运行方式都会不同。（每个页面可以独立选择使用哪个组件框架。）

- **exparser** 是传统的组件框架（默认启用）。
- **glass-easel** 是新一代组件框架，是一个 [开源项目](https://github.com/wechat-miniprogram/glass-easel/) ，提供了更多特性、具有更好的性能表现。

详情请参考 [组件框架](../component-framework/index) 章节。

## 渲染引擎

渲染引擎为小程序提供了最终的界面绘制支持。与它相关的主要特性包括（但不限于）：

- [基础组件](component)
- [WXSS](wxss) 中的具体样式规则

小程序基础库提供了两个可选的渲染引擎。选择不同的渲染引擎时，可用的基础组件和 WXSS 样式规则会有一定区别。（每个页面可以独立选择使用哪个渲染引擎。）

- **Webview** 是传统的渲染引擎（默认启用）。
- **Skyline** 是更高效的渲染引擎，提供了很多增强特性、具有更好的性能表现。

渲染引擎对组件框架有一定的依赖。目前，如果选用了 Skyline 渲染引擎，就必须选用 glass-easel 组件框架。详情请参考 [Skyline 渲染引擎](../runtime/skyline/introduction) 章节。

