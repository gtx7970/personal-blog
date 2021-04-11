---
title: "vue component"
metaTitle: "vue component"
metaDescription: "This is the meta description for this page"
---

vue 2.x

## Vue component

### props $emit


### lifeCycle
挂载
更新
销毁

beforeDestroy  解绑事件, 清除计数器

生命周期父子组件
父组件
子组件

父 created -> 子 created -> 子 mounted -> 父mounted

父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated

父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed

### 自定义 v-model

### $nextTick
异步渲染
data改变后，DOM不会立刻渲染
$nextTick会在DOM渲染之后被触发，以获取最新DOM

### slot

### 动态、异步组件

:is="component-name"

import()
异步加载

```js
export default {
  components: () => import('./BaseDemo')
}
```

### keep-alive
缓存组件, 频繁切换
### mixin


### vue-router
hash history

动态路由

