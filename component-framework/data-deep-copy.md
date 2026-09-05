---
title: "数据字段拷贝控制"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/data-deep-copy.html"
source_route: "/miniprogram/dev/framework/component-framework/data-deep-copy.html"
raw_html: "raw-html/component-framework/data-deep-copy.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/data-deep-copy.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/data-deep-copy.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/data-deep-copy.html`

# 数据字段拷贝控制

> 仅 glass-easel 组件框架支持。

组件框架在部分时候会对数据进行 **深拷贝**。会执行深拷贝的时刻包括：

- 使用 `setData` 等数据更新方法的时候；
- 在组件间传递属性值的时候；
- 向 WXS 传递参数、WXS 返回值的时候。

其中前两者的深拷贝行为可以通过组件选项来调整。（目前 WXS 相关的深拷贝行为不能调整。）

## 数据深拷贝

使用 `setData` 等数据更新方法的时候，默认情况下，数据字段应用到 WXML 模板前会经过一次深拷贝。这样，对 `this.data` 的直接修改就不会应用到 WXML 模板上。例如：

```
// 这样设置的 foo 不会应用到 WXML 模板上
this.data.foo = 'bar'
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/AA7FKvmP80bL)

因为实际应用在 WXML 模版上的数据是 `this.data` 的一份深拷贝副本。如果直接修改 `this.data`，它的深拷贝副本不会更新。

### 简单深拷贝

默认情况下，这是一种 **简单深拷贝** 策略：递归复制所有基本 JS 类型、数组、简单对象、函数和 Symbol，即，所有 JSON 可表达的数据类型外加上函数、Symbol。它不会递归复制：

- 数组中使用非 number 索引的字段（会丢失）；
- 函数中的子字段（不会复制、会保留同一引用）；
- 对象的原型（会丢失）；
- `Date` `RegExp` 等 JS 内置对象（会视为普通对象处理，丢失原型）。

### 支持子字段递归的深拷贝

如果对象内的子字段存在无限递归的情况，例如：

```
var rec = { b: null }
rec.b = { c: rec }

// 此时
rec.b.c === rec // true
rec.b.c.b.c === rec // true
rec.b.c.b.c.b.c === rec // true
// ...
// rec 对象的子字段会出现无限递归

// setData 时，会触发栈溢出！
this.setData({ foo: rec }) // 不要这么做！
```

这会导致深拷贝时出现无限递归导致的栈溢出。出现这种情况时，需要将深拷贝策略改为 **支持子字段递归的深拷贝** `simple-recursion`。

```
export default Component()
  .options({
    // 将深拷贝策略改为支持子字段递归的深拷贝
    dataDeepCopy: 'simple-recursion',
  })
  .init(function ({ setData, lifetime }) {
    lifetime('attached', () => {
      setData({ foo: rec }) // OK!
    })
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/A97hMvmQ80b7)

### 禁用深拷贝

也可以选择完全禁用深拷贝，前提是你确定不会直接修改 `this.data`。这样可以完全避免深拷贝带来的开销、对象的原型链能够保留、性能最优：

```
export default Component()
  .options({
    // 禁用深拷贝
    dataDeepCopy: 'none',
  })
  .init(function ({ setData, lifetime }) {
    lifetime('attached', () => {
      setData({
        foo: rec,
        date: new Date(),
      })
    })
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/7j7MGvma80b2)

### 所有可用的深拷贝策略

下表包含了目前所有可用的深拷贝策略。

| 深拷贝策略 | 说明 |
| --- | --- |
| `simple` | 启用简单深拷贝策略（默认值） |
| `simple-recursion` | 启用支持子字段递归的深拷贝 |
| `none` | 禁用深拷贝 |

## 属性传递深拷贝

默认情况下，通过 WXML 传递组件属性值时，也会经过一次深拷贝。这样可以使得两个组件之间不共享对象，对一个组件的数据变更不会影响另一个组件。

但这也会让属性传递时不能传递存在无限递归的对象，也会有一定的深拷贝开销。可以通过更改被引用组件的 `propertyPassingDeepCopy` 选项来选用不同的深拷贝策略。

例如，在被引用组件（子组件）上：

```
// custom-component.js
export default Component()
  .options({
    propertyPassingDeepCopy: 'simple-recursion',
  })
  .property('fooProp', Object)
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/iH76Hvm480bw)

这样，子组件的所有属性就都可以接受无限递归的对象了。

`propertyPassingDeepCopy` 也可以指定为 `none` 来禁用深拷贝。禁用之后，多个组件的数据中将共享同一对象的引用，所以在编程时请格外留意：避免修改数组、对象属性的子字段。

```
// custom-component.js
export default Component()
  .options({
    propertyPassingDeepCopy: 'none',
  })
  .property('fooProp', {
    type: Object,
    value: {
      bar: 1,
    },
  })
  .init(function ({ lifetime }) {
    lifetime('attached', () => {
      const foo = this.data.fooProp
      // 请留意不要修改属性的子字段
      foo.bar = 2 // 不要这样做！
    })
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Di7EJvm480br)

## 在组件间无拷贝地传递复杂对象

有时，需要在组件间共享带有原型链的复杂对象，或完全避免巨大数组、巨大对象的拷贝开销。这时需要进行两项设置：

- 在被引用组件（子组件）上，设置 `propertyPassingDeepCopy` 为 `none`；
- 在引用组件（父组件）上，设置 `dataDeepCopy` 为 `none`。

这样，父组件就可以通过 setData 设置对象、数组，通过 WXML 模板传递后，子组件会无拷贝原样收到。

