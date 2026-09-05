---
title: "在客服消息中使用临时素材"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/customer-message/temp-media.html"
source_route: "/miniprogram/dev/framework/open-ability/customer-message/temp-media.html"
raw_html: "raw-html/open-ability/customer-message/temp-media.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/customer-message/temp-media.html](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/customer-message/temp-media.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/open-ability/customer-message/temp-media.html`

# 在客服消息中使用临时素材

开发者可在接收和发送客服消息的过程中获取或上传临时素材。

## 获取客服消息内的临时素材

接收到用户消息之后，可通过 [获取临时素材](https://developers.weixin.qq.com/miniprogram/dev/server/API/kf-mgnt/kf-message/api_getmedia) 获取消息中的临时素材

## 新增图片素材

通过 [新增临时素材](https://developers.weixin.qq.com/miniprogram/dev/server/API/kf-mgnt/kf-message/api_uploadtempmedia) 可以上传临时素材，并在 [发送消息接口](https://developers.weixin.qq.com/miniprogram/dev/server/API/kf-mgnt/kf-message/api_sendcustommessage) 中使用。

