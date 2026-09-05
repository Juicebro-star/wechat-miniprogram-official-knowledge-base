---
title: "Object getPluginOnloadOptions()"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/getPluginOnloadOptions.html"
source_route: "/miniprogram/dev/framework/device/voip-plugin/api/getPluginOnloadOptions.html"
raw_html: "raw-html/device/voip-plugin/api/getPluginOnloadOptions.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/getPluginOnloadOptions.html](https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/getPluginOnloadOptions.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/device/voip-plugin/api/getPluginOnloadOptions.html`

# Object getPluginOnloadOptions()

> 插件 2.2.3 版本开始支持

获取**插件通话页面打开(onLoad)时**页面路径中的参数。

注意：获取参数时，如果插件通话页面onLoad还没有触发，可能取到的不是最新的值。**建议在 `callPageOnShow` 事件后调用。**

## 参数

无

## 返回值

### Object

插件页面 [onLoad 生命周期的 query 参数](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page#onLoad-Object-query)

## 示例代码

```
const wmpfVoip = requirePlugin('wmpf-voip').default

let query = {}

wmpfVoip.onVoipEvent((event) => {
  if (event.eventName === 'callPageOnShow') {
    query = wmpfVoip.getPluginOnloadOptions()
  }
})
```

