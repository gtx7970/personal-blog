---
title: "vue base"
metaTitle: "vue base"
metaDescription: "This is the meta description for this page"
---

vue 2.x basic

## Vue基本使用

v-html
```html
<p v-html="></p>
```

computed 缓存
```js
export default {
  data() {
    return {
      num: 20
    }
  },
  computed: {
    double() {
      return this.num * 2
    }
  }
}
```

watch 监听引用类型，拿不到oldVal (指向同一个)

class (对象，数组)
style

v-if, v-show区别, 使用场景

v-show(display: none )

循环列表
v-for (遍历数组, 对象)
key
v-for 和 v-if (v-for 优先级高于v-if)

event (event是原生的，挂载到当前元素)

表单
v-model

v-model.trim
v-model.lazy
v-model.number
### Vue父子组件通讯

## vue-router的原理是什么?
> 实现原理：vue-router的原理就是更新视图而不重新请求页面vue-router可以通过mode参数设置为三种模式：hash模式、history模式、abstract模式。- 1.hash模式 默认是hash模式，基于浏览器history api，使用window.addEventListener('hashchange',callback,false)对浏览器地址进行监听。当调用push时，把新路由添加到浏览器访问历史的栈顶。使用replace时，把浏览器访问历史的栈顶路由替换成新路由 hash的值等于url中#及其以后的内容。浏览器是根据hash值的变化，将页面加载到相应的DOM位置。锚点变化只是浏览器的行为，每次锚点变化后依然会在浏览器中留下一条历史记录，可以通过浏览器的后退按钮回到上一个位置- 2.History history模式，基于浏览器history api ，使用window.onpopstate对浏览器地址进行监听。对浏览器history api中的pushState()、replaceState()进行封装，当方法调用，会对浏览器的历史栈进行修改。从而实现URL的跳转而无需加载页面 但是他的问题在于当刷新页面的时候会走后端路由，所以需要服务端的辅助来兜底，避免URL无法匹配到资源时能返回页面- 3.abstract 不涉及和浏览器地址的相关记录。流程跟hash模式一样，通过数组维护模拟浏览器的历史记录栈 服务端下使用。使用一个不依赖于浏览器的浏览器历史虚拟管理后台- 4.总结 hash模式和history模式都是通过window.addEvevtListenter()方法监听 hashchange和popState进行相应路由的操作。可以通过back、foward、go等方法访问浏览器的历史记录栈，进行各种跳转。而abstract模式是自己维护一个模拟的浏览器历史记录栈的数组
## 7、防抖节流原理、区别以及应用

