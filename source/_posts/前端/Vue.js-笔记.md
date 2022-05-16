---
title: Vue.js-入门
author: god23bin
categories: 前端
tags: 
- Vue.js
- 前端
- 入门
abbrlink: d95f2c0c
description: Vue.js 入门学习记录
date: 2022-01-10 09:15:35
---

## 入门

> Vue官网：[https://cn.vuejs.org/](https://cn.vuejs.org/)

直接下载Vue.js

### 初体验

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue入门</title>
    <!-- 用script引入vue的话，放在head更好-->
    <script src="./vue.js"></script>
</head>
<body>
    <div id="root">{{msg}}</div>

    <script>
        new Vue({
            el: "#root",
            data: {
                msg: "Hello World"
            }
        })

    </script>
</body>
</html>
```



### 挂载点、模板、实例

挂载点：Vue实例中el对应的dom对象，比如这里el: "#root"，对应`<div id="root"></div>`这个div。Vue只会处理挂载点的内容。

模板：在挂载点里面的内容我们都叫为模板。可以把模板写在Vue实例里。

实例：new Vue();

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue入门</title>
    <!-- 用script引入vue的话，放在head更好-->
    <script src="./vue.js"></script>
</head>
<body>
    <!-- 挂载点、模板、实例之间的关系 -->
    <!-- <div id="root">{{msg}}</div> -->

    <div id="root">
        <!-- 挂载点里面的内容就是模板 -->
        <!-- <h2>Hi {{msg}}</h2> -->
    </div>

    <script>
        // vue 实例
        new Vue({
            el: "#root",
            template: '<h2>Hi {{msg}}</h2>', // 模板可以写在Vue实例中，即在template属性里
            data: {
                msg: "Hello World"
            }
        })

    </script>
</body>
</html>
```



### 数据展示-插值表达式、事件绑定

两个大括号括起来的的语法，就是插值表达式。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue入门</title>
    <script src="./vue.js"></script>
</head>
<body>

    <div id="root">
        <!-- 两个大括号的语法，我们称为插值表达式 -->
        <h2>{{number}}</h2>

        <h2 v-text="number"></h2>

        <h2 v-html="number"></h2>

        <!-- v-text，直接输出字符串 -->
        <div v-text="content"></div>    
        <!-- v-html，有html标签，就显示html本该显示的，原来是h2现在还是h2-->
        <div v-html="content"></div>

        <!-- 绑定事件 -->
        <!-- 
            1. v-on:click这个模板指令
            2. 方法写在Vue实例里面的methods属性
            3. 方法触发，想让页面发生变化，不需要操作dom，通过改变Vue实例的数据就可以了。
        -->
        <!-- <div v-on:click="handleClick">{{msg}}</div> -->
        <!-- v-on:click可以简写成@click -->
        <div @click="handleClick">{{msg}}</div>
    </div>


    <script>
        // vue 实例
        new Vue({
            el: "#root",
            data: {
                msg: "Hello World",
                number: 123,
                content: "<h2>I love you</h2>"
            },
            methods: {
                handleClick: function() {
                    this.msg = "你好世界";
                }
            }
        })

    </script>
</body>
</html>
```

#### 关于v-on:的参数

上述的事件绑定是没有传递参数的，那么这里就需要知道如何传递参数。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-on:click的传参问题</title>
    <script src="./vue.js"></script>
</head>
<body>
    <div id="root">
        <h2>不带参数</h2>
        <button @click="handleClick1">@click="handleClick1"</button>
        <button @click="handleClick1()">@click="handleClick1()"</button>


        <h2>需要一个参数，但是省略了方法的括号</h2>
        <button @click="handleClick2">@click="handleClick2"，vue会将浏览器产生的event事件对象传入作为参数</button>

        <h2>需要event对象，还需其他参数</h2>
        <button @click="handleClick3(123, $event)">使用$event：@click="handleClick3(123, $event)"</button>

    </div>
    
    <script>
        const app = new Vue({
            el: '#root',
            data: {

            },
            methods: {
                // ES6 方法的简化写法
                handleClick1() {
                    console.log('--------------1');
                },
                handleClick2(parameter) {
                    console.log('--------------' + parameter);
                },
                handleClick3(parameter, event) {
                    console.log(parameter + '---------' + event);
                }
            }
        });
    </script>
</body>
</html>
```

#### 关于v-on修饰符

> [https://cn.vuejs.org/v2/guide/syntax.html#%修饰符](https://cn.vuejs.org/v2/guide/syntax.html#%E4%BF%AE%E9%A5%B0%E7%AC%A6)

- `.stop`可以防止冒泡
- `.prevent`可以阻止默认事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-on:修饰符</title>
    <script src="./vue.js"></script>
</head>
<body>

    <div id="root">
        <div>
            <h1>v-on:修饰符的使用</h1>
            <h2>.stop防止冒泡</h2>
            <div v-on:click="handleClickDiv">
                这里是最外层的Div
                <!-- <button v-on:click="handleClick">按钮1</button> -->
                <!-- 使用   .stop   可以防止冒泡（即你点击button，那么父节点的方法就不会执行了） -->
                <button v-on:click.stop="handleClick">按钮1</button>
            </div>

            <h2>.prevent阻止默认事件</h2>
            <div>
                <form action="https://www.bilibili.com">
                    <input type="text">
                    <!-- <button v-on:click="handleSubmit" type="submit">提交表单信息</button> -->
                    <button v-on:click.prevent="handleSubmit" type="submit">提交表单信息</button>
                </form>
            </div>

            <h2>键盘点击事件</h2>
            <div>
                <h3>监听所有键盘敲击</h3>
                <input v-on:keyup="handleKeyup" type="text">
                <h3>监听回车敲击</h3>
                <input v-on:keyup.enter="handleKeyupEnter" type="text">
            </div>
        </div>
    </div>

    <script>
        const app = new Vue({
            el: '#root',
            methods: {
                handleClick() {
                    console.log('--------------我-按钮');
                },
                handleClickDiv() {
                    console.log('--------------我-Div');
                },
                handleSubmit() {
                    console.log('--------------处理提交信息');
                },
                handleKeyup() {
                    console.log('--------------敲击键盘！');
                },
                handleKeyupEnter() {
                    console.log('--------------敲击回车！');
                }
            }
        });
        
    </script>
    
</body>
</html>
```





### 属性绑定和双向数据绑定

- v-bind:属性

- v-model

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>属性绑定和双向数据绑定</title>
    <script src="./vue.js"></script>
</head>
<body>
    <!-- 
    <div id="root">
        <div title="这是HelloWorld">Hello World</div>
    </div> 
    -->


    <!-- 
    <div id="root">
        <div v-bind:title="title">Hello World</div>
    </div> 
    -->

    <!-- 使用了模板指令后，等号后面的内容，相当于js了 -->
    <div id="root">
        <!-- 在编码的时候，一般使用:来代替v-bind: -->
        <div :title="'我在这里' + title">Hello World</div>

        <h2>单向绑定</h2>
        <!-- 单向绑定：数据可以决定页面，页面的不能决定数据的内容 -->
        <input :value="content" type="text">
        <div>{{content}}</div>
        <h2>双向绑定</h2>
        <!-- 双向绑定：数据可以决定页面，页面可以决定数据 -->
        <input v-model="content" type="text">
    </div>




    <script>
        new Vue({
            el: "#root",
            data: {
                title: "这是从Vue实例来的title",
                content: "这是content"
            }
        });

    </script>
    
</body>
</html>
```

单向绑定=属性绑定，一般就是你绑定标签的某个属性，比如上面的就是标签的title属性、value属性。

双向绑定，一般就是你Vue实例中的变量，比如上面的就是content这个变量。

#### v-model结合radio、checkbox、select类型

**v-model结合radio类型**

```html
<input type="radio" id="male" value="男" v-model="sex" >
<input type="radio" id="female" value="女" v-model="sex" >

...
data: {
	sex: "",
}
...
```

**v-model结合checkbox类型**

对于单选框的话，双向绑定的是个布尔值

```html
<label for="license">
	<input type="checkbox" id="license" v-model="isAgree">我已同意协议
</label>

...
data: {
	isAgree: false,
}
...
```

对于复选框，双向绑定是个数组，因为你可以选择多个框框

```html
<label for="basketball">
    <input id="basketball" type="checkbox" value="篮球" v-model="hobbies">篮球
</label>
<label for="football">
    <input id="football" type="checkbox" value="足球" v-model="hobbies">足球
</label>
<label for="badminton">
    <input id="badminton" type="checkbox" value="羽毛球" v-model="hobbies">羽毛球
</label>
<label for="pingpong">
    <input id="pingpong" type="checkbox" value="乒乓球" v-model="hobbies">乒乓球
</label>

...
data: {
	hobbies: [],
}
...
```

**v-model结合select类型**

```html
<!-- 下拉菜单——单选 -->
<select v-model="fruit">
    <option value="苹果">苹果</option>
    <option value="香蕉">香蕉</option>
    <option value="凤梨">凤梨</option>
    <option value="葡萄">葡萄</option>
</select>
您选中的是：{{fruit}}

<!-- 下拉菜单——多选 -->
<select v-model="fruits" multiple>
    <option value="苹果">苹果</option>
    <option value="香蕉">香蕉</option>
    <option value="凤梨">凤梨</option>
    <option value="葡萄">葡萄</option>
</select>


...
data: {
	fruit: "香蕉",
	fruits: [],
}
```

**value绑定——值绑定**

```html
<label for="basketball">
    <input id="basketball" type="checkbox" value="篮球" v-model="hobbies">篮球
</label>
<label for="football">
    <input id="football" type="checkbox" value="足球" v-model="hobbies">足球
</label>
<label for="badminton">
    <input id="badminton" type="checkbox" value="羽毛球" v-model="hobbies">羽毛球
</label>
<label for="pingpong">
    <input id="pingpong" type="checkbox" value="乒乓球" v-model="hobbies">乒乓球
</label>
<!-- 这些东西我们从服务器获取，value并不是写死的，那么我们就需要进行value绑定 -->

<label v-for="item of originHobbies" :key="item" :for="item">
	<input type="checkbox" :value="item" :id="item" v-model="hobbies">{{item}}
</label>

...
data: {
	hobbies: [],	// 复选框选中的内容
	originHobbies: ['篮球', '足球', '羽毛球', '乒乓球'], // 服务器获取的原始数据
}
```



#### v-model的修饰符

- **lazy修饰符︰**

  默认情况下，v-model默认是在input事件中同步输入框的数据的。也就是说，一旦有数据发生改变对应的data中的数据就会自动发生改变。

  lazy修饰符可以让数据在失去焦点或者回车时乎才更新

- **number修饰符︰**

  默认情况下，在输入框中无论我们输入的是字母还是数字，都会被当做字符串类型进行处理。

  但是如果我们希望处理的是数字类型，那么最好直接将内容当做数字处理。

  number修饰符可以让在输入框中输入的内容自动转成数字类型。

- **trim修饰符︰**

  如果输入的内容首尾有很多空格，通常我们希望将其去除trim修饰符可以过滤内容左右两边的空格。





### 计算属性与侦听器（监听器）

- computed:

  > 详情看这里：[https://cn.vuejs.org/v2/api/#computed](https://cn.vuejs.org/v2/api/#computed)

- watch:

  > 详情看这里：[https://cn.vuejs.org/v2/api/#watch](https://cn.vuejs.org/v2/api/#watch)

使用场景介绍，watch(异步场景)，computed(数据联动)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>计算属性与侦听器（监听器）</title>
    <script src="./vue.js"></script>
</head>
<body>
    
    <div id="root">
        姓：<input v-model="firstName">
        名：<input v-model="lastName">
        <!-- <div>{{firstName}}{{lastName}}</div> -->
        <div>{{fullName}}</div>
        <div>{{count}}</div>
    </div>

    <script>
        new Vue({
            el: "#root",
            data: {
                firstName: '',
                lastName: '',
                count: 0
            },
            computed: {
                fullName: function() {
                    return this.firstName + ' ' + this.lastName;
                }
            },
            watch: {
                // firstName: function() {
                //     this.count++;
                // },
                // lastName: function() {
                //     this.count++;
                // }
                fullName: function() {
                    this.count++;
                }
            }
        });
    </script>
</body>
</html>
```

计算属性：

- 本质是getter和setter
- 与methods的区别：计算属性多次使用，只会调用一次，它是有缓存的。



watch：

- 监听某个属性的改变





### v-if, v-show, v-for

条件渲染

- v-if
- v-show

条件渲染有什么用？比如你想指定某个时间显示某些内容、某些div这些，那么就需要用到条件渲染了。

列表渲染

- v-for

  v-for可以遍历数组，也可以遍历对象，拿出的是对象的value。使用v-for的时候需要加上key，为什么要加上key呢？这是跟Vue的虚拟dom和diff算法有关，目前先记住需要加上key就对了。

列表渲染有什么用？好比新闻列表，你每条新闻列表直接可以遍历出来，不用手动循环遍历。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-if, v-show, v-for</title>
    <script src="./vue.js"></script>
</head>
<body>
    <div id="root">
        <h2>v-if & v-show的作用</h2>
        <!-- v-if是直接把dom删除 -->
        <div v-if="show">HelloWorld</div>
        <button @click="handleClick">toggle</button>
        <!-- v-show是给dom添加css属性，display: none -->
        <div v-show="show">你好世界</div>

        <h2>v-for的作用，循环内容的展示</h2>
        <ul>
            <!-- <li v-for="item of list">{{item}}</li> -->
            <!-- <li v-for="item of list" :key="item">{{item}}</li> -->
            <li v-for="(item, index) of list" :key="index">{{item}}</li>
        </ul>
    </div>

    <script>
        new Vue({
            el: "#root",
            data: {
                show: true,
                list: [1, 2, 3]
            },
            methods: {
                handleClick: function() {
                    this.show = !this.show;
                }
            }
        });
    </script>
</body>
</html>
```



#### 关于v-for的key

```html
<li v-for="(item, index) of list" :key="index">{{item}}</li>
```

这里的key选用index，实际上，这样并不是最好的做法。最好的做法是直接和要展示的内容一一对应，可以改成下面这样。当然也有例外，目前这里比较难解释，对于我来说。

```html
<li v-for="(item, index) of list" :key="item">{{item}}</li>
```



### ToDoList

小demo，运用之前学的知识

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ToDoList</title>
    <script src="./vue.js"></script>
</head>
<body>
    <div id="root">
        <div>
            <input v-model="inputValue">
            <button @click="handleSubmit" type="submit">提交</button>
        </div>

        <ul>
            <li v-for="(item, index) of list" :key="index">
                {{item}}
            </li>
        </ul>
    </div>
    
    <script>
        new Vue({
            el: "#root",
            data: {
                inputValue: "hello",
                list: []
            },
            methods: {
                handleSubmit: function() {
                    this.list.push(this.inputValue);
                    this.inputValue = "";
                }
            }
        });
    </script>
</body>
</html>
```

**数组有哪些方法是响应式的？**

上面的Demo中，数组list使用push()这个方法，那么页面会及时响应数据。那么数组还有哪些方法是响应式的？

- push()：
  - 在数组末尾添加元素；
  - 这里和栈类似，push直接放入数组，类似元素入栈；
  - 参数可以有多个，比如push('A', 'B', 'C');
- pop()：
  - 在数组末尾删除元素；
  - 同理，类似元素出栈；
  - 参数可多个；
- shift()：在数组开头删除元素；参数可多个
- unshift()：在数组开头添加元素；参数可多个
- splice()：
  - 删除元素/插入元素/替换元素
  - splice(开始删除/插入/替换的位置下标，传入你要删除元素的个数/你要替换的元素的个数/传入0表示插入，这里写插入的元素)
  - 第一个参数：起始位置
  - 第二个参数：可以这样理解，删除几个元素，传入2就删除2个，传入0就不删除
  - 第三个参数：可以这样理解，追加的元素/插入的元素
- sort()：排序
- reverse()：逆置/反转



### 组件拆分

经常提到的组件，那么说说组件的概念。组件就是指页面上的某一部分，当我们做的网页十分庞大的时候，我们可以把大型的网页拆成几个部分，每个部分就是一个小的组件。这样大型项目拆成小组件，那么就容易开发维护了。

**如何定义组件？组件之间如何通信？**

定义组件可以定义全局和局部的。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ToDoList-拆分组件</title>
    <script src="./vue.js"></script>
</head>
<body>
    <div id="root">
        <div>
            <input v-model="inputValue">
            <button @click="handleSubmit" type="submit">提交</button>
        </div>

        <ul>
            <!-- 
            <li v-for="(item, index) of list" :key="index">
                {{item}}
            </li>
            -->
            <todo-item 
                v-for="(item, index) of list" 
                :key="index"
                :content="item"
                ></todo-item>
        </ul>
    </div>
    
    <script>

        // 全局组件
        // Vue.component('todo-item', {
        //     template: '<li>item</li>'
        // });

        // Vue.component('todo-item', {
        //     props: ['content'],
        //     template: '<li>{{content}}</li>'
        // });

        var TodoItem = {
            props: ['content'],
            template: '<li>{{content}}</li>'
        };

        new Vue({
            el: "#root",
            // 局部组件
            components: {
                'todo-item': TodoItem
            },
            data: {
                inputValue: "hello",
                list: []
            },
            methods: {
                handleSubmit: function() {
                    this.list.push(this.inputValue);
                    this.inputValue = "";
                }
            }
        });
    </script>
</body>
</html>
```

**那么以后组件的html都直接在template里写吗？**这样是不是看起来很麻烦，耦合度比较高，js代码中参杂着html代码。

```html
        var TodoItem = {
            props: ['content'],
            template: '<li>{{content}}</li>'
        };
```

显然，我们是可以进行分离的

- 通过script标签

```html
<script type="text/x-template" id="cpn">
	<li>{{content}}</li>
</script>

<script>
    ...
	    var TodoItem = {
            props: ['content'],
            template: '#cpn'
        };
    ...
</script>
```

- 通过template标签

```html
<template id="cpn">
	<li>{{content}}</li>
</template>

<script>
    ...
	    var TodoItem = {
            props: ['content'],
            template: '#cpn'
        };
    ...
</script>
```



### 组件和实例的关系

上面的代码，我们搞了一个组件。实际上，一个组件，它本身也是一个Vue实例，它里面也可以写methods、template这些。可以这么说，就是一个项目可以由许许多多的组件构成，也就是由许许多多的Vue实例构成。之后我们会习惯这样说，把实例与组件划等号，不管提到组件还是实例，都要知道它是一个什么东西，只是不同的时候称呼不同，就是这种感觉。

每个组件都拥有自己的props、data、methods等等。**其中，特别是data，以前data是一个对象，现在就不是了，而是函数，而且会返回一个对象。这样在组件复用的时候，多次创建组件实例，每创建一次就返回一个对象，那么这个对象就对应这个组件，也就是说多个组件实例的data都是属于自己的，并不是共享的。那么如果这个data你还是使用对象，你在组件复用的时候，你修改其中一个组件的data中的数据，所有复用的组件的data数据都同时发生改变，这就是为什么在写组件的时候，data不是写成对象而是写成函数的原因了。目前这样说可能会比较抽象，不过等你熟悉后你就理解了。**

注意：对于上面的例子来帮助我们理解，如果我们没有在Vue root实例下面定义模板（template），那么Vue会按照挂载点寻找，并把下面的模板当作是模板。

**总而言之，每一个组件也是一个Vue的实例，组件=实例。**



### 组件之间的通信

**父组件给子组件通信：是通过给子组件属性绑定的方式传值**

**子组件给父组件通信：是通过发布订阅的方式传值**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ToDoList-删除功能</title>
    <script src="./vue.js"></script>
</head>
<body>
    <div id="root">
        <div>
            <input v-model="inputValue">
            <button @click="handleSubmit" type="submit">提交</button>
        </div>

        <ul>
            <!-- 1  :index="index" 多带一个index参数 -->
            <!-- 4  @delete="handleDelete" 父组件监听子组件向外发布的事件，发生时就执行handleDelete() -->
            <todo-item 
                v-for="(item, index) of list" 
                :key="index"
                :content="item"
                :index="index"
                @delete="handleDelete"
                ></todo-item>
        </ul>
    </div>
    
    <script>

        var TodoItem = {
            props: ['content', 'index'],
            // 2. 点击li
            template: '<li @click="handleClick">{{content}}{{index}}</li>',
            methods: {
                handleClick: function() {
                    // 3. 子组件向外触发一个事件————delete，这个事件携带了子组件的index值
                    this.$emit('delete', this.index);
                }
            }
        };

        new Vue({
            el: "#root",
            // 局部组件
            components: {
                'todo-item': TodoItem
            },
            data: {
                inputValue: "hello",
                list: []
            },
            methods: {
                handleSubmit: function() {
                    this.list.push(this.inputValue);
                    this.inputValue = "";
                },
                // 5
                handleDelete: function(index) { // 子组件传递过来的下标index
                    // alert('delete')
                    this.list.splice(index, 1); // 从对应下标index的地方删除1项
                }
            }
        });
    </script>
</body>
</html>
```



综上，那么

父组件传数据给子组件：子组件使用props这个Vue实例的属性来实现。props可以是数组、可以是对象，当你需要对props进行类型限制/验证（验证的话需要自定义验证函数）时，就需要用对象的写法了。开发的时候用的最多的情况还是使用对象。

支持的类型（当然也支持你自定义的类型）：

- String 
- Number
- Boolean
- Array
- Object
- Date
- Function
- Symbol

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>组件-父组件传数据给子组件</title>
    <script src="./vue.js"></script>
</head>
<body>
    <div id="root">
        <h2>传数据</h2>
        <!-- 父组件这里传数据给子组件 -->
        <cpn :content="list" :msg="m"></cpn>
        <h2>没传数据</h2>
        <cpn></cpn>
    </div>
    
    <template id="cpn">
        <div>
            <ul>
                <li v-for="item of content" :key="item">{{item}}</li>
            </ul>
            <h3>
                {{msg}}
            </h3>
            <ul>
                <li v-for="item of movies" :key="item">{{item}}</li>
            </ul>
        </div>
    </template>
        
    <script>
        
        const cpn = {
            template: '#cpn',
            props: {
                // 1. 类型限制
                content: Array,
                index: Number,
                // 2. 提供一些默认值
                msg: {
                    type: String,
                    default: '我是默认消息',
                },
                // 3. 当对象类型是 Array/Object 时，那么默认值是一个函数
                movies: {
                    type: Array,
                    default() {
                        return ['我是默认电影', '我是默认龙猫'];
                    }
                }
            }
        }
        
        new Vue({
            el: '#root',
            components: {
                // cpn: cpn
                // 属性增强写法，也就是key和value是同样的时候，你可以直接写一个进行简写
                cpn,
            },
            data: {
                list: ['龙珠', '海贼王', '火影忍者'],
                m: '我喜欢你'
            }
        });
    </script>
</body>
</html>
```

关于props里面属性的命名，你可以驼峰命名，但是在标签那里绑定的时候就必须转一下。比如

```html
<div>
    <cpn :my-movies="list"></cpn>
</div>

<template id="cpn">
	<div>
        ...
    </div>
</template>

<script>
	const cpn = {
        template: '#cpn',
        props: {
            myMovies: [],
        }
    }
    
    const app = new Vue({
        ...
        data: {
            list: ['龙珠', '海贼王', '火影忍者']
        }
    });
</script>

```



**我们知道父组件传值给子组件，可以通过props。然后我们如果想在修改子组件的时候，还能同时修改props怎么搞？**——》因为props是由父组件决定的，那么我们就要通过修改父组件的数据，同时来影响props。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>测试父传子/子修改影响父修改/影响props</title>
    <script src="./vue.js"></script>
</head>
<body>

    <div id="root">
        <h2>父组件</h2>
        <h4>stuid: {{stuid}}</h4>
        <h4>classid: {{classid}}</h4>
        <!-- 4. 父组件接收子组件的sidmodify事件、cidmodify事件后，对于处理方法有参数时，可以不写参数，会默认接收传过来的参数 -->
        <cpn :sid="stuid" :cid="classid" @sidmodify="handleSidModify" @cidmodify="handleCidModify"></cpn>
    </div>

    <template id="cpn">
        <div>
            <h2>子组件</h2>
            <h4>props-sid: {{sid}}</h4>
            <h4>props-cid: {{cid}}</h4>
            <!-- 1. 方法有参数，但是你不传参数，那么默认会把事件传进去 -->
            输入s_id: <input v-model="d_sid" @input="sidHandleInput" type="text">
            输入c_id: <input v-model="d_cid" @input="cidHandleInput" type="text">
            <h4>data-d_sid: {{d_sid}}</h4>
            <h4>data-d_cid: {{d_cid}}</h4>
        </div>
    </template>
    <script>

        const cpn = {
            template: '#cpn',
            props: {
                sid: Number,
                cid: Number
            },
            data() {
                return {
                    // d_sid: 0,
                    // d_cid: 0,
                    d_sid: this.sid,
                    d_cid: this.cid,
                }
            },
            methods: {
                // 1. 方法有参数，但是你不传参数，那么默认会把事件传进去
                sidHandleInput(event) {
                    // 2. 子组件的data修改，目的使props也修改，所以得让父组件修改，这样props才会改变
                    d_sid = event.target.value;
                    // 3. 向父组件触发事件，传过去d_sid参数
                    this.$emit("sidmodify", d_sid);
                },
                cidHandleInput(event) {
                    // 子组件的data修改，目的使props也修改，所以得让父组件修改，这样props才会改变
                    c_sid = event.target.value;
                    this.$emit("cidmodify", c_sid);
                }
            }
        };

        const app = new Vue({
            el: '#root',
            data: {
                stuid: 1,
                classid: 2
            },
            components: {
                cpn,
            },
            methods: {
                handleSidModify(d_sid) {
                    // this.stuid = d_sid;
                    this.stuid = parseInt(d_sid);
                },
                handleCidModify(d_cid) {
                    // this.stuid = c_sid;
                    this.classid = parseInt(d_cid);
                }
            }
        });
    </script>
    
</body>
</html>
```



#### $children或者$refs

**如果你父组件想要拿到子组件的对象，然后操作一下子组件，而不是传值，该怎么处理？**这种可以说是组件访问的情况。

使用$children或者$refs，当然后者使用情况最多

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>使用$children/使用$refs——————父组件访问子组件/获取子组件对象</title>
    <script src="./vue.js"></script>
</head>
<body>

    <div id="root">
        <cpn1 ref="bbb"></cpn1>
        <cpn2 ref="iii"></cpn2>
        <cpn3 ref="nnn"></cpn3>

        <button @click="accessChildrenCpn">访问子组件</button>
    </div>

    <template id="cpn1">
        <div>
            <h2>我是子组件1的内容</h2>
        </div>
    </template>

    <template id="cpn2">
        <div>
            <h2>我是子组件2的内容</h2>
        </div>
    </template>

    <template id="cpn3">
        <div>
            <h2>我是子组件3的内容</h2>
        </div>
    </template>
    
    <script>

        const cpn1 = {
            template: "#cpn1",
            data() {
                return {
                    name: "子组件cpn1",
                    msg: "子组件1默认消息"
                }
            },
            methods: {
                showMessage() {
                    consolo.log(this.msg);
                }
            }
        }

        const cpn2 = {
            template: "#cpn2",
            data() {
                return {
                    name: "子组件cpn2",
                    msg: "子组件2默认消息"
                }
            },
            methods: {
                showMessage() {
                    consolo.log(this.msg);
                }
            }
        }

        const cpn3 = {
            template: "#cpn3",
            data() {
                return {
                    name: "子组件cpn3",
                    msg: "子组件3默认消息"
                }
            },
            methods: {
                showMessage() {
                    consolo.log(this.msg);
                }
            }
        }

        const app = new Vue({
            el: "#root",
            components: {
                cpn1,
                cpn2,
                cpn3,
            },
            methods: {
                accessChildrenCpn() {
                    // 1. 使用$children访问子组件对象
                    // console.log(this.$children);
                    // for (let c of this.$children) {
                    //     console.log(c.name);
                    //     console.log(c.msg);
                    // }

                    // 2. 使用$refs访问子组件对象————更加常用的方法！！！！！！！
                    console.log(this.$refs.bbb.name);
                    console.log(this.$refs.iii.name);
                    console.log(this.$refs.nnn.name);
                }
            }
        });
    </script>
</body>
</html>
```



#### $parent或者$root

反过来，子组件想获取父组件，那么就可以通过$parent或者$root来获取，还是一样，后者用的情况比前者多。

### 插槽——slot

理解插槽，抽取相同的东西出来，给定不同内容显示不同内容。例子就是导航栏。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>插槽的使用</title>
    <script src="./vue.js"></script>
</head>
<body>
    <div id="root">
        <cpn></cpn>
        <cpn>这里给插槽写东西，会替换插槽内容</cpn>
        <cpn><button>这不是默认按钮</button></cpn>
        <cpn>
            <h2>标题h2</h2>
            <button>我是按钮</button>
        </cpn>
    </div>

    <template id="cpn">
        <div>
            <h2>我是子组件</h2>
            <slot>
                <h4>这里是插槽默认值</h4>
            </slot>
        </div>
    </template>
    
    <script>
        const cpn = {
            template: "#cpn",
        };

        const app = new Vue({
            el: "#root",
            components: {
                cpn,
            }
        });
    </script>
</body>
</html>
```



1. 插槽的基本使用：slot标签

```html
<slot></slot>
```

2. 插槽的默认值

```html
<slot>这里可以写默认的html代码</slot>
```

3. 如果有多个值同时放到组件中进行替换，那么会一起替换掉插槽的默认内容。

```html
<cpn>
    <h2>标题h2</h2>
    <button>我是按钮</button>
</cpn>
```



当你有多个插槽的时候，你还需要给它加上name属性，以此来区别插槽，这样可以让你准确的替换某个插槽的内容。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>插槽的使用</title>
    <script src="./vue.js"></script>
</head>
<body>
    <div id="root">
        <cpn><span slot="center">搜索框</span></cpn>
        <cpn><button slot="left">返回</button></cpn>
    </div>

    <template id="cpn">
        <div>
            <h2>我是子组件</h2>
            <slot name="left">左边</slot>
            <slot name="center">中间</slot>
            <slot name="right">右边</slot>
        </div>
    </template>
    
    <script>
        const cpn = {
            template: "#cpn",
        };

        const app = new Vue({
            el: "#root",
            components: {
                cpn,
            }
        });
    </script>
</body>
</html>
```



#### 插槽作用域

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>插槽作用域</title>
    <script src="./vue.js"></script>
</head>
<body>
    <div id="root">
        <cpn></cpn>
        <cpn>
            <!-- 2. 目的是获取子组件的planguages -->
            <!-- 3. 这里的xxx也是随便取名 -->
            <!-- <template slot-scope="xxx"> -->
            <template slot-scope="slot">
                <span>{{slot.data.join(' - ')}}</span>
            </template>
        </cpn>
    </div>

    <template id="cpn">
        <div>
            <!-- 1. 这里的yyy随便取名，但是一般是写成data，因为我们取的是数据嘛 -->
            <!-- <slot :yyy="planguages"> -->
            <slot :data="planguages">
                <ul>
                    <li v-for="item of planguages" :key="item">{{item}}</li>
                </ul>
            </slot>
        </div>
    </template>
    
    <script>

        const cpn = {
            template: "#cpn",
            data() {
                return {
                    planguages: ['C++', 'Java', 'JavaScript', 'Go'],
                };
            }
        };

        const app = new Vue({
            el: "#root",
            components: {
                cpn,
            }
        });
    </script>
</body>
</html>
```



### ES6模块化——导入/导出

`import`关键字和`export`关键字

> ES6模块化：[http://es.xiecheng.live/es6/module.html#import](http://es.xiecheng.live/es6/module.html#import)

## 关于webpack

### 什么是webpack？

实际上，几句话很难说清什么是webpack，简而言之，它可以支持前端的模块化开发以及打包。

我们知道模块化开发的时候有规范，比如CommonJS、AMD、CMD、ES6这些。模块化的方式写代码时，浏览器不认识模块化的写法，比如模块化的一些导入导出的语法。于是，webpack就出现了，它可以说是一个工具，可以使你以模块化写的代码变成浏览器认识的代码。同时解决好多问题，比如模块之间依赖的问题。以前我们写js的时候，需要在一个页面下面引用很多js文件，现在可以不用这样，直接用模块化的思想，导出导入，直接写，最后交给webpack打包成一个最终的js文件，然后让这个页面引用这个最终的js文件就可以了。

换句话说，现在的js文件中使用了模块化的方式进行开发，他们可以直接使用吗？

不可以。因为如果直接在index.html引入这两个js文件，浏览器并不识别其中的模块化代码。另外，在真实项目中当有许多这样的js文件时，我们一个个引用非常麻烦，并且后期非常不方便对它们进行管理。

我们应该怎么做呢？使用webpack工具，对多个js文件进行打包。

我们知道，webpack就是一个模块化的打包工具，所以它支持我们代码中写模块化，可以对模块化的代码进行处理。

另外，如果在处理完所有模块之间的关系后，将多个js打包到一个js文件中，引入时就变得非常方便了。

OK，如何打包呢？使用webpack的指令即可

```shell
webpack src/index.js dist/bundle.js
```

打包后会在dist目录下生成bundle.js文件。



### webpack配置

webpack.config.js

```js
const path = require('path');	// 引入path包

module.exports = {
    entry: './src/index.js',
    output: {
        // path 需要为绝对路径。如何获取绝对路径？通过node的包找到。我们引入path这个包。
        // path: './dist',
        path: path.resolv(__dirname, 'dist'),
        filename: 'bundle.js'
    }
}
```



### 关于loader

loader是webpack中一个非常核心的概念。

一些问题

- 在我们之前的实例中，我们主要是用webpack来处理我们写的js代码，并且webpack会自动处理js之间相关的依赖。
- 但是，在开发中我们不仅仅有基本的js代码处理，我们也需要加载css、图片，也包括一些高级的将ES6转成ES5代码，将TypeScript转成ES5代码，将scss、less转成css，将.jsx、.vue文件转成js文件等等。
- 对于webpack本身的能力来说，对于这些转化是不支持的。
- 那怎么办呢？给webpack扩展对应的loader就可以啦。

loader使用过程︰

- 步骤一︰通过npm安装需要使用的loader
- 步骤二∶在webpack.config.js中的modules关键字下进行配置
- 大部分loader我们都可以在webpack的官网中找到，并且学习对应的用法。

#### 处理css

> [https://www.webpackjs.com/guides/asset-management/#加载-css](https://www.webpackjs.com/guides/asset-management/#%E5%8A%A0%E8%BD%BD-css)
>
> css-loader：[https://www.webpackjs.com/loaders/css-loader/](https://www.webpackjs.com/loaders/css-loader/)
>
> less-loader：[https://www.webpackjs.com/loaders/less-loader/](https://www.webpackjs.com/loaders/less-loader/)
>
> url-loader：[https://www.webpackjs.com/loaders/url-loader/](https://www.webpackjs.com/loaders/url-loader/)
>
> file-loader：[https://www.webpackjs.com/loaders/file-loader/](https://www.webpackjs.com/loaders/file-loader/)

#### 处理图片

我们发现webpack自动帮助我们生成一个非常长的名字这是一个32位hash值，目的是防止名字重复

但是，真实开发中，我们可能对打包的图片名字有一定的要求

比如，将所有的图片放在一个文件夹中，跟上图片原来的名称，同时也要防止重复img/name.hash:8 I

所以，我们可以**在options中添加上如下选项**：

img：文件要打包到的文件夹

name：获取图片原来的名字，放在该位置

hash:8：为了防止图片名称冲突，依然使用hash，但是我们只保留8位

ext：使用图片原来的扩展名

webpack.config.js

```js
module.exports = {
  module: {
    ...
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,
              name: 'img/[name].[hash:8].[ext]'
            }
          }
        ]
      }
    ]
  }
}
```

但是，我们发现图片并没有显示出来，这是因为图片使用的路径不正确

默认情况下，webpack会将生成的路径直接返回给使用者

但是，我们整个程序是打包在dist文件夹下的，所以这里我们需要在路径下再添加一个dist/

webpack.config.js

```js
const path = require('path');	// 引入path包

