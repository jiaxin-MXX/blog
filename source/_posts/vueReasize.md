---
title: Vue监听dom大小改变
date: 2020-05-21 17:24:12
tags: [Vue Dom]
categories: [Vue,Resize]
keywords:
description:
top_img:
cover: '2020/05/21/vuePull/vue.png'
---
### Vue监听dom大小改变
> 需求描述：layout左边菜单栏收缩，右边的content区域的swiper宽度没有改变（没有图，朋友的问题，大体画一下）

![](C:\Users\ljk\Desktop\图片1.png)

> 类似于点击折叠左边目录会变小，右边内容区域会变大，但是swiper在刚开始的时候就确定了宽度，所以我的想法是监听右边宽度大小去updata一下。但是我用vue的watch监听$refs.swiper.offsetwidth失败了！！！！但是宽度确实是在改变的很费解，先说一下解决方法把

### 1、使用[element-resize-detector](https://github.com/wnr/element-resize-detector)

```js
var elementResizeDetectorMaker = require("element-resize-detector")
erd.listenTo(document.getElementById("swiper"), function(element) {
  var width = element.offsetWidth;
  var height = element.offsetHeight;
  console.log("Size: " + width + "x" + height);
});
//别为我为什么vue用getid。。我懒得改了。用ref获取dom也可以
```

### 2 、使用[ResizeObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/ResizeObserver)

```js
    this.observer = new ResizeObserver(entries => {
      entries.forEach(() => {
        swiper.updata()
      })
    })
    let content = document.getElementById('swiper')
    this.observer.observe(content)
```

>至于为什么watch不能兼听我的宽度的问题。。我感觉是只能监听被双向绑定的数据，也就是MVVM的数据，比如我们经常用watch监听data和$route。但还是不太确定希望有大佬帮我~~~

