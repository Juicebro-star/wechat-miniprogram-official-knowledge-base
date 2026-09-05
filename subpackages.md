---
title: "分包加载"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages.html"
source_route: "/miniprogram/dev/framework/subpackages.html"
raw_html: "raw-html/subpackages.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages.html](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/subpackages.html`

# 分包加载

> 微信客户端 6.6.0，基础库 [1.7.3](compatibility) 及以上版本开始支持。开发者工具请使用 1.01.1712150 及以上版本，可[点此下载](https://developers.weixin.qq.com/miniprogram/dev/devtools/download)。

某些情况下，开发者需要将小程序划分成不同的子包，在构建时打包成不同的分包，用户在使用时按需进行加载。

在构建小程序分包项目时，构建会输出一个或多个分包。每个使用分包小程序必定含有一个**主包**。所谓的主包，即放置默认启动页面/TabBar 页面，以及一些所有分包都需用到公共资源/JS 脚本；而**分包**则是根据开发者的配置进行划分。

在小程序启动时，默认会下载主包并启动主包内页面，当用户进入分包内某个页面时，客户端会把对应分包下载下来，下载完成后再进行展示。

目前小程序分包大小有以下限制：

- 整个小程序所有分包大小不超过 30M（服务商代开发的小程序不超过 20M）
- 单个分包/主包大小不能超过 2M

对小程序进行分包，可以优化小程序首次启动的下载时间，以及在多团队共同开发时可以更好的解耦协作。

具体使用方法请参考：

- [使用分包](subpackages/basic)
- [独立分包](subpackages/independent)
- [分包预下载](subpackages/preload)
- [分包异步化](subpackages/async)

