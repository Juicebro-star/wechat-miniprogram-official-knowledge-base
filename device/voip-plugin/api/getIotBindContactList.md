---
title: "getIotBindContactList(Object req)"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/getIotBindContactList.html"
source_route: "/miniprogram/dev/framework/device/voip-plugin/api/getIotBindContactList.html"
raw_html: "raw-html/device/voip-plugin/api/getIotBindContactList.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/getIotBindContactList.html](https://developers.weixin.qq.com/miniprogram/dev/framework/device/voip-plugin/api/getIotBindContactList.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/device/voip-plugin/api/getIotBindContactList.html`

# getIotBindContactList(Object req)

> 本接口为异步接口，返回 `Promise` 对象。

根据 openId，查询指定用户是否授权某台设备。

## 参数

### Object req

| 属性 | 类型 | 默认值 | 必填 | 说明 |
| --- | --- | --- | --- | --- |
| sn | string |  | 是 | 设备 SN |
| model_id | string |  | 是 | 设备的 model_id |
| openid_list | string[] |  | 是 | 要查询的用户 openId 列表 |

## 返回值

### Object

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| errcode | number | 错误码 |
| errmsg | string | 错误信息 |
| contact_list | Info[] | openid 授权信息，status: 1 表示已授权，0 表示未授权 |

## 示例代码

```
const wmpfVoip = requirePlugin('wmpf-voip').default

wmpfVoip
  .getIotBindContactList({
    sn: '设备sn',
    model_id: '申请的modelid',
    openid_list: ['openid_1', 'openid_2'], // 传入需要验证的openid列表
  })
  .then(res => {
    console.log(`[getIotBindContactList]:`, res.contact_list)
    // [{sn: 'xxx', model_id: 'xxx', status: 0}]
    // status: 0/未授权；1/已授权
  })
```

