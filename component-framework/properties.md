---
title: "组件属性"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/properties.html"
source_route: "/miniprogram/dev/framework/component-framework/properties.html"
raw_html: "raw-html/component-framework/properties.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/properties.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/properties.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/properties.html`

# 组件属性

组件可以定义一些属性，接收组件使用者传入的值。组件的使用者可以在 WXML 节点中指定属性对应的值。

## 定义属性

Definition 形式定义属性形如：

```
export default Component({
  properties: {
    fooProp: {
      type: String,
      value: '',
    },
  },
})
```

Chaining 形式定义属性形如：

```
export default Component()
  .property('fooProp', {
    type: String,
    value: '',
  })
  .register()
```

上例中定义了 `fooProp` 属性，定义时提供的 `type` 表示类型，`value` 表示初始值。

定义了属性之后，组件的使用者就可以在 WXML 节点中指定属性对应的值，例如在页面中：

```
<!-- page/index/index.wxml -->
<custom-component foo-prop="bar" />
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/XRtTKCm98Lby)

在 WXML 中，驼峰写法的属性名可以换成连字符写法（也可以仍然使用驼峰写法）。

对组件属性命名时，需要注意：

- 属性名不能与 data 数据字段重名；
- `id` `class` `style` `slot` 是 WXML 保留属性，虽然可以设为组件属性，但无法在 WXML 中对它们赋值；
- `dataXxx` 这样的属性名也无法在 WXML 中对它们赋值，因为 `data-xxx=""` 会被视为 [dataset](event-object#dataset) 。

## 属性类型与简化定义

属性支持下表中的类型。

| 类型 | 默认值 | 说明 |
| --- | --- | --- |
| `String` | `''` | 字符串 |
| `Number` | `0` | 数值 |
| `Boolean` | `false` | 布尔值 |
| `Object` | `null` | 对象 |
| `Array` | `[]` | 数组 |
| `Function` | `function () {}` | 函数 |
| `null` | `null` | 表示属性支持任意类型 |

> `Function` 类型仅 glass-easel 组件框架支持。在 exparser 组件框架中不能传递函数类型的属性，但可以将函数作为 `Object` 类型中的一个字段来传递。

如果一个属性的初始值 `value` 与它的类型默认值相同，就可以将属性定义简写为 `type` 本身，例如：

```
export default Component({
  properties: {
    fooProp: String,
  },
})
```

```
export default Component()
  .property('fooProp', String)
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/92tp0CmO8MbO)

## 初始值和默认值

**初始值** 指的是一个属性未被指定值时所具有的值。

**默认值** 指的是一个属性未被指定值时所具有的值，或被赋予非法值时用于代替非法值的属性值。仅 glass-easel 组件框架支持。默认值应使用一个函数来指定，形如：

```
export default Component({
  properties: {
    fooProp: {
      type: String,
      default: () => 'DEFAULT',
    },
  },
})
```

```
export default Component()
  .property('fooProp', {
    type: String,
    value: () => 'DEFAULT',
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/ubtiFCmC8Lbm)

如果指定了默认值 `default`，初始值 `value` 不能被指定。

## 属性值类型转换

在 WXML 中为一个属性指定值时，会自动进行类型转换；如果转换失败，则视为非法值（将使用默认值代替）。`type` 为 `null` 的属性不会进行类型转换。

例如，如果 `custom-component` 组件的 `num-prop` 属性是 `Number` 类型的，而在使用者的 WXML 中：

```
<custom-component num-prop="1" />
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/r1tYXCmC8Lbk)

这种情况下 `num` 属性会接收到数值 `1`。

### 布尔类型属性值转换

对于 `Boolean` 类型的属性，如果在 WXML 中未指定值，会自动将其值为 `true`。

```
<custom-component bool-prop />
<!-- 等价于 -->
<custom-component bool-prop="ANY" />
<!-- 等价于 -->
<custom-component bool-prop="{{true}}" />
```

想要把它指定为 `false`，必须使用数据绑定写法，例如：

```
<custom-component bool-prop="{{false}}" />
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/alzgjCmP83bw)

### 对象、数组类型属性值转换

对象和数组只能使用数据绑定写法，例如：

```
<custom-component array-prop="{{ [1, 2] }}" object-prop="{{ { a: 1, b: 2 } }}" />
```

在 glass-easel 组件框架中，绑定对象外层的花括号可以省略：

```
<custom-component object-prop="{{ a: 1, b: 2 }}" />
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/CKthNCma8Lbc)

## 可选类型

如果一个属性可能接受多种类型的值，可以用 `optionalTypes` 为它附加多个类型。例如：

```
export default Component({
  properties: {
    fooProp: {
      type: String,
      optionalTypes: [String, Boolean],
    },
  },
})
```

```
export default Component()
  .property('fooProp', {
    type: Number,
    optionalTypes: [String, Boolean],
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/uWtjHCmI8Lb5)

## 使用 TypeScript 指定对象类型

使用 TypeScript 时，对于 `type` 为 `Array` 或 `Object` 的属性，会根据它初始值、默认值的类型作为这个属性字段的具体类型，来进行类型推断。

此时，可以通过 `as` 来指定具体类型：

```
export default Component()
  .property('objectProp', {
    type: Object,
    value: {} as {
      foo?: number
      bar?: string
    },
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Rxt3BCmv8Lbl)

## 属性值变化监听器

属性值可以指定一个 `observer`，当属性值发生变化时，会调用这个函数。

```
export default Component()
  .property('foo', {
    type: String,
    observer(newVal, oldVal) {
      // 属性值变化后触发
    },
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/13thRCme8LbR)

如果需要在 observer 中同步执行 `setData` 等更新操作，更推荐使用 [数据监听器](../observer) ，因为通常它有更好的性能。

对于 type 为 `Object` 或 `Array` 的属性，如果通过该组件自身的 `this.setData` 来改变属性值的一个子字段，则依旧会触发属性 observer，且 observer 接收到的 `newVal` 是变化的那个子字段的值。

## 属性值比较器

> 仅 glass-easel 组件框架支持。

每当组件的使用者进行更新时，组件框架需要判断它使用的每个子组件是不是被更新了。框架的判断方法是，对比每个属性值，如果属性值不一样，就认为子组件被更新了。如果子组件被更新了，就会产生一定的更新开销。

对于子组件属性是数值、字符串、布尔值等基本类型时，框架可以直接对比它们的值。但对于对象、数组这样的值，框架难以比较，常常将它们直接视为被更新了。

此时可以通过 `comparer` 来指定一个属性值比较函数，避免不必要的更新、提升整体性能。如果 `comparer` 返回真，表示属性值变化了、组件需要更新。例如：

```
export default Component()
  .property('objectProp', {
    type: Object,
    comparer(newValue, oldValue) {
      return newValue.foo !== oldValue.foo || newValue.bar !== oldValue.bar
    },
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/h3tiUCmo8LbD)

## 页面根组件的属性值

对于页面根组件，属性值会从页面的 query 参数中获取（参数值会被自动 URL decode）。例如：

```
// pages/index/index.js
export default Component()
  .property('strProp', String)
  .property('numProp', Number)
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/9DtA5Cmx8MbT)

如果页面以 `pages/index/index?strProp=foo&numProp=1` 打开，那么 `strProp` 会被赋值 `'foo'`，`numProp` 会被赋值 `1`。

由于 query 参数是字符串，页面根组件的属性值应当是 `String` 或 `Number` 类型的。

