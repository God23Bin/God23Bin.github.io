---
title: Element UI-入门
author: god23bin
categories: 前端
tags: 
- Element UI
- 前端
- 入门
abbrlink: f4d72f5d
description: Element UI-入门学习记录
date: 2022-01-12 11:12:49
---



## 入门

基于vue2.x的官方文档：[https://element.eleme.cn/#/zh-CN](https://element.eleme.cn/#/zh-CN)

Element，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库。

基于vue3.x的官方文档：[https://element-plus.org/zh-CN/#/zh-CN](https://element-plus.org/zh-CN/#/zh-CN)

Element Plus，一套为开发者、设计师和产品经理准备的基于 Vue 3 的桌面端组件库

---

那么从2.x的学起。

### 安装

首先就是在创建好的vue项目中安装element-ui，我这里使用vue2.x的版本。

```shell
npm install element-ui --save
```

注册（引入）Element-UI

main.js

```js
import Vue from 'vue'
import App from './App.vue'
import ElementUI from 'element-ui'  // ElementUI全部组件
import 'element-ui/lib/theme-chalk/index.css'  // ElementUI的css

Vue.config.productionTip = false

Vue.use(ElementUI)  // 使用ElementUI，用use方法进行安装

new Vue({
  render: h => h(App),
}).$mount('#app')

```

### 使用

直接参考文档进行使用，想要什么复制什么，然后自己再改改就OK~