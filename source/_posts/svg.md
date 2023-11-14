---
title: 根据指定宽度来对SVG内文字换行的方法探索
date: 2022-05-30 11:48:13
tags: [CSS]
categories: [前端]
---
## svg 里的 text 元素不像普通 DOM 元素那样能很好地处理文本换行，所以需要特殊对待，通常是使用tspsn和foreignObject标签来包裹文字内容

1.用 tspan 把 text 拆成多行，重新计算每个tspan 的 y 坐标

2.用 foreignObject 包裹 DOM 元素，利用 DOM 的文本布局能力自动处理换行。
需要注意的是，当我们需要对节点中文字的换行规则进行设置时，可以使用css样式中的word-break和word-wrap属性。

> word-break:break-all,例如div宽400px，它的内容就会到400px自动换行，如果该行末端有个英文单词很长（congratulation等），它会把单词截断，变成该行末端为conra(congratulation的前端部分)，下一行为tulation（conguatulation）的后端部分了。

>word-wrap:break-word,例子与上面一样，但区别就是它会把congratulation整个单词看成一个整体，如果该行末端宽度不够显示整个单词，它会自动把整个单词放到下一行，而不会把单词截断掉的。

来自 <https://segmentfault.com/a/1190000003710063> 


