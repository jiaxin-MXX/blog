---
title: Vue伪瀑布流的实现
date: 2020-05-21
tags: [Vue]
categories: [Vue,瀑布流]
keywords:
description:
top_img:
cover: '2020/05/21/vuePull/vue.png'
---
# Vue伪瀑布流的实现

> 我记得是去年暑假的时候，我的好基友给我们分享了一下瀑布流，当时没有认真听，结果今天遇见了，而且是跟vue结合的。

**如果对传统的实现方法还不了解可以看一下这个[老哥](https://blog.csdn.net/zhinianling/article/details/97271944)的**

> 思路：我中心屏幕分为n列，然后把数据分为n份，分别填充到列中。类似于入栈

```html
#list是你要分成几列
#哦对了 我这是结合antd vue组件库使用的
<a-row>
  <a-col v-for="(value,key) in this.imagelist" :key="key" :span="24/list">
       <img
         :alt="item.name"
         :src="$axios.defaults.baseURL + item.src"
         slot="cover"
         width="100%"
         style="border-radius: 3px"
        />        
   </a-col>
</a-row>
```

> 数据的格式（imagelist），看一下就会明白了

```js
imagelist={
    0:[
        {src:image.src}
    ],
    1:[
        {src:image.src}
    ],
}
//这样的话是分成了两列
```

> 弄到上面的话瀑布流已经出来了，但是根据窗口大小改变列数就要这么搞了

```js
data(){
    return{
     	screenWidth: document.documentElement.clientWidth, //屏幕宽度
    	list: null,   
    }
}
mounted(){
    window.onresize = () => {
      // 定义窗口大小变更通知事件
      this.screenWidth = document.documentElement.clientWidth; //窗口宽度
    };
}
methods：{
    pandleWidth(val) {
      if (val < 576) {
        this.list = 1;
      } else if (val >= 576 && val < 768) {
        this.list = 2;
      } else if (val >= 768 && val < 992) {
        this.list = 3;
      } else if (val >= 992 && val < 1200) {
        this.list = 4;
      } else {
        this.list = 6;
      }
    }
}
watch: {
    screenWidth: function(val) {
      //监听屏幕宽度变化
      this.pandleWidth(val);
    },
    list: function() {
        //监听变成几列
      let count = 0;
      this.imagelist = _.groupBy(this.images, () => {
        count += 1;//_.groupBy是引用的lodash库中的方法
        return (count - 1) % this.list;
      });
    }
  }
```

> 现在就可以根据窗口大小改变列数了~，但是初始的this.list还没有设置

```js
 this.$axios
        .post(url, postData)
        .then(res => {
            this.images = [];//存最初的images列表
            this.images = res.data.list;
            this.pandleWidth(this.screenWidth);
            let count = 0;
            this.imagelist = _.groupBy(this.images, () => {
              count += 1;
              return (count - 1) % this.list;
            });
        })
        .catch(err => {
          this.$message.error(`网络错误：${err}`);
          this.loading = false;
        });
```

> 现在应该就ok了~温馨提示瀑布流在图片高度差距比较大的时候才好看。。。。否则还是比较low的。。。。