module.exports = {
    entry: './src/index.js',
    output: {
        // path 需要为绝对路径。如何获取绝对路径？通过node的包找到。我们引入path这个包。
        // path: './dist',
        path: path.resolv(__dirname, 'dist'),
        filename: 'bundle.js',
        // 不过这里配置dist，以后就不需要了，因为index.html也会放到dist目录下面
        publicPath: "dist/"
    }
    ...
}
```

#### Babel处理ES6语法转ES5

> babel：[https://www.webpackjs.com/loaders/babel-loader/](https://www.webpackjs.com/loaders/babel-loader/)



### webpack集成/配置vue

在项目中，使用npm安装vue，之后通过import引入vue，这样才能用模块化的思想写vue的东西。

```shell
npm install vue --save
```



#### el和template的区别

正常运行之后，我们来考虑另外一个问题：

如果我们希望将data中的数据显示在界面中，就必须是修改index.html

如果我们后面自定义了组件，也必须修改index.html来使用组件

但是html模板在之后的开发中，我并不希望手动的来频繁修改，是否可以做到呢？

定义template属性：

在前面的Vue实例中，我们定义了el属性，用于和index.html中的#app进行绑定，让Vue实例之后可以管理它其中的内容

现在，我们可以将div元素中的{{msg}}内容删掉，只保留一个基本的id为div的元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">

    </div>
    <script src="./dist/bundle.js"></script>
</body>
</html>
```

