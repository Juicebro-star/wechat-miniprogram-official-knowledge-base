---
title: "真机调试 2.0"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/performance/remote_debug_2.html"
source_route: "/miniprogram/dev/framework/performance/remote_debug_2.html"
raw_html: "raw-html/performance/remote_debug_2.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/performance/remote_debug_2.html](https://developers.weixin.qq.com/miniprogram/dev/framework/performance/remote_debug_2.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/performance/remote_debug_2.html`

# 真机调试 2.0

微信开发者工具的「真机调试 2.0」功能，可以帮助开发者利用工具调试真机上的小程序表现，也包括了性能分析的能力。开启真机调试 2.0 的步骤请参考[《真机调试 2.0 文档》](https://developers.weixin.qq.com/miniprogram/dev/devtools/remote-debug-2)。

## 内存调试

> 仅支持安卓设备

开发者可以使用「memory」面板，获取小程序逻辑层的 JS 堆内存快照，分析内存分布情况，排查内存泄漏问题。

![](https://res8.wxqcloud.qq.com.cn/wxdoc/e6b4ba46-62ae-41c6-8281-c2407e6c9eea.jpg)

![](https://res8.wxqcloud.qq.com.cn/wxdoc/784e8de6-72d7-401d-88a3-f42420b05381.jpg)

> 详细的使用说明可参考 Chrome 的「Memory」面板

## JavaScript Profiler

> 仅支持安卓设备

开发者可以使用「JavaScript Profiler」面板，分析小程序逻辑层的 JS 执行情况。如果要分析启动过程中小程序代码注入的情况，可以在代码中使用 `debugger` 来断点。

```
// app.js
debugger

App({
  onLaunch() {}
})
```

![](https://res8.wxqcloud.qq.com.cn/wxdoc/181cb8b7-2908-481c-95ed-1611cdc4da48.jpg)

![](https://res8.wxqcloud.qq.com.cn/wxdoc/cff88795-4216-4a5a-9f40-ff5f43a345e9.jpg)

> 详细的使用说明可参考 Chrome 的「JavaScript Profiler」面板

