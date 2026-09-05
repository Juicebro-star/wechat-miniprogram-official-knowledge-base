---
title: "逻辑层 App Service"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/"
source_route: "/miniprogram/dev/framework/app-service/"
raw_html: "raw-html/app-service/index.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/app-service/index.html`

# 逻辑层 App Service

小程序开发框架的逻辑层使用 `JavaScript` 引擎为小程序提供开发 `JavaScript` 代码的运行环境以及微信小程序的特有功能。它也被称为 AppService。

逻辑层将数据进行处理后发送给视图层，同时接受视图层的事件反馈。

在 `JavaScript` 的基础上，逻辑层还提供了一些便于小程序开发的功能：

- 增加 `App` 和 `Page` 方法，进行 [程序注册](app) 和 [页面注册](page) 。
- 提供丰富的 [API](api)，如微信用户数据，扫一扫，支付等微信特有能力。
- 提供 [模块化](module#模块化) 能力，每个页面有独立的 [作用域](module#文件作用域) 。

**注意：小程序框架的逻辑层并非运行在浏览器中，因此 `JavaScript` 在 web 中一些能力无法使用，如 `window`、`document` 等。**