但是如果我依然希望在其中显示{{msg}}的内容应该怎么处理呢?

我们可以再定义一个template属性，代码如下:

```js
new Vue({
    el: '#app',
    template: '<div id="app">{{msg}}</div>',
    data: {
        msg: 'hello vue'
    }
})
```

最后，关系直接理解就是：template会把el的内容替换掉。

### vue文件封装处理

实际上，以后的vue可以这样写。

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">
    </div>
</body>
</html>
```

app.js

```js
export default {
    template: '<div id="app">{{msg}}</div>',
    data() {
        return {
        	msg: 'hello vue'
    	}
    }
}
```

index.js

```js
import Vue from 'vue'
import App from './vue/app.js'

new Vue({
    el: '#app',
    template: '<App/>',
	components: {
        App
    }
})
```

然后呢，如果我们新建一个app.vue文件，会发现是这样的结构

```vue
<template>
</template>

<script>
export default {
    name: 'App',
}
</script>

<style>
</style>
```

所以我们可以把app.js的内容放到app.vue里

```vue
<template>
	<div id="app">{{msg}}</div>
</template>

<script>
export default {
    name: 'App',
    data() {
    return {
        msg: 'hello vue'
    }
}
</script>

<style>
</style>
```

但是，这样的文件并不能被正确加载处理。我们需要有人来帮我们解决。怎么解决？

用vue-loader（loader高于14版本吧好像就需要某个插件，可以按照13版本的）以及vue-template-compiler

安装这两个东西

```shell
npm install vue-loader vue-template-compiler --save-dev
```

修改webpack.config.js

```js
module.exports = {
  module: {
    ...
    rules: [
      {
        test: '/\.vue$/',
        use: ['vue-loader']
      }
    ]
  }
}

```



### 关于Plugin

**plugin是什么?**

plugin是插件的意思，通常是用于对某个现有的架构进行扩展。

webpack中的插件，就是对webpack现有功能的各种扩展，比如打包优化，文件压缩等等。

**loader和plugin区别：**

loader主要用于转换某些类型的模块，它是一个转换器。plugin是插件，它是对webpack本身的扩展，是一个扩展器。

**plugin的使用过程：**

步骤一︰通过npm安装需要使用的plugins(某些webpack已经内置的插件不需要安装)

步骤二:在webpack.config.js中的plugins中配置插件。

下面，我们就来看看可以通过哪些插件对现有的webpack打包过程进行扩容，让我们的webpack变得更加好用。

#### 版权声明—Banner

BannerPlugin是webpack自带的插件，使用它，只需修改webpack.config.js

```js
...
const webpack = require('webpack');

module: {
    ...
    plugins: [
        new webpack.BannerPlugin('最终版权归god23bin所有')
    ]
}
```

然后进行打包，最终js就有这个东西了。



#### html打包

目前，我们的index.html文件是存放在项目的根目录下的。

我们知道，在真实发布项目时，发布的是dist文件夹中的内容，但是dist文件夹中如果没有index.html文件，那么打包的js等文件也就没有意义了。

所以，我们需要将index.html文件打包到dist文件夹中，这个时候就可以使用HtmlWebpackPlugin插件

**HtmlWebpackPlugin插件可以为我们做这些事情：**

- 自动生成一个index.html文件(可以指定模板来生成)

- 将打包的js文件，自动通过script标签插入到body中

安装HtmlWebpackPlugin插件

```shell
npm install html-webpack-plugin --save-dev
```

修改webpack.config.js

```js
...
module: {
    ...
    plugins: [
        new webpack.BannerPlugin('最终版权归god23bin所有');
        new HtmlWebpackPlugin({
        	template: 'index.html'
        })
    ]
}
```

这里的template表示根据什么模板来生成index.html

另外，我们需要删除之前在output中添加的publicPath属性否则插入的script标签中的src可能会有问题

#### js压缩

在项目发布之前，我们必然需要对js等文件进行压缩处理口这里，我们就对打包的js文件进行压缩

我们使用一个第三方的插件uglifyjs-webpack-plugin，并且版本号指定1.1.1，和CLI2保持一致

```shell
npm install uglifyjs-webpack-plugin@1.1.1 --save-dev
```

修改webpack.config.js

```js
...
const UglifyJsPlugin = require('uglifyjs-webpack-plugin')

module: {
    ...
    plugins: [
        new webpack.BannerPlugin('最终版权归god23bin所有');
        new HtmlWebpackPlugin({
        	template: 'index.html'
        });
		new UglifyJsPlugin()
    ]
}
```

然后进行打包，最终js就压缩了。



### 本地服务器

webpack提供了一个可选的本地开发服务器，这个本地服务器基于node.js搭建，内部使用express框架，可以实现我们想要的让浏览器自动刷新显示我们修改后的结果。

不过它是一个单独的模块，在webpack中使用之前需要先安装它

```shell
npm install --save-dev webpack-dev-server@2.9.1
```



devserver也是作为webpack中的一个选项，选项本身可以设置如下属性∶

- contentBase：为哪一个文件夹提供本地服务，默认是根文件夹，我们这里要填写./dist

- port：端口号

- inline：页面实时刷新

- historyApiFallback：在SPA页面中，依赖HTML5的history模式

所以，来修改webpack.config.js

```js
module: {
    ...
    devServer: {
        contentBase: './dist',
        inline: true
    }
}
```

在package.json配置一个script

```json
"dev": "webpack-dev-server"
或者加上--open，会自动打开浏览器
"dev": "webpack-dev-server --open"
```



### 抽离开发配置环境和发布配置环境

有些配置你开发的时候需要用，发布的时候不需要用；有些配置你开发的时候不需要用，发布的时候需要用。

那么写在一起不太好，如何进行抽离？

写成3个配置文件

- base.config.js
- prod.config.js
- dev.config.js

需要有人来帮助，所以安装webpack-merge

```shell
npm install webpack-merge --save-dev
```

base.config.js

```js
const path = require('path');	// 引入path包

module.exports = {
    entry: './src/index.js',
    output: {
        // path 需要为绝对路径。如何获取绝对路径？通过node的包找到。我们引入path这个包。
        // path: './dist',
        // path: path.resolv(__dirname, 'dist'),
        path: path.resolv(__dirname, '../dist'), // 这里也要修改了，不然dist会在build下面生成
        filename: 'bundle.js',
        // 不过这里配置dist，以后就不需要了，因为index.html也会放到dist目录下面
        // publicPath: "dist/"
    }
    ...
}
```



prod.config.js

```js
...
const UglifyJsPlugin = require('uglifyjs-webpack-plugin')
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config.js')

module.exports = webpackMerge(baseConfig, {
    ...
    plugins: [
		new UglifyJsPlugin()
    ]
})
```

dev.config.js

```js
const webpack = require('webpack');
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config.js')

module.exports = webpackMerge(baseConfig, {
    ...
    plugins: [
        new webpack.BannerPlugin('最终版权归god23bin所有');
        new HtmlWebpackPlugin({
        	template: 'index.html'
        });
    ]
})
```

最后在package.json里添加一个script，并且修改之前dev

```json
"build": "webpack --config ./build/prod.config.js"
"dev": "webpack-dev-server --open --config ./build/dev.config.js"
```

最后就可以打包，即运行npm run build，就OK了





## 关于vue-cli

最上面一开始写vue都是写在html里面的，但是一般的项目开发，都不是这样，需要通过webpack打包工具，帮助我们构建项目目录，最后完成后，帮我们打包成一个线上可运行的代码。如果让每一个程序员都去webpack，那么这是一个不小的挑战。所以出现了脚手架工具，称为vue-cli，可以帮助我们快速构建一个标准的vue项目，同时这里面自带了各种webpack的配置，使用它不会有任何技术门槛，可以直接进入开发当中。

### 安装vue-cli

下面这条命令提示已经过时，此时安装的版本为2.9.6

```shell
npm install --global vue-cli
```

Vue官网的说明

> [https://cn.vuejs.org/v2/guide/installation.html#命令行工具-CLI](https://cn.vuejs.org/v2/guide/installation.html#%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%B7%A5%E5%85%B7-CLI)

### 后序如何升级vue-cli？

> Vue-CLI安装（包含升级）：[https://cli.vuejs.org/zh/guide/installation.html](https://cli.vuejs.org/zh/guide/installation.html)

### 如何创建vue项目？

#### CLI 2.9.6

**在2.9.6的版本下**，可以通过下面的命令来创建vue项目

创建一个通过webpack模板搭建的项目，这里项目名为todolist_demo，回车，接着选一些选项。

```shell
vue init webpack todolist_demo
```

> **在3.0.0以上的版本**，可以通过这两个命令来创建
>
> vue create 项目名称
>
> vue ui

然后进入该项目，并运行，就可以跑起来了

```shell
cd todolist_demo
npm run dev
# 显示下面信息
 DONE  Compiled successfully in 4046ms                                               

 I  Your application is running here: http://localhost:8080
```

然后看下目录结构

![目录结构](https://raw.githubusercontent.com/god23bin/pic-bed/master/img/20220516145012.png)

1. build 目录下放置的是项目的webpack配置文件，可以不动

2. config 是针对线上环境和开发环境的配置文件，也可以不动

3. node_modules 指的是项目的依赖

4. src 指的是源代码放置的目录

5. static 放置的是静态的资源

   src 中的main.js文件是整个项目的入口文件

   ![main.js](https://raw.githubusercontent.com/god23bin/pic-bed/master/img/20220516145020.png)

我们主要编写的代码就是在src下面的代码。

#### CLI 3

vue-cli 3与2版本有很大区别

vue-cli 3是基于webpack 4打造，vue-cli 2还是webapck 3

vue-cli 3的设计原则是“0配置”，移除的配置文件根目录下的，build和config等目录vue-cli 3提供了vue ui命令，提供了可视化配置，更加人性化

移除了static文件夹，新增了public文件夹，并且index.html移动到public中

```shell
vue create 项目名称

vue ui
```



#### CLI 4



### 关于.vue

vue文件（单文件组件）主要由三部分组成，分别是：

- 模板
- script逻辑代码
- 样式

```vue
<template>

</template>

<script>
export default {

}
</script>

<style>

</style>

```

main.js这个根主入口，显示的就是根组件App，所以就会把App.vue这个组件的内容显示出来，App.vue里面的模板是什么，那么它就会显示什么。



**一些写法**，以前data是一个对象，现在就不是了，而是**函数！**这个前面也提过了，是关于组件的。

> 详情看这里：[https://cn.vuejs.org/v2/api/#data](https://cn.vuejs.org/v2/api/#data)

引入TodoItem组件的写法，然后注册组件。

```vue
<script>
// 引入TodoItem组件
import TodoItem from './components/TodoItem.vue'

export default {
  // 注册组件
  components: {
    'todo-item': TodoItem
  },
  // 以前data是一个对象，现在就不是了，而是函数
  // ——————以前——————
  // data: {
  //     inputValue: "hello",
  //     list: []
  // },
  // ——————现在——————
  // data: function() {
  //   return {
  //     inputValue: ''
  //   }
  // }
  // ES6语法可以简化写法
  data () {
    return {
      inputValue: '',
      list: []
    }
  },
  // methods注意后面有冒号，是个对象
  methods: {
    handleSubmit () {
      // alert(123)
      // this.list = this.$data.list，vue底层会自己去找list
      // data里面没找到那么就会去computed里去找，类似这样
      this.list.push(this.inputValue);
      this.inputValue = '';
    },
    // 这里需要接收子组件传过来的index参数
    handleDelete(index) {
      this.list.splice(index, 1);
    }
  }
}
</script>
```

**全局样式和局部样式：可通过scoped来限制样式范围。**

```vue
<style scoped>

</style>
```



### 如何运行？

使用命令

```shell
npm run dev
# 或者
npm run start
```



### runtime-compiler和only的区别

简单总结

- 如果在之后的开发中，你依然使用template，就需要选择Runtime-Compiler
- 如果你之后的开发中，使用的是.vue文件夹开发，那么可以选择Runtime-only



## Vue全家桶？

什么是Vue全家桶？

Vue全家桶 = VueCore + Vue-Router + Vuex

## 关于router

> Vue Router起步：[https://router.vuejs.org/zh/guide/#html](https://router.vuejs.org/zh/guide/#html)
>
> [https://cn.vuejs.org/v2/guide/routing.html](https://cn.vuejs.org/v2/guide/routing.html)

### 路由的意思

说起路由你想起了什么？

- 路由，这就要扯到计算机网络了。

- 路由（routing）就是通过互联的网络把信息从源地址传输到目的地址的活动。——维基百科

  额，这是什么东西？没看懂。

- 在生活中，我们有没有听说过路由的概念呢？当然了，路由器嘛。路由器是做什么的？你有想过吗？

- 路由器提供了两种机制：路由和转送。

  - 路由是决定数据包从来源到目的地的路径
  - 转送将输入端的数据转移到合适的输出端
  - 路由中有一个非常重要的概念叫路由表
  - 路由表本质上就是一个映射表，决定了数据包的指向

### 前端渲染、后端渲染

什么是前端渲染？什么是后端渲染？

要回答这个问题，就要从以前说起了。以前开发网页应用，基本上就是用jsp，你在浏览器发送一个请求，说白了就是你输入url回车，然后发送了对应请求，请求到服务端，就是后端，然后后端就帮你处理，通过你给定的url，选择对应的jsp页面，然后给你返回html+css+js，这就是在后端处理好的，就称为后端渲染。后来，由于这种方式很麻烦，后端开发者还需要写前端，前端开发者还要会写些后端，还不好维护，所以出现了前后端分离，前端只负责前端，后端只负责后端，同时后端提供数据给前端，前端就可以通过ajax请求服务器数据，然后直接填充前端页面，这就是前端渲染

**前后端分离阶段：**

随着Ajax的出现，有了前后端分离的开发模式。

后端只提供API来返回数据，前端通过Ajax获取数据，并且可以通过JavaScript将数据渲染到页面中。这样做最大的优点就是前后端责任的清晰，后端专注于数据上，前端专注于交互和可视化上。

并且当移动端(iOS/Android)出现后，后端不需要进行任何处理，依然使用之前的一套API即可。目前很多的网站依然采用这种模式开发。

**单页面富应用阶段：**

其实SPA最主要的特点就是在前后端分离的基础上加了一层前端路由。也就是前端来维护—套路由规则。

### 前端路由的核心是什么呢？

- 改变URL，但是页面不进行整体的刷新。
- 如何实现呢？

**URL的hash**

URL的hash也就是锚点(#)，本质上是改变window.location的href属性，我们可以通过直接赋值location.hash来改变href，但是页面不发生刷新

```js
location.hash = "login"
```



**HTML5的history模式：**

**pushState()**

pushState(data对象参数，title参数，url参数)

类似栈，这是压栈，即入栈

所以还有出栈，history.back()

```js
history.pushState({}, '', 'login')
```

**replaceState()：重置，不能返回**

go()

```js
history.pushState({}, '', 'login');
history.pushState({}, '', 'home');
history.pushState({}, '', 'demo');

history.go(-1);	// 类似back，出栈一个页面
history.go(-2);	// 出栈两个页面

```



### 路由配置&配置映射关系

学习路由的时候我是用vue2.x来学习的。

- 路由配置

因为我们已经学习了webpack，后续开发中我们主要是通过工程化的方式进行开发的。所以在后面，我们直接使用vue-cli来安装路由即可，当然忘了选上的话，也可以通过npm安装router。

1. 安装vue-router

```shell
npm install vue-router --save
```

2. 手写router

创建一个router目录，然后在该目录下创建index.js

在模块化工程中使用它（因为router是一个插件，所以可以通过Vue.use(插件)来安装路由功能）

router/index.js

```js
// 导入路由对象，并且调用Vue.use(VueRouter)
import VueRouter from 'vue-router';

Vue.use(VueRouter);

// 创建路由实例，并且传入路由映射配置
const routes = [
    
]

const router = new Router({
    routes
})

export default router;
```

3. 在Vue实例中挂载创建的路由实例

main.js

```js
import Vue from 'vue';
import App from './App';
// import router from './router/index.js' 可以简写为下面的样子
import router from './router'

Vue.config.production = false;

new Vue({
    el: '#app',
    router,
    render: h => h(App)
})
```



- 配置映射关系

通过路由组合我们需要的功能组件，来形成我们的单页面应用。假设你已经写好组件了，那么我们现在就需要进行映射了。一个映射关系就是一个对象。

router/index.js（有的地方会写成router.js）

```js
...
const routes = [
  {
    path: '/',
    redirect: '/home'
  },
  {
    path: '/home',
    name: 'Home',
    component: Home
  }
]
...
```

- 通过两种标签使用路由

router-link和router-view

在router-link中，有个to属性，把路径写上去，就OK了。

App.vue

```html
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>
```

> `<router-link>`：该标签是一个vue-router中已经内置的组件，它会被渲染成一个`<a>`标签。
>
> `<router-view>`：该标签会根据当前的路径，动态渲染出不同的组件。
>
> 网页的其他内容，比如顶部的标题/导航,或者底部的一些版权信息等会和`<router-view>`处于同一个等级。
>
> 在路由切换时,切换的是`<router-view>`挂载的组件，其他内容不会发生改变。
>
> **router-link的其他属性：**
>
> - tag：可以指定`<router-link>`渲染成什么标签，比如可以被渲染成一个`<li>`标签，而不是`<a>`标签
> - replace：不会留下history记录，所以指定replace情况下，后退键返回不能返回到上一个页面中。
> - active-class：当`<router-link>`对应的路由匹配成功时，会自动给当前元素设置一个router-link-active的class，设置active-class可以修改默认的名称。
>   - 在进行高亮显示的导航菜单或者底部tabbar时，会使用到该类。
>   - 但是通常不会修改类的属性，会直接使用默认的router-link-active即可。

---

路由的定义：

这里的定义应该是vue3.x的写法

1. 在router.js里对路由进行定义。

下面这个代码是来自vue-cli自动生成的。

```js
import Home from './views/Home.vue';

export default new Router({
    routes: [
        {
            path: '/',
            name: 'home',
            component: Home,
        },
    ]
})
```

router/router.js（下面将写成index.js）

1. 在App.vue添加router-link标签，去链接我们需要的组件，比如这里的Home

---

**默认情况是用哈希的形式（带#号）显示url的，那么不想要哈希，想用history怎么搞？**

router/index.js

```js
...

const router = new Router({
    routes,
    // 这里加上mode
    mode: history
})

...
```



### 通过代码实现路由跳转

$router的使用，每个组件都有$router属性

App.vue

```vue
<template>
  <div id="app">
    <div id="nav">
      <!-- <router-link to="/home">Home</router-link> -->
      <!-- <router-link to="/about">About</router-link> -->
	  <button @click="homeClick"></button>
      <button @click="aboutClick"></button>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default ({
  name: 'App',
  methods: {
    homeClick() {
      // 通过代码的方式修改路由
      this.$router.push('/home')
      // this.$router.repalce('/home')
    },
    aboutClick() {
      this.$router.push('/about')
    }
  }
})
</script>
```



### 动态路由

在某些情况下，一个页面的path路径可能是不确定的。

比如我们进入用户界面时，希望是如下的路径/user/aaaa或/user/bbbb

除了有前面的/user之外，后面还跟上了用户的ID

这种path和Component的匹配关系，我们称之为动态路由（也是路由传递数据的一种方式）。

router/index.js

```js
...
const routes = [
    {
        path: '/home',
        components: Home,
    },
    {
        path: 'user/:userid',	// 就可以这样写成动态路由
        components: User,
    }
]
...
```

然后在vue中拼接，这里就App.vue

```vue
<template>
	<router-link :to="'/user/' + userid">个人中心</router-link>
</template>
...
<script>
export default {
    name: 'App',
    data() {
        return {
            userid: 'god23bin', // 从后端获取的id
        }
    }
}
</script>
```

**如何在另一个组件中获取传过来的参数？**

比如这里App.vue通过路由传了给userid过去给User.vue，那么在User.vue中如何获取传过来的userid参数？

**结论：通过$route.params获取**，$route对象当前是哪个过来的就是哪个。

User.vue

```vue
<template>
	<div>
        {{userid}}
    </div>
</template>

<script>
export default {
    name: 'User',
    computed: {
        userId() {
            // 通过$route.params获取
            return this.$route.params.userid;
        }
    }
}
</script>
```



### 路由的懒加载

当我们写的东西越来越多，比如js，越来越大，不可能一次性完全加载完，那么就需要加载一部分，你用到什么就加载什么。

> 官方给出了解释：
>
> 当打包构建应用时，Javascript包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。
>
> 官方在说什么呢？
>
> - 首先,我们知道路由中通常会定义很多不同的页面。
> - 这个页面最后被打包在哪里呢？一般情况下，是放在一个js文件中。但是，页面这么多放在一个js文件中，必然会造成这个页面非常的大。
> - 如果我们一次性从服务器请求下来这个页面，可能需要花费一定的时间，甚至用户的电脑上还出现了短暂空白的情况。
> - 如何避免这种情况呢？使用路由懒加载就可以了。路由懒加载做了什么？
> - 路由懒加载的主要作用就是将路由对应的组件抵包成一个个的js代码块。
> - 只有在这个路由被访问到的时候，才加载对应的组件。

**路由懒加载的3种方式**

方式一：结合Vue的异步组件和Webpack的代码分析。

```js
const Home = resolve => { require.ensure([ ' ../components/Home.vue']，() =>{ resolve(require( ' ../ components/Home.vue' )) })};
```

方式二：AMD写法。

```js
const About = resolve => require([ ' ../ components/About.vue'],resolve);
```

**方式三：在ES6中，我们可以有更加简单的写法来组织Vue异步组件和Webpack的代码分割。**

```js
const Home = () => import( ' ../ components/Home.vue ' )
```



### 路由嵌套/嵌套路由

使用children这个对象属性，然后里面写子路由。

```js
...
const routes = [
    {
        path: '/home',
        components: Home,
        children: [
            {
                path: '',
                redirect: 'news'
            },
            {
                path: 'news',
                components: () => import('../components/HomeNews.vue')
            },
            {
                path: 'messages',
                components: () => import('../components/HomeMessages.vue')
            }
        ]
    },
    {
        path: 'user/:userid',	// 就可以这样写成动态路由
        components: User,
    }
]
...
```

后面如何使用这些呢？还是一样，在这个组件中用router-link和router-view这两个标签。



### 传参

传递参数主要有两种类型：params和query，params在上面动态路由就有用了。

params的类型

- 配置路由格式：/router/:id
- 传递的方式：在path后面跟上对应的值
- 传递后形成的路径：/router/123，/router/abc

query的类型：

- 配置路由格式：/router，也就是普通配置
- 传递的方式：对象中使用query的key作为传递方式。
- 传递后形成的路径：/router?id=123，/router?id=abc

App.vue

```vue
<router-link :to="{path: '/profile', query: {name: 'god23bin', age: '18'}}"></router-link>
```

Profile.vue

```vue
// $route.query取值
$route.query.name;
$route.query.age;
```



通过代码传参的写法

App.vue

```vue
<template>
  <div id="app">
    <div id="nav">
      <!-- <router-link to="/home">Home</router-link> -->
      <!-- <router-link to="/about">About</router-link> -->
	  <button @click="homeClick"></button>
      <button @click="aboutClick"></button>
      <button @click="userClick"></button>
      <button @click="profileClick"></button>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default ({
  name: 'App',
  methods: {
    homeClick() {
      // 通过代码的方式修改路由
      this.$router.push('/home')
      // this.$router.repalce('/home')
    },
    aboutClick() {
      this.$router.push('/about')
    },
    userClick() {
      this.$router.push('/user/' + id)
    },
    profileClick() {
      this.$router.push({
        path: '/profile',
        query: {
  		  name: 'kobe',
          age: 41,
          height: 1.98
        }
      })
    }
  },
  computed: {
    userId() {
      return userid;	// 从服务器获取的数据
    }
  }
})
</script>
```

### 全局导航守卫

router/index.js

```js
const routes = [
    {
        path: '/home',
        components: 'Home',
        meta: {
            title: '主页',
        },
    },
    {
        ...
    }
];

// 前置守卫/钩子（钩子(hook)：回调的意思）
router.beforeEach(function(to, from, next) => {
  // 从from跳到to
  // document.title = to.meta.title;
  document.title = to.matched[0].meta.title;
  next();	// 必须调用，不然不知道下一步是什么，调用该方法后才能进入下一个钩子
});
```

如果是后置钩子，也就是afterEach，则不需要主动调用next()函数。

上面我们使用的导航守卫，被称之为全局守卫。

还有路由独享的守卫、组件内的守卫。

> 路由独享的守卫：[https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#路由独享守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E8%B7%AF%E7%94%B1%E7%8B%AC%E4%BA%AB%E7%9A%84%E5%AE%88%E5%8D%AB)
>
> 组件内的守卫：[https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#组件内的守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E7%BB%84%E4%BB%B6%E5%86%85%E7%9A%84%E5%AE%88%E5%8D%AB)

 ### keep-alive

keep-alive是vue内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

router-view也是一个组件，如果直接被包在keep-alive里面，所有路径匹配到的视图组件都会被缓存。

用于实现什么？比如你想实现一开始是新闻，然后点了消息，显示消息，切换到其他页面再切换回来还是显示消息，而不是重新显示新闻。

结合组件内的守卫

```vue
<template>
  <div>
    <router-link to="/about"></router-link>
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      path: '/home/news'
    }
  },
  // activated()、deactivated()两个函数是与keep-alive有关的，如果你不用keep-alive，这两个函数是不会被调用的。
  activated() {
    this.$router.push(this.path);
  },
  beforeRouteLeave(to , from, next) {
    console.log(this.$route.path);
    this.path = this.$route.path;
    next();
  }
}
</script>
```

keep-alive的属性

它们有两个非常重要的属性：

- include - 字符串或正则表达，只有匹配的组件会被缓存
- exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存（注意逗号后面不要加空格）



## Tabbar

1. 如果在下方有一个单独的TabBar组件，你如何封装？

   自定义TabBar组件，在APP中使用

   让TabBar出于底部，并且设置相关的样式

2. TabBar中显示的内容由外界决定

   定义插槽

   flex布局平分TabBar

3. 自定义TabBarItem，可以传入图片和文字

   定义TabBarItem，并且定义两个插槽：图片、文字。

   给两个插槽外层包装div，用于设置样式。

   填充插槽，实现底部TabBar的效果

## ES6 Promise

### 为什么这里要学Promise？

因为后面的vuex是依赖于这个promise的，所以为后面学vuex做铺垫。

### 什么是Promise?

ES6中一个非常重要和好用的特性就是Promise，但是初次接触Promise会一脸懵逼，这TM是什么东西？

看看官方或者一些文章对它的介绍和用法，也是一头雾水。

Promise到底是做什么的呢？

Promise是异步编程的一种解决方案。那什么时候我们会来处理异步事件呢？

一种很常见的场景应该就是网络请求了。我们封装一个网络请求的函数，因为不能立即拿到结果，所以不能像简单的3+4=7一样将结果返回。所以往往我们会传入另外一个函数，在数据请求成功时，将数据通过传入的函数回调出去。

如果只是一个简单的网络请求，那么这种方案不会给我们带来很大的麻烦。但是，当网络请求非常复杂时，就会出现回调地狱。

### Demo-定时器异步

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Promise</title>
</head>
<body>
    
    <script>
        // 异步操作
        // 使用setTimeout()
        // setTimeout(() => {
        //     console.log("Hello World!");
        // }, 1000);

        // setTimeout(() => {
        //     console.log("Hello World!");
        //     console.log("Hello World!");
        //     console.log("Hello World!");
        //     console.log("Hello World!");
        //     console.log("Hello World!");
        //     console.log("Hello World!");
        //     setTimeout(() => {
        //         console.log("Hello ES6!");
        //         console.log("Hello ES6!");
        //         console.log("Hello ES6!");
        //         console.log("Hello ES6!");
        //         console.log("Hello ES6!");
        //         console.log("Hello ES6!");
        //         setTimeout(() => {
        //             console.log("Hello Vue");
        //             console.log("Hello Vue");
        //             console.log("Hello Vue");
        //             console.log("Hello Vue");
        //             console.log("Hello Vue");
        //             console.log("Hello Vue");
        //         }, 1000);
        //     }, 1000);
        // }, 1000);

        // 使用promise来执行
        // new Promise(参数)
        // 参数： 函数(resolve, reject), resolve, reject 本身又是函数
        // 链式编程
        new Promise((resolve, reject) => {
            // 第一次网络请求的代码
            setTimeout(() => {
                resolve();
            }, 1000)
        }).then(() => {
            // 第一次拿到结果的代码，然后进行处理
            console.log("Hello World!");
            console.log("Hello World!");
            console.log("Hello World!");
            console.log("Hello World!");
            console.log("Hello World!");
            console.log("Hello World!");

            return new Promise((resolve, reject) => {
                // 第二次网络请求的代码
                setTimeout(() => {
                    resolve();
                }, 1000)
            });
        }).then(() => {
            // 第二次拿到结果的代码，然后进行处理
            console.log("Hello ES6!");
            console.log("Hello ES6!");
            console.log("Hello ES6!");
            console.log("Hello ES6!");
            console.log("Hello ES6!");
            console.log("Hello ES6!");

            return new Promise((resolve, reject) => {
                // 第三次网络请求的代码
                setTimeout(() => {
                    resolve();
                }, 1000)
            });
        }).then(() => {
            // 第三次拿到结果的代码，然后进行处理
            console.log("Hello Vue");
            console.log("Hello Vue");
            console.log("Hello Vue");
            console.log("Hello Vue");
            console.log("Hello Vue");
            console.log("Hello Vue");
        })

        // 什么时候使用Promise？
        // 一般情况下是有异步操作时，然后你需要对这个异步操作进行封装，那么就需要用到Promise

        // new Promise((resolve, reject) => {
        //     setTimeout(() => {
        //         // 成功调用resolve
        //         // 这里假设data = 'Hello World'，那么then那里也就有data
        //         resolve(data);
        //         // 失败调用reject
        //         // 这里假设err = 'error msg'，那么catch那里就有err
        //         reject(err);
        //     });
        // }).then((data) => {
        //     // 进行处理...
        // }).catch((err) => {
        //     // 进行处理...
        // })

    </script>
</body>
</html>
```







