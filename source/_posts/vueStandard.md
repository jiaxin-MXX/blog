---
title: Vue开发规范
date: 2020-05-21
tags: 
 - vue
 - 规范
categories: [Vue,规范]
keywords:
description:
top_img:
cover: '2020/05/21/vuePull/vue.png'
---
# Vue开发规范参考


**本文章参考Vue官方[风格指南](https://cn.vuejs.org/v2/style-guide/)加上自己理解进行总结编写**

> 如果有更好的建议欢迎提出

---
[toc]
---



##   规则归类

### 优先级★★：[必须遵守的](# 必须遵守的)
	**这些规则会帮你规避错误，所以学习并接受它们带来的全部代价吧。这里面可能存在例外，但应该非常少，且只有你同时精通 JavaScript 和 Vue 才可以这样做。**

### 优先级★：[强烈推荐](# 强烈推荐)
	**这些规则能够在绝大多数工程中改善可读性和开发体验。即使你违反了，代码还是能照常运行，但例外应该尽可能少且有合理的理由。**
### 优先级：[禁用](# 禁用)
	**有些 Vue 特性的存在是为了照顾极端情况或帮助老代码的平稳迁移。当被过度使用时，这些特性会让你的代码难于维护甚至变成 bug 的来源。这些规则是为了给有潜在风险的特性敲个警钟，并说明它们什么时候不应该使用以及为什么。**

## 必须遵守的
### 组件名为多个单词

**组件名应该始终是多个单词的，根组件 `App` 以及 ``、`` 之类的 Vue 内置组件除外。**

```js
//bad
Vue.component('todo',{})
export default{name:'Todo'}
//good
Vue.component('todo-item',{})
export default{name:'TodoItem'}
```

### 组建数据
**组件的<font color='red'> data </font>必须是一个函数（避免污染全局的data）**

```js
//bad
Vue.component('some-comp', {
  data: {
    foo: 'bar'
  }
})
//good
Vue.component('some-comp', {
  data: function () {
    return {
      foo: 'bar'
    }
  }
})
```

### Prop定义

**在你提交的代码中，prop 的定义应该尽量详细，至少需要指定其类型**

```js
//bad
props: ['status']
//good
props: {
  status: String
}
```

### 为<font color='red'> v-for </font>设置专属的`key`

**在组件上*总是*必须用 `key` 配合 `v-for`，以便维护内部组件及其子树的状态**

```html
//bad
<ul>
  <li v-for="todo in todos">
    {{ todo.text }}
  </li>
</ul>
//good
<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

### 避免 <font color='red'> `v-for `</font>和  <font color='red'> `v-if `</font> 用在一起

**永远不要把 `v-if` 和 `v-for` 同时用在同一个元素上。`v-for`的优先级要比`v-if`高，放在一起有时候会增加运算量**

一般我们在两种常见的情况下会倾向于这样做：

- 为了过滤一个列表中的项目 (比如 `v-for="user in users" v-if="user.isActive"`)。在这种情形下，请将 `users` 替换为一个计算属性 (比如 `activeUsers`)，让其返回过滤后的列表。
- 为了避免渲染本应该被隐藏的列表 (比如 `v-for="user in users" v-if="shouldShowUsers"`)。这种情形下，请将 `v-if` 移动至容器元素上 (比如 `ul`、`ol`)。

### 为组件样式设置作用域

**对于应用来说，顶级 `App` 组件和布局组件中的样式可以是全局的，但是其它所有组件都应该是有作用域的**

```css
/**bad**/
<style>
.btn-close {
  background-color: red;
}
</style>
/**good**/
<style scoped>
.button-close {
  background-color: red;
}
</style>
```
## 强烈推荐

### 组件的顶级元素的顺序

**单文件组件应该总是让 `<script>` 、`<template>`和 `<style>`标签的顺序保持一致。且 `<style>` 要放在最后，因为另外两个标签至少要有一个。**

```html
//good
<script>/* ... */</script>
<template>...</template>
<style>/* ... */</style>
//or
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```



### JS/JSX 中的组件名大小写

**JS/JSX 中的组件名应该始终是 PascalCase 的，尽管在较为简单的应用中只使用 `Vue.component` 进行全局组件注册时，可以使用 kebab-case 字符串**

```js
//bad
import myComponent from './MyComponent.vue'
Vue.component('myComponent', {})
//good
import MyComponent from './MyComponent.vue'
Vue.component('MyComponent', {})
Vue.component('my-component', {})
```

### 紧密耦合的组件名

**和父组件紧密耦合的子组件应该以父组件名作为前缀命名**

```
//bad
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue
//good
components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue
```

### 组件名中的单词顺序

**组件名应该以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。**

```
//bad
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
//good
components/
|- SearchButtonClear.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxLaunchOnStartup.vue
```
### 组件选项顺序

```js
  - components
  - props
  - data
  - computed
  - created
  - mounted
  - metods
  - filter
  - watch
```



### 多个`attribute`的元素

**多个 attribute 的元素应该分多行撰写，每个 attribute 一行。**

```html
//bad
<MyComponent foo="a" bar="b" baz="c"/>
//good
<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>
```

### `attribute`的顺序

**原生属性放在前面，指令放在后面**

```js
  - class
  - id,ref
  - name
  - data-*
  - src, for, type, href,value,maxlength,max,min,pattern
  - title, alt，placeholder
  - required,readonly,disabled
  - is
  - v-for
  - key
  - v-if
  - v-else-if
  - v-else
  - v-show
  - v-cloak
  - v-pre
  - v-once
  - v-model
  - v-bind,:
  - v-on,@
  - v-html
  - v-text
```

### 模板中简单的表达式

**组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。(活用计算属性)**

```
//bad
fullName.split(' ').map(function (word) {
  return word[0].toUpperCase() + word.slice(1);
}).join(' ')
//good
{{ normalizedFullName }}
computed: {
  normalizedFullName: function () {
    return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1);
    }).join(' ')
  }
}
```

### 使用ES6风格编码

- 定义变量使用 let ,定义常量使用 const（涉及到块级作用域）

- 静态字符串一律使用单引号或反引号，动态字符串使用反引号

```js
  // bad
  const a = 'foobar'
  const b = 'foo' + a + 'bar'
  // good
  const a = 'foobar'
  const b = `foo${a}bar`
  const c = 'foobar'
