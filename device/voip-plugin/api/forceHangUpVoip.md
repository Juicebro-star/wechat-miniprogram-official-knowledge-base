---
title: "void forceHangUpVoip([string roomId])"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/forceHangUpVoip.html"
source_route: "/miniprogram/dev/framework/device/voip-plugin/api/forceHangUpVoip.html"
raw_html: "raw-html/device/voip-plugin/api/forceHangUpVoip.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/forceHangUpVoip.html](https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/forceHangUpVoip.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/device/voip-plugin/api/forceHangUpVoip.html`

# void forceHangUpVoip([string roomId])

强制结束通话

## 参数

### String roomId

可选。2.3.2 开始支持。

- 不传入时，挂断当前正在进行的通话；
- 传入时，仅在当前通话 roomId 与传入相同时，挂断当前正在进行的通话。（建议）

## 返回值

无

## 示例代码

```
const wmpfVoip = requirePlugin('wmpf-voip').default

wmpfVoip.forceHangUpVoip('some group id')
```