## Vuex

### Vuex是什么？

> vuex ：[https://vuex.vuejs.org/zh/](https://vuex.vuejs.org/zh/)

> Vuex是一个专为Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。Vuex也集成到Vue的官方调试工具 devtools extension，提供了诸如零配置的 time-travel调试、状态快照导入导出等高级调试功能。

### 状态管理到底是什么?

> 状态管理模式、集中式存储管理这些名词听起来就非常高大上，让人捉摸不透。其实，你可以简单的将其看成把需要多个组件共享的变量全部存储在一个对象里面。然后，将这个对象放在顶层的Vue实例中，让其他组件可以使用。那么，多个组件是不是就可以共享这个对象中的所有变量属性了呢？

### 什么时候用它呢？

有什么状态时需要我们在多个组件间共享的呢？

如果你做过大型开放，你一定遇到过多个状态，在多个界面间的共享问题。比如用户的登录状态、用卢名称、头像、地理位置信息等等。比如商品的收藏、购物车中的物品等等。这些状态信息，我们都可以放在统一的地方，对它进行保存和管理，而且它们还是响应式的。

### 多个界面间的状态管理

Vue已经帮我们做好了单个界面的状态管理，但是如果是多个界面呢？

多个试图都依赖同一个状态（一个状态改了，多个界面需要进行更新）。不同界面的Actions都想修改同一个状态( Home.vue需要修改，Profile.vue也需要修改这个状态)。也就是说对于某些状态(状态1/状态2/状态3)来说只属于我们某一个试图，但是也有一些状态(状态a/状态b/状态c)属于多个试图共同想要维护的。状态1/状态2/状态3你放在自己的房间中，你自己管理自己用，没问题。但是状态a/状态b/状态c我们希望交给一个大管家来统一帮助我们管理！！！没错，Vuex就是为我们提供这个大管家的工具。

全局单例模式（大管家）

我们现在要做的就是将共享的状态抽取出来，交给我们的大管家，统一进行管理。之后，你们每个试图，按照我规定好的规定，进行访问和修改等操作。这就是Vuex背后的基本思想。

### 安装

```shell
npm install vuex --save
```

基本写法：

store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})

