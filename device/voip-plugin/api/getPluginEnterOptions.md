---
title: "Object getPluginEnterOptions()"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/getPluginEnterOptions.html"
source_route: "/miniprogram/dev/framework/device/voip-plugin/api/getPluginEnterOptions.html"
raw_html: "raw-html/device/voip-plugin/api/getPluginEnterOptions.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/getPluginEnterOptions.html](https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/getPluginEnterOptions.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/device/voip-plugin/api/getPluginEnterOptions.html`

# Object getPluginEnterOptions()

获取**从插件页面进入小程序时**的启动参数。

从插件页面进入小程序时，小程序本身无法直接通过[`wx.getEnterOptionsSync`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/life-cycle/wx.getEnterOptionsSync.html) 获取启动参数，故提供本接口。

注意：**当小程序在前台时被客户端 reLaunch（例如手机微信来电提醒打开小程序，或 WMPF launchMiniProgram 接口打开小程序）**，这种情况下并不算「进入小程序」，`getPluginEnterOptions` 无法获取最新的 query，需要通过 [`getPluginOnloadOptions`](getPluginOnloadOptions) 获取。

## 参数

无

## 返回值

### Object

启动参数。与插件调用[`wx.getEnterOptionsSync`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/life-cycle/wx.getEnterOptionsSync.html)返回值一致。

## 示例代码

```
const wmpfVoip = requirePlugin('wmpf-voip').default

const query = wmpfVoip.getPluginEnterOptions()
```

