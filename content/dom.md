---
title: "Dom"
metaTitle: "This is the title tag of this page"
metaDescription: "This is the vue description"
---

### render-step

process html -> build dom tree
process css -> build cssom tree
combine dom & css to render tree
run layout on the render tree to computed geometry of each node
paint nodes to screen

### render-tree

DOM-tree
CSSOM-tree

render-tree
(traverse each visible tree)
(apply cssom rules to each visible tree)
(emit visible nodes with content and their computed styles)

-> layout -> paint

```js
```