```



### 使用

store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count: 1000
  },
  mutations: {
    // 传入state
    increment(state) {
      state.count++;
    },
    decrement(state) {
      state.count--;
    }
  },
  actions: {
  },
  modules: {
  }
})

```

>我们来对使用步骤，做一个小结：
>
>1. 提取出一个公共的store对象，用于保存在多个组件中共享的状态
>
>2. 将store对象放置在new Vue对象中，这样可以保证在所有的组件中都可以使用到
>
>3. 在其他组件中使用store对象中保存的状态即可
>
>通过this.$store.state.属性的方式来访问状态
>
>通过this.$store.commit('mutation中方法')来修改状态注意事项：
>
>- 我们通过提交mutation的方式，而非直接改变store.state.count。
>- 这是因为Vuex可以更明确的追踪状态的变化，所以不要直接改变store.state.count的值。

### 核心概念

> Vuex 核心概念：[https://vuex.vuejs.org/zh/guide/state.html](https://vuex.vuejs.org/zh/guide/state.html)

#### State

**单一状态树**

> Vuex提出使用单—状态树,什么是单—状态树呢？
>
> 英文名称是Single Source of Truth，也可以翻译成单一数据源。但是，它是什么呢？我们来看一个生活中的例子。
>
> OK，我用一个生活中的例子做一个简单的类比。
>
> 我们知道，在国内我们有很多的信息需要被记录，比如上学时的个人档案，工作后的社保记录，公积金记录，结婚后的婚姻信息，以及其他相关的户口、医疗、文凭、房产记录等等（还有很多信息）。
>
> 这些信息被分散在很多地方进行管理，有一天你需要办某个业务时(比如入户某个城市)，你会发现你需要到各个对应的工作地点去打印、盖章各种资料信息，最后到一个地方提交证明你的信息无误。
>
> 这种保存信息的方案，不仅仅低效，而且不方便管理，以及日后的维护也是一个庞大的工作（需要大量的各个部门的人力来维护，当然国家目前已经在完善我们的这个系统了）。
>
> 这个和我们在应用开发中比较类似：
>
> 如果你的状态信息是保存到多个Store对象中的，那么之后的管理和维护等等都会变得特别困难。所以Vuex也使用了单—状态树来管理应用层级的全部状态。
>
> 单一状态树能够让我们最直接的方式找到某个状态的片段，而且在之后的维护和调试过程中，也可以非常方便的管理和维护。

#### Getters

我们拿数据，在store里面这个数据是需要经过变化的，那么我们就可以通过getters来处理需要变化的数据。定义一些变化后，让其他人直接拿我们的getters就OK了。

#### Mutations

Vuex的store状态的更新唯一方式：提交mutations

mutations主要包括两部分：

- 字符串的事件类型（type）
- 一个回调函数（handler）,该回调函数的第一个参数就是state。

mutations的定义方式：

store/index.js

```js
  mutations: {
    // 传入state
    increment(state) {
      state.count++;
    },
    decrement(state) {
      state.count--;
    }
  },
```

通过mutation更新：

```js
this.$store.commit('increment');
this.$store.commit('decrement');
```

关于传参

store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count: 1000
  },
  mutations: {
    // 传入state
	// ...
    // 传参count，这个有个专业名词：payload
    addCount(state, count) {
      state.count += count;
    }
  },
  actions: {
  },
  modules: {
  }
})

