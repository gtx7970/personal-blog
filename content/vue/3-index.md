---
title: "vue deep"
metaTitle: "vue deep"
metaDescription: "This is the meta description for this page"
---

vue 2.x

### Vue响应式

```js
Object.defineProperty()
```

```js
const data = {name: 'abc'}

// array
const oldArrayProto = Array.prototype
const arrProto = Object.create(oldArrayProto)
['push', 'pop', 'shift', 'unshift', 'splice'].forEach(method => {
  arrProto[method] = function() {
    // update view

    oldArrayProto[method].apply(this, arguments)
  }
})

function observe(target) {
  if (typeof target !== 'object' || target === null) return target

  if (Array.isArray(target)) {
    target.__proto__ = arrProto
  }
  for (let key in target) {
    defineReactive(target, key, target[key])
  }
}



function defineReactive(target, key, value) {
  // deep observe
  observe(target)

  Object.defineProperty(target, key, {
    get() {
      return value
    },

    set(newVal) {
      if (newVal !== value) {
        // deep observe
        observe(newVal)

        value = newVal
        // update view
      }
    }
  })
}
```
监听数组

监听对象

复杂对象，深度监听

缺点 新增 删除

### vue 组件化

传统组件 -> 静态渲染 -> 更新依赖于操作DOM
数据驱动视图

MVVM
（model <- viewModel -> view)

### VNode, diff
DOM操作耗费性能

vnode
```js
{
  tag: 'div',
  data: null,
  children: [],
  el: null
}
```

只比较同一层级l
tag不同

tag 和 key 相同

patch(container, vnode)

pathc(preVNode, vnode)

```js
function sameVNode(preVNode, nextVNode) {
  return preVNode.key === nextVNode.key && preVNode.tag === nextVNode.tag
}
```

### 模板编译

### 流程
初次渲染
解析模板 -> render function
触发响应式，监听data, (getter, setter)
执行render 函数 -> vnode -> patchVnode
修改data, 触发setter
重新执行render函数，生成newVNode -> pathc(vnode, new Vnode)

### 异步渲染
$nextTick

一次性更新视图，
减少DOM操作

### 前端路由
hash
 hash变化会触发到网页跳转
 hash变化不会触发页面刷新，
 hash不会提交到server

 hashchange

history

history.pushState
window.onpopstate

```js

const state = {name: 'page'}
history.pushSate(state, '', 'page1')
window.onpopstate = event => {
  console.log(event.state, location.path)
}
```

