---
title: js复盘回顾
author: god23bin
categories: 前端
tags: 
- Js
- 前端
- 入门
abbrlink: b30d2e9f
description: 这里就是回顾下js，太久没看了
date: 2021-12-28 20:22:33
---

## js复盘

回顾下js基础知识

**为什么学习js？**

前端三件套，必学。

**js可以干嘛？**

多了去了，常见的就进行与用户交互、制作动态效果这些，说细了可以干很多事情。

**怎么写？**

只要有个文本编辑器，随时随地都能写，主要写完是给浏览器解析的。



在html中js代码写在<script></script>之间

```html
<script>
	... //js代码
</script>>
```

当然你也可以直接写一个js文件，直接在js文件里面写js代码



### 常见互动

`document.write();` 可用于直接向 HTML 输出流写内容，简单的说就是直接在网页中输出内容。

`alert(字符串或变量);  `alert弹出消息对话框(包含一个确定按钮)。

`confirm(str);`str：在消息对话框中要显示的文本；返回值: Boolean值



### 控制DOM

`document.getElementById("id");`通过id获取元素

`Object.style.display = value;`

- Object是获取的元素对象，比如通过document.getElementById("id")获取的元素。
- value取值none或block



### 基本语法

变量声明

`var 变量名;`

实际上和其他编程语言差不多，只不过js就只有var这种类型。当然是在一开始的标准下的，在ES6标准下的js可以通过let、const来声明。

创建数组

`var arr = new Array(6);`

1. 创建的新数组是空数组，没有值，如输出，则显示undefined。
2. 虽然创建数组时，指定了长度，但实际上数组都是变长的，也就是说即使指定了长度为8，仍然可以将元素存储在规定长度以外。

然后一些流程控制语句，if、while、for、do...while、switch这些都差不多，还有什么break、continue。



函数的定义

```js
function 函数名(参数1，参数2...)
{
     函数体;
}
```

### 关于事件

| 事件        | 说明                 |
| ----------- | -------------------- |
| onclick     | 鼠标单击事件         |
| onmouseover | 鼠标经过事件         |
| onmouseout  | 鼠标移开事件         |
| onchange    | 文本框内容改变事件   |
| onselect    | 文本框内容被选中事件 |
| onfocus     | 光标聚集             |
| onblur      | 光标离开             |
| onloade     | 网页导入             |
| onunload    | 关闭网页             |



### 关于内置对象

js提供多个内建对象，比如 String、Date、Array 等等，使用对象前先定义。

如下使用数组对象：

```js
// 使用new关键字定义对象
var objectName = new Array();
// 或者
var objectName = [];
```



日期对象可以储存任意一个日期，并且可以精确到毫秒数（1/1000 秒）。

定义一个时间对象 :

```js
var Udate = new Date(); 
```



定义字符串的方法就是直接赋值。比如：

```js
var mystr = "I love JavaScript!"
```



### 关于浏览器对象

window对象、history对象、location对象、navigator对象、screen对象，这些了解下就OK了，我也不经常用到。