```



App.vue

```vue
<script>
import HelloVuex from './components/HelloVuex.vue'

export default {
  components: {
    HelloVuex
  },
  data() {
    return {
      count: 1
    }
  },
  methods: {
    // ...
    addClick(count) {
      this.$store.commit('addCount', count);
    }
  }
}
</script>
```



Vuex的store中的state是响应式的，当state中的数据发生改变时，Vue组件会自动更新。

这就要求我们必须遵守一些Vuex对应的规则：

- 提前在store中初始化好所需的属性。
- 当给state中的对象添加新属性时，使用下面的方式：
  - 方式一：使用Vue.set(obj, 'newProp', 123)
  - 方式二：用新对象给旧对象重新赋值

不过，目前高版本的vuex已经能够直接做到添加新属性也能成为响应式的了。

store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count: 1000,
    stus: [
      {
        name: 'kobe',
        age: 41,
        height: '1.98'
      },
      {
        name: 'james',
        age: 37,
        height: '2.03'
      },
      {
        name: 'god23bin',
        age: 22,
        height: '1.91'
      }
    ]
  },
  mutations: {
    // ...
    addStu(state, stu) {
      state.stus.push(stu);
    }
  },
  actions: {
  },
  modules: {
  }
})

```



通常情况下, Vuex要求我们mutations中的方法必须是同步方法。