```

- 数组或对象成员对变量赋值时，优先使用解构赋值

```js

  // 数组解构赋值
  const arr = [1, 2, 3, 4]
  // bad
  const first = arr[0]
  const second = arr[1]
 
  // good
  const [first, second] = arr
```




### 指令缩写

**指令缩写 (用 `:` 表示 `v-bind:`、用 `@` 表示 `v-on:` 和用 `#` 表示 `v-slot:`) 应该要么都用要么都不用。**

### `methods`方法命名规范

- **驼峰式命名，统一使用动词或者动词+名词形式**

```js
//bad
go、nextPage、show、open、login 
// good
jumpPage、openCarInfoDialog
```

- **请求数据的方法，以data结尾**

```js
//bad
takeData、confirmData、getList、postForm
// good
getListData、postFormData
```
## 禁用
### scoped中的元素选择器

**元素选择器应该避免在 `scoped` 中出现**

```html
//bad
<template>
  <button>X</button>
</template>

<style scoped>
button {
  background-color: red;
}
</style>
//good
<template>
  <button class="btn btn-close">X</button>
</template>

<style scoped>
.btn-close {
  background-color: red;
}
</style>
```

### 隐性的父子组件通信

**应该优先通过 prop 和`this.$emit`进行父子组件之间的通信，而不是 `this.$parent` 或变更 prop**

```html
//bad
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  template: '<input v-model="todo.text">'
})
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  methods: {
    removeTodo () {
      var vm = this
      vm.$parent.todos = vm.$parent.todos.filter(function (todo) {
        return todo.id !== vm.todo.id
      })
    }
  },
  template: `
    <span>
      {{ todo.text }}
      <button @click="removeTodo">
        X
      </button>
    </span>
  `
})
```

```html
//good
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  template: `
    <input
      :value="todo.text"
      @input="$emit('input', $event.target.value)"
    >
  `
})
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  template: `
    <span>
      {{ todo.text }}
      <button @click="$emit('delete')">
        X
      </button>
    </span>
  `
})
```


