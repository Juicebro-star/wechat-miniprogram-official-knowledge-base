---
title: "behaviors"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/behaviors.html"
source_route: "/miniprogram/dev/framework/component-framework/behaviors.html"
raw_html: "raw-html/component-framework/behaviors.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/behaviors.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/behaviors.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/behaviors.html`

# behaviors

behaviors 是用于组件间脚本代码共享的特性，类似于一些编程语言中的 `mixins`。

## Behavior 构造器

behaviors 通过 `Behavior` 构造器构造。

类似于 `Component` 构造器，`Behavior` 构造器也有 Definition 和 Chaining 两种形式，也可以混用。

Behavior 构造器的 Definition 形式例如：

```
const fooBehavior = Behavior({
  data: {
    foo: 1,
  },
  lifetimes: {
    attached() {
      // 页面创建时执行
    },
  },
  methods: {
    onButtonTap() {
      // 用户事件触发时执行
    },
  },
})
```

Chaining 形式例如：

```
const fooBehavior = Behavior()
  .data(() => ({
    foo: 1,
  }))
  .init(function ({ lifetime, method }) {
    lifetime('attached', () => {
      // 页面创建时执行
    })
    const buttonTap = method(() => {
      // 用户事件触发时执行
    })
    return {
      buttonTap,
    }
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/5bc18BmF8obW)

`Behavior` 构造器的用法和 `Component` 构造器非常相似，只是 `options` / `.options()` 对于 `Behavior` 构造器无效。另外，`Behavior` 构造器的返回值需要用到，需要把返回值存储到变量中或 export 给别的文件使用。

## 使用 behaviors

在 `Component` 构造器中，可以通过 `behaviors` / `.behavior()` 来引用 behaviors。

Definition 形式引用 behaviors 例如：

```
export default Component({
  behaviors: [fooBehavior],
  data: {
    bar: 2,
  },
})
```

Chaining 形式引用 behaviors 例如：

```
export default Component()
  .behavior(fooBehavior)
  .data(() => ({
    bar: 2,
  }))
  .register()
```

引用 behaviors 后，相当于将 behaviors 中的属性、数据字段、方法、生命周期函数等等都合并到组件中。在上例中，`fooBehavior` 提供了 `foo` 数据字段，而组件自身有 `bar` 数据字段，所以整个组件就有 `foo` 和 `bar` 两个数据字段。属性、方法、生命周期函数等等也会产生类似的效果。

一个组件可以引用多个 behaviors，而且对它们的定义形式没有要求（可以一些是 Definition 形式定义的、另一些 Chaining 形式定义的）。

`Behavior` 构造器中也可以嵌套引用其他 behaviors。

```
const barBehavior = Behavior()
  .behavior(fooBehavior)
  .data(() => ({
    bar: 2,
  }))
  .register()
```

自小程序基础库版本 [2.9.2](../compatibility) 起，在 `Page` 构造器中，也可以类似地使用 `behaviors`。

```
export default Page({
  behaviors: [myBehavior],
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/58cDbBme8obN)

## 同名字段的覆盖和组合规则

组件和它引用的 behaviors 中可以包含同名的字段。

如果有同名的属性 properties 或方法 methods 是覆盖式的：

- 若组件本身有这个属性或方法，则组件的属性或方法会覆盖 behaviors 中的同名属性或方法；
- 若组件本身无这个属性或方法，则在组件 behaviors 中靠后的会覆盖靠前的；
- 若存在嵌套引用 behaviors 的情况，则引用者的属性或方法覆盖被引用的。

如果有同名的数据字段：

- 若同名的数据字段都是对象类型，会进行对象合并；
- 其余情况处理方式是覆盖式的（与上述属性和方法的方式一致）。

数据监听器 observers、生命周期函数 lifetimes、页面生命周期函数 pageLifetimes、外部样式类 externalClasses 等等其他情况，都是组合式的。例如，在 behaviors 和组件自身都定义了同一个生命周期函数时，它们都会被执行，执行顺序是：

- 先按 behaviors 列表中的顺序执行（如果定义了）；
- 最后执行组件自身的生命周期函数（如果定义了）；
- 若存在嵌套引用 behaviors 的情况，则被引用的优先与引用者的执行；

如果同一个 `Behavior` 被一个组件多次引用，它的数据监听器、生命周期函数、页面生命周期函数可能会被重复执行（也可能不会），基于以下规则：

- 如果是 `Behavior` 是 Definition 形式的，那它的数据监听器、生命周期函数、页面生命周期函数 **不会** 被重复执行；
- 如果是 `Behavior` 是 Chaining 形式的，那它的数据监听器、生命周期函数、页面生命周期函数 **会** 被重复执行。

## 内置 behaviors

小程序基础库也定义了一些内置的 behaviors。自定义组件可以通过引用它们来获得内置组件的一些行为。

```
export default Component()
  .behavior('wx://form-field')
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/h5cDeBmE8ob1)

在上例中， `wx://form-field` 是内置的，它使得这个自定义组件有类似于表单控件的行为。

内置 behaviors 往往会为组件添加一些属性。在没有特殊说明时，组件可以覆盖这些属性来改变它的 `type` 或添加 `observer` 。

### wx://form-field

使自定义组件有类似于表单控件的行为。 form 组件可以识别这些自定义组件，并在 submit 事件中返回组件的字段名及其对应字段值。

详细用法以及代码示例请参考 [form 组件参考文档](https://developers.weixin.qq.com/miniprogram/dev/component/form.html) 。

### wx://form-field-group

从基础库版本 [2.10.2](../compatibility) 开始提供支持。

使 form 组件可以识别到这个自定义组件内部的所有表单控件。

详细用法以及代码示例请参考 [form 组件参考文档](https://developers.weixin.qq.com/miniprogram/dev/component/form.html) 。

### wx://form-field-button

从基础库版本 [2.10.3](../compatibility) 开始提供支持。

使 form 组件可以识别到这个自定义组件内部的 button。如果自定义组件内部有设置了 form-type 的 button，它将被组件外的 form 接受。

详细用法以及代码示例请参考 [form 组件参考文档](https://developers.weixin.qq.com/miniprogram/dev/component/form.html) 。

### wx://component-export

> 仅对 exparser 有效，glass-easel 组件框架自带这个特性，不需要额外引入。

从基础库版本 [2.2.3](../compatibility) 开始提供支持。

使自定义组件支持 `export` 定义段。这个定义段可以用于指定组件被 `selectComponent` 调用时的返回值。

