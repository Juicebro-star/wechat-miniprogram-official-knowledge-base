---
title: "目录结构"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/structure.html"
source_route: "/miniprogram/dev/framework/structure.html"
raw_html: "raw-html/structure.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/structure.html](https://developers.weixin.qq.com/miniprogram/dev/framework/structure.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/structure.html`

# 目录结构

## 主要目录文件结构

小程序包含一个描述整体程序的 `app` 和多个描述各自页面的 `page`。

一个小程序主体部分由三个文件组成，必须放在项目的根目录，如下：

| 文件 | 必需 | 作用 |
| --- | --- | --- |
| [app.js](app-service/app) | 是 | 小程序逻辑 |
| [app.json](config) | 是 | 小程序公共配置 |
| [app.wxss](view/wxss) | 否 | 小程序公共样式表 |

一个小程序页面（和自定义组件）由四个文件组成，分别是：

| 文件类型 | 必需 | 作用 |
| --- | --- | --- |
| [js](app-service/page) | 是 | 页面逻辑 |
| [wxml](view/wxml/index) | 是 | 页面结构 |
| [json](config#页面配置) | 否 | 页面配置 |
| [wxss](view/wxss) | 否 | 页面样式表 |

**注意：描述页面（和自定义组件）的四个文件必须具有相同的路径与文件名。**

## 允许上传的文件

在项目目录中，以下文件会经过编译，因此上传之后无法直接访问到：*.js、app.json、*.wxml、*.wxss（其中 wxml 和 wxss 文件仅针对在 app.json 中配置了的页面）。除此之外，只有后缀名在白名单内的文件可以被上传，不在白名单列表内文件在开发者工具能被访问到，但无法被上传。具体白名单列表如下：

1. wxs
2. png
3. jpg
4. jpeg
5. gif
6. svg
7. json
8. cer
9. mp3
10. aac
11. m4a
12. mp4
13. wav
14. ogg
15. silk
16. wasm
17. br
18. cert

