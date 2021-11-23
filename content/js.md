---
title: "JS"
metaTitle: "This is the title tag of this page"
metaDescription: "This is the vue description"
---

### mock instanceOf

```js
function instanceOfMock(l, r) {
  if (typeof l !== 'object') {
    return false
  }

  while (true) {
    if (l === null) return false

    if (r.prototype === l.__proto__) {
      return true
    }

    l = l.__proto__
  }
}
```

### 基本类型

基本数据类型用栈存储
引用类型用堆存储

闭包变量存在堆内存中


```js
boolean
null
undefined
number
string
symbol
bigint
```

### 垃圾回收

### EventLoop

```js
for (;;) {

}
```

### 深拷贝

1. JSON.strigfy
```js
JSON.parse(JSON.stringfy(obj))
```
限制

2. 基础版
```js
function deepCopy(obj) {
  let res = {}
  if (typeof obj !== 'object') return obj

  for (let key in obj) {
    if (typeof obj[key] === 'object') res[key] = deepCopy(res[key])
    else res[key] = obj[key]
  }

  return res
}
```
限制 1 2只针对普通引用类型, 不能针对Array、Date、RegExp、Error、Function 3循环引用

3. 改进版
```js
// 遍历对象不可枚举类型和Symbol类型
Reflect.ownKeys()

// 获得对象所有属性
Object.getOwnPropertyDescriptors
```

```js
const toString = Object.prototype.toString
function deepCopy(obj) {
  if (toString.call(obj) === '[object Date]') {
    return new Date(obj)
  }

  if (toString.call(obj) === '[object RegExp]') {
    return new RegExp(obj)
  }
}
```

### 继承

1. 原型链继承
```js
function Parent(name) {
  this.name = name
}

function Child(type) {
  this.type = type
}

Child.prototype = new Parent()

// 缺点
// 实例共用一个原型对象
```

2. 构造函数继承(call)

```js
function Parent() {}

Parent.prototype.getName = function() {}

function Child(name) {
  Parent.call(this, name)
  this.type = 'child'
}

// 缺点
// 每次创建实例都会创建一遍方法
```

3. 组合继承
```js
function Parent(name) {
  this.name = name
}

Parent.prototype.getName = function () {
  console.log(this.name)
}

function Children(name, age) {
  Parent.call(this, name);
  this.age = age
}

Child.prototype = new Parent()

```

4. 原型式继承

5. 寄生式继承

6. 寄生组合式继承

### 闭包


### 位操作符


### call stack, event loop, task

task
micro task

```js
while (queue.waitForMessage()) {
  queue.processNextMessage()
}
```

每个线程都有自己的event loop

Tasks(setTimeout)

Microtasks   straight after the currently executing script,

The microtask queue is processed after callbacks as long as no other JavaScript is mid-execution, and at the end of each task.


### requestAnimationFrame

```js
setInterval(function() {
  // do something
}, 1000 / 60)

let id
function repeatOfen() {
  // do sth
  id = requestAnnimation(repeatOfen)
}

requestAnnimation(repeatOfen)

cancelAnimationFrame(id)
```

### JS Engines

js Source Code -> parser -> ast -> 解释器 -> bytecode -> 优化 -> 机器码

### 位操作符

二进制

```js
Number(113).toString(2)  // 1110001
```

&
```js
1100 & 1111

1100 // 12
```

|
```js
1100 | 1111

1111 // 15
```

~
```js
~1100

0011
```

^
```js
1100 ^ 1111

1111
```

### Promise
