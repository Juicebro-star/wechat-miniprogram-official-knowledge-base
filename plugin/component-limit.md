---
title: "插件使用组件的限制"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/component-limit.html"
source_route: "/miniprogram/dev/framework/plugin/component-limit.html"
raw_html: "raw-html/plugin/component-limit.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/component-limit.html](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/component-limit.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/plugin/component-limit.html`

# 插件使用组件的限制

在插件开发中，以下组件不能在插件页面中使用：

- 开放能力（open-type）为以下之一的 [button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html)：
  - contact（打开客服会话）
  - getPhoneNumber（获取用户手机号）
  - getUserInfo（获取用户信息）
- [open-data](https://developers.weixin.qq.com/miniprogram/dev/component/open-data.html)
- [web-view](https://developers.weixin.qq.com/miniprogram/dev/component/web-view.html)

以下组件的使用对基础库版本有要求：

- [navigator](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) 需要基础库版本 [2.1.0](../compatibility)
- [live-player](https://developers.weixin.qq.com/miniprogram/dev/component/live-player.html) 和 [live-pusher](https://developers.weixin.qq.com/miniprogram/dev/component/live-pusher.html) 需要基础库版本 [2.3.0](../compatibility)