主要的原因是当我们使用devtools时，可以devtools可以帮助我们捕捉mutations的快照。但是如果是异步操作，那么devtools将不能很好的追踪这个操作什么时候会被完成。

#### Actions

我们强调，不要再mutations中进行异步操作。但是某些情况，我们确实希望在Vuex中进行一些异步操作，比如网络请求，必然是异步的。这个时候怎么处理呢？

actions类似于mutations，但是是用来代替mutations进行异步操作的。

store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count: 1000,
    stus: [
      {
        name: 'kobe',
        age: 41,
        height: '1.98'
      },
      {
        name: 'james',
        age: 37,
        height: '2.03'
      },
      {
        name: 'god23bin',
        age: 22,
        height: '1.91'
      }
    ]
  },
  mutations: {
    // 默认参数 state
    // ...
    updateInfo(state) {
      state.stus[0].name = 'jordan';
      // 错误写法：在mutations中写异步操作，不要这样做
      // setTimeout(() => {
      //   state.stus[0].name = 'jordan';
      // }, 1000);
    }
  },
  actions: {
    // 默认参数 context 上下文 这里可以先理解为store对象
    // 这里进行异步操作
    aUpdateInfo(context) {
      setTimeout(() => {
        context.commit('updateInfo');
      }, 1000)
    }
  },
  modules: {
  }
})

