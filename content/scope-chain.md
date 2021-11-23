---
title: "scope-chain"
metaTitle: "This is the title tag of this page"
metaDescription: "This is the vue description"
---

### scope

```js
function foo() {
  function bar() {}
}

// 创建时
foo.[[scope]] = [
  globalContext.VO
]
bar.[[scope]] = [
  fooContext.AO,
  globalContext.VO
]

// 激活
Scope = [AO].concat([[Scope]]);
```
