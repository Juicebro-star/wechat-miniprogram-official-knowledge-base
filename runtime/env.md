---
title: "小程序的运行环境"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/env.html"
source_route: "/miniprogram/dev/framework/runtime/env.html"
raw_html: "raw-html/runtime/env.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/env.html](https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/env.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/runtime/env.html`

# 小程序的运行环境

## 小程序基础库

小程序开发框架包含一个庞大的运行时支持库，称为 **小程序基础库** 。

小程序基础库被内嵌在微信客户端中。每当微信客户端需要运行小程序的时候，就会激活小程序基础库，并在此基础上运行小程序。

## 小程序基础库版本

不同版本的微信客户端会运行着不同版本的小程序基础库，例如，Android 和 iOS 微信客户端版本 8.0.68 上会运行基础库 3.14.x。关于基础库与客户端之间的版本关系详情，可参考 [基础库](../client-lib/index) 文档。

同时，微信客户端版本对操作系统版本也有要求，如果用户的操作系统版本很老，就可能只能运行较老的微信客户端版本。另外，有些用户可能也会主动选择使用较老的微信客户端版本。因此，用户使用到的微信基础库版本就是多种多样的。每个基础库版本具体有多大比例的用户，可以在 [基础库版本分布](../client-lib/version) 中查看。

对于具体单个小程序而言，小程序不能指定用户必须使用哪个基础库版本、哪个微信客户端版本来运行小程序。用户设备上安装的微信版本决定了小程序会在哪个基础库版本上运行。

如果小程序只能在不低于某个特定版本的基础库上运行，可以在 [小程序管理页](https://mp.weixin.qq.com/) 「设置」-「基本设置」-「基础库最低版本设置」中设置小程序运行所要求的最低基础库版本。如果用户所使用的基础库版本不满足要求，微信就会阻止用户启动小程序并提示更新微信客户端。

## 运行环境种类

由于微信客户端会运行在多种平台上，小程序也会随之运行在多种平台上，包括：

- iOS/iPadOS 微信客户端；
- Android 微信客户端；
- 鸿蒙 OS 客户端；
- Windows PC 微信客户端；
- Mac 微信客户端；
- [小程序硬件框架](https://developers.weixin.qq.com/doc/oplatform/Miniprogram_Frame/index) ；
- 微信开发者工具（仅用于开发调试）。

不同运行环境下，脚本执行环境以及用于组件渲染的环境是不同的，性能表现也存在差异。下表列举了在不同平台上，小程序的逻辑层、视图层分别运行于哪个 JavaScript 引擎、浏览器引擎中。

| 运行环境 | 逻辑层 | 视图层（WebView 渲染引擎） |
| --- | --- | --- |
| iOS / iPadOS / MacOS | JavaScriptCore | WKWebView |
| Android | [V8](https://developers.google.com/v8/) | 微信 XWeb（基于 Mobile Chromium 内核） |
| Windows | Chromium 内核 | Chromium 内核 |
| 小程序开发者工具 | NWJS 或 Electron | Chrome WebView |

> JavaScriptCore 无法开启 JIT 编译 (Just-In-Time Compiler)，同等条件下的运行性能低于其他平台。

## 平台差异

尽管各运行环境是十分相似的，但是还是有些许区别。

- JavaScript 语法和 API 支持不一致：语法上可以通过开启 [`ES6` 转 `ES5` 功能](https://developers.weixin.qq.com/miniprogram/dev/devtools/codecompile#es6-转-es5) 来规避；此外，小程序基础库内置了必要的 Polyfill，来弥补 API 的差异。具体细节可参考 [JavaScript 支持情况](js-support) 文档。
- WXSS 样式表现在不同视图层 WebView 渲染引擎上会有所差异：尽管可以通过开启 [样式补全](https://developers.weixin.qq.com/miniprogram/dev/devtools/codecompile#样式补全) 来规避大部分的问题，还是建议在不同运行环境分别检查小程序的真实表现。

开发者工具仅供调试使用，最终的表现以微信客户端上的实际表现为准。