```

App.vue

```vue
<template>
  <div id="app">
    <h2>这里是App</h2>
    <!-- <div>{{count}}</div> -->
    <div>{{$store.state.count}}</div>
    <button @click="handleClickUp">+</button>
    <button @click="handleClickDown">-</button>
    <button @click="addClick(5)">+5</button>
    <button @click="addClick(10)">+10</button>

    <h2>对象信息</h2>
    <div>{{$store.state.stus}}</div>
    <button @click="addStuClick">添加一个对象</button>
    <button @click="updateInfoClick">异步更新信息</button>
    <hello-vuex :count="count" />
  </div>
</template>

<script>
import HelloVuex from './components/HelloVuex.vue'

export default {
  components: {
    HelloVuex
  },
  data() {
    return {
      count: 1
    }
  },
  methods: {
    // ...
    updateInfoClick() {
      // this.$store.commit('updateInfo');    // 异步操作不要直接写在mutations中
      this.$store.dispatch('aUpdateInfo');
    }
  }
}
</script>

```





有两点需要知道的

1. 异步操作是在actions里完成的
2. actions里的函数可以返回一个Promise给外面的dispatch，然后dispatch.then这样的方式来写。



#### Modules

Module是模块的意思，为什么在Vuex中我们要使用模块呢？Vue使用单—状态树，那么也意味着很多状态都会交给Vuex来管理。当应用变得非常复杂时，store对象就有可能变得相当臃肿。

为了解决这个问题，Vuex允许我们将store分割成模块（modules），而每个模块拥有自己的state、mutations、actions、getters等

```js
const moduleA = {
  state: {...},
  mutations: {...},
  actions: {...}
}
              
const moduleB = {
  state: {...},
  mutations: {...},
  actions: {...}
}
              
const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})
```



## Axios

> Axios：[https://www.axios-http.cn/docs/intro](https://www.axios-http.cn/docs/intro)

先了解下几个概念

**跨域、JSONP**

在前端开发中，我们一种常见的网络请求方式就是JSONP。使用JSONP最主要的原因往往是为了解决跨域访问的问题。JSONP的原理是什么呢？

JSONP的核心在于通过`<script>`标签的src来帮助我们请求数据。原因是我们的项目部署在domain1.com服务器上时，是不能直接访问domain2.com服务器上的资料的。这个时候我们利用`<script>`标签的src帮助我们去服务器请求到数据，将数据当做一个javascript的函数来执行，并且执行的过程中传入我们需要的json。

所以，封装JSONP的核心就在于我们监听window上的JSONP进行回调时的名称。

**为什么选择Axios？**

- 在浏览器中发送XMLHttpRequests

- 请求在node.js中发送http请求

- 支持 Promise API

- 拦截请求和响底
- 转换请求和响应数据
- and so on

支持多种请求方式：

- axios(config)
- axios.request(config)
- axios.get(url[, config])
- axios.delete(url[, config])
- axios.head(url[, config])
- axios.post(url[, data[, config]])
- axios.put(url[, data[, config]])
- axios.patch(url[, data[, config]])



### 基本操作

```js
// 基本操作
axios({
  url: 'http://cloud-music.pl-fe.cn/search?keywords=%E6%B5%B7%E9%98%94%E5%A4%A9%E7%A9%BA',
  method: 'get'
}).then((res) => {
  console.log(res);
})

// axios.get()
// axios.post()

axios({
  url: 'http://cloud-music.pl-fe.cn/search',
  params: {
    keywords: '%E6%B5%B7%E9%98%94%E5%A4%A9%E7%A9%BA'
  }
}).then((res) => {
  console.log(res);
})
```



### 并发请求

```js
// 并发操作
axios.all([axios({
  url: 'http://cloud-music.pl-fe.cn/comment/music?id=186016&limit=1'
}), axios({
  url: 'http://cloud-music.pl-fe.cn/dj/program?rid=336355127'
})])
.then( res => {
  console.log(res);
})
```



### 全局配置

在上面的示例中，我们的BaseURL是固定的。事实上，在开发中可能很多参数都是固定的。

这个时候我们可以进行一些抽取，也可以利用axiox的全局配置

```js
axios.defaults.baseURL = 'http://cloud-music.pl-fe.cn';

axios({
  url: '/search',
  params: {
    keywords: '%E6%B5%B7%E9%98%94%E5%A4%A9%E7%A9%BA'
  }
}).then((res) => {
  console.log(res);
})
```



### axios实例、封装axios

一般不推荐直接用全局，而是封装起来，单独用实例。

封装到一个文件：request.js

network/request.js

```js
import axios from 'axios';

export function request(config) {
    
}
```

创建实例

```js
const instance = axios.create({
    baseURL: 'http://cloud-music.pl-fe.cn',
    timeout: 5000
})
```

#### 第一种封装方式

network/request.js

```js
import axios from 'axios';

// success, failure：为传入的函数，作为回调函数
export function request(config, success, failure) {
    // 1. 创建axios实例
    const instance = axios.create({
        baseURL: 'http://cloud-music.pl-fe.cn',
        timeout: 2000
    })
	// 2. 真正发送网络请求
    instance(config)
    .then(res => {
        // console.log(res);
        success(res);
    })
    .catch(err => {
        // console.log(err);
        failure(err);
    })
}
```

如何用呢？

```js
import {request} from './network/request'

request({
  url: '/search',
  params: {
    keywords: '%E6%B5%B7%E9%98%94%E5%A4%A9%E7%A9%BA'
  }
}, res => {
  console.log(res);
}, err => {
  console.log(err);
})
```



#### 第二种封装方式

network/request.js

```js
import axios from 'axios';

export function request(config) {
    // 1. 创建axios实例
    const instance = axios.create({
        baseURL: 'http://cloud-music.pl-fe.cn',
        timeout: 2000
    })
	// 2. 真正发送网络请求
    instance(config.baseConfig)
    .then(res => {
        // console.log(res);
        config.success(res);
    })
    .catch(err => {
        // console.log(err);
        config.failure(err);
    })
}
```

如何使用？

```js
import {request} from './network/request'

request({
  baseConfig: {
    url: '/search',
    params: {
      keywords: '%E6%B5%B7%E9%98%94%E5%A4%A9%E7%A9%BA'
    }
  },
  success: (res) => {
    console.log(res);
  },
  failure: (err) => {
    console.log(err);
  }
})
```



#### 第三种封装方式

network/request.js

```js
import axios from 'axios';

export function request(config) {
    return new Promise((resolve, reject) => {
        // 1. 创建axios实例
        const instance = axios.create({
            baseURL: 'http://cloud-music.pl-fe.cn',
            timeout: 2000
        })
        // 2. 真正发送网络请求
        instance(config)
        .then(res => {
            resolve(res);
        })
        .catch(err => {
            reject(err);
        })
    })
}
```

如何使用？

```js
import {request} from './network/request'

request({
  url: '/search',
  params: {
    keywords: '%E6%B5%B7%E9%98%94%E5%A4%A9%E7%A9%BA'
  }
}).then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
})
```

#### 最终方案

network/request.js

```js
// 最终方案： instance 函数返回的就是一个 Promise 对象
export function request(config) {
    // 1. 创建axios实例
    const instance = axios.create({
        baseURL: 'http://cloud-music.pl-fe.cn',
        timeout: 2000
    })
    // 2. 真正发送网络请求
    return instance(config)
}
```

如何使用？

```js
import {request} from './network/request'
// 最终方案
request({
  url: '/search',
  params: {
    keywords: '%E6%B5%B7%E9%98%94%E5%A4%A9%E7%A9%BA'
  }
}).then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
})
```



### 拦截器

```js
// 使用拦截器
export function request(config) {
    // 1. 创建axios实例
    const instance = axios.create({
        baseURL: 'http://cloud-music.pl-fe.cn',
        timeout: 2000
    })
    // 2. axios拦截器
    // 1). 请求拦截
    // 请求成功或者失败的拦截；config是传过来的参数，你随便命名
    // 一般很少进入到请求失败的情况中
    instance.interceptors.request.use(config => {
        // 进行拦截
        console.log(config);
        // 比如说config中的信息不符合服务器的要求
        // 比如header，你可以在创建实例的时候加，也可以拦截的时候加。
        // 比如请求的时候显示加载的图标
        // 比如某些网络请求，比如登录的时候（Token）
        // 拦截处理完后返回config
        return config;
    }, err => {
        console.log(err);
    });
    // 2). 响应拦截
    instance.interceptors.response.use(res => {
        console.log(res);
    }, err => {
        console.log(err);
    });
    // 3. 真正发送网络请求
    return instance(config)
}
```





## JS需要知道的3个高阶函数

需求1：取出所有小于100的数

需求2：将小于100的数进行乘2

需求3：将这些数进行相加汇总

```js
const nums = [10, 20, 30, 110, 150, 50, 70];	// 原始数据

let newNums = [];	// 存放小于100的数
// 普通循环
// 取出所有小于100的数
for (let i = 0; i <= nums.length; i++) {
    if (nums[i] < 100)
        newNums.push(nums[i]);
}

for (let i in nums) {
    if (nums[i] < 100)
        newNums.push(nums[i]);
}

for (let item of nums) {
    if (item < 100)
        newNums.push(item);
}
// 用普通做法需要写很多
// 我们可以用高阶函数的方式来写，这样比较少。

// 高阶函数
// 1. filter()函数
// 取出所有小于100的数
let newNumsByFilter = nums.filter(function(item){
    return item < 100;
});
// newNumsByFilter内容：10, 20, 30, 50, 70

// 2. map()函数
// 将小于100的数进行乘2
let newNumsByMap = newNumsByFilter.map(function(item){
    return item * 2;
});
// newNumsByMap内容：20, 40, 60, 100, 140

// 3. reduce()函数：可以用来汇总
// 将这些数进行相加汇总
// 参数1：回调函数；参数2：初始值
let newNumsByReduce = newNumsByMap.reduce(function(prevalue, item) {
    return prevalue + item;
}, 0);
// newNums内容： 10, 20, 30, 50, 70
// 第一次执行： prevalue是0, item是10
// 第二次执行： prevalue是10, item是20
// 第三次执行： prevalue是30, item是30
// 第四次执行： prevalue是60, item是50
// 第五次执行： prevalue是110, item是70
// 返回 110 + 70 = 180
```

- filter()函数

  它会遍历数组，把符合条件的元素放入新数组中。就上述代码，它会把nums数组中符合条件的元素放入新数组newNumsByFilter中。

- map()函数

  同理。

- reduce()函数

  参数1：回调函数；参数2：初始值

综上，对于这3个需求可以这样写

```js
const nums = [10, 20, 30, 110, 150, 50, 70];	// 原始数据
let total = nums.filter(function(item) {
    return item < 100;
}).map(function(item) {
    return item * 2;
}).reduce(function(prevalue, item) {
    return prevalue + item;
}, 0);
```



## 风格指南

> 风格指南：[https://cn.vuejs.org/v2/style-guide/](https://cn.vuejs.org/v2/style-guide/)

