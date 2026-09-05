---
title: "组件样式"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/style.html"
source_route: "/miniprogram/dev/framework/component-framework/style.html"
raw_html: "raw-html/component-framework/style.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/style.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/style.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/style.html`

# 组件样式

考虑到组件间相互影响的问题，组件的样式表写法受到一定限制。

## 组件选择器限制

在编写组件样式表时，只推荐使用 class 选择器、伪类和伪元素选择器。这样比较利于代码维护，且它们具有更好的性能。

使用其他选择器需要注意：

- 属性选择器（`[foo]`）不受支持、不会生效；
- ID 选择器（`#foo`）不推荐使用，仅在使用 `Page` 构造器构造的页面根组件中生效；
- 子元素选择器（`.foo > .bar`）只能用于 `view` 组件与其子节点之间，用于其他组件可能导致非预期的情况；
- 标签名选择器（`button`）应尽量避免使用，它不受样式隔离限制，对所有组件都会生效；如需使用，应写在 `app.wxss` 中。

```
[foo] { } /* 不受支持 */
#foo { } /* 不推荐  */
.foo > .bar { } /* 除非 .foo 是 view 组件节点，否则不一定会生效 */
button { } /* 不推荐 */
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/N08wuvm28TbU)

## 组件样式隔离

默认情况下，自定义组件的样式只受到自定义组件 WXSS 样式的影响，即 **样式隔离** 。这样，可以在自定义组件中使用比较短的 class 名字，无需担心与其他组件的 class 名冲突。

```
<view class="title">标题</view>
```

```
.title {
  font-size: 2em;
}
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/zk8Rvvmv8TbY)

从基础库版本 [2.10.1](../compatibility) 开始，可以通过设置 `styleIsolation` 选项来修改自定义组件样式隔离行为。它支持以下取值：

- `isolated` 表示启用样式隔离，在自定义组件内外，使用 class 指定的样式将不会相互影响；
- `apply-shared` 表示页面的样式将影响到自定义组件，但自定义组件的样式不会影响页面；
- `shared` 表示页面的样式将影响到自定义组件，自定义组件的样式也会影响页面和其他设置了 `apply-shared` 或 `shared` 的自定义组件（这个选项在插件的自定义组件中不可用）。

`styleIsolation` 选项需要在自定义组件 JSON 中设置：

```
{
  "usingComponents": {},
  "styleIsolation": "isolated"
}
```

对于 `styleIsolation` 的默认值：

- 如果组件是 `Page` 构造器构造的页面根组件，`styleIsolation` 的默认值为 `shared`；
- 如果组件路径在 `app.json` 的 `pages` 列表中，`styleIsolation` 的默认值为 `shared`；
- 在其他组件中，`styleIsolation` 的默认值为 `isolated`。

> `styleIsolation` 也可以在 JS 脚本的 `options` 中配置，但目前已不推荐。

> JS 脚本的 `options` 中也支持 `addGlobalClass: true` 配置，等价于设置 `styleIsolation: apply-shared`。目前已不推荐使用这个配置。

## host 节点选择器

自基础库 [1.7.2](../compatibility) 起，通过 `:host` 选择器，组件可以指定它自身对应节点的默认样式。例如：

```
/* custom-component.wxss */
:host {
  color: yellow;
}
```

无论引用者为组件赋予的节点名是什么，`:host` 选择器都会对这个节点生效。例如：

```
{
  "usingComponents": {
    "custom": "/path/to/custom-component"
  }
}
```

```
<!-- 引用组件的 WXML -->
<custom>这段文本是黄色的</custom>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/yR85wvmg8Tbx)

