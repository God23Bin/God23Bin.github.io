---
title: Node.js-入门
author: god23bin
categories: 前端
tags: 
- Node.js
- 前端
- 入门
abbrlink: a4bf6c0d
description: Node.js 入门学习记录
date: 2021-03-10 16:44:30
---



## 入门教程

可以看这个文档

> [Node.js 简介](http://nodejs.cn/learn/introduction-to-nodejs)

## 相关术语

### Nodejs/nodejs

---

**什么是Nodejs？**

经常看到Node.js这个词，但是这是什么东西呢？

> 简单的说 Node.js 就是运行在服务端的 JavaScript。
>
> Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。
>
> Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

说了这么多，可能还是不知道nodejs是什么。

我们之前写js代码，你写了js代码之后，需要放到html里，然后html需要浏览器来运行。也就是说js是只能在浏览器里运行的，人家开发js的目的也是让浏览器来解析，搞一些动画啊事件啊这样。之后有一个人，他就想能不能把js用在服务端做更多的事情，也就是服务器这边，但是服务器又没有不是浏览器可以解析js代码，所以一开始肯定遇到了很大的困难，不过最后还是被他用C++开发出一个东西，可以支持js运行，这个东西就是nodejs。比如你现在电脑上安装了nodejs，那么你就可以在你的终端运行js代码了，不需要浏览器也可以运行了。



### npm

**什么是npm？**

---

同时，也会看到npm这个词，看起来像命令一样。

> npm是随同Node.js一起安装的包管理工具，能解决Node.js代码部署上的很多问题，常见的使用场景有以下几种：
>
> - 允许用户从npm服务器下载别人编写的第三方包到本地使用。
> - 允许用户从npm服务器下载并安装别人编写的命令行程序到本地使用。
> - 允许用户将自己编写的包或命令行程序上传到npm服务器供别人使用。

这个看起来有点熟悉，有点Linux中的yum命令，是吧。那也确实，我们就可以通过npm命令来从npm服务器下载安装我们需要的东西。



## 如何安装？

> [Node.js 安装配置-菜鸟教程](https://www.runoob.com/nodejs/nodejs-install-setup.html)

## 第一个应用-体验Nodejs当服务器的感觉

使用 Node.js 时，我们不仅仅在实现一个应用，同时还实现了整个 HTTP 服务器。

实际上，我们的 Web 应用以及对应的 Web 服务器基本上是一样的，Web 应用 = 对应的 Web 服务器。

在我们创建 Node.js 第一个 "Hello, World!" 应用前，让我们先了解下 Node.js 应用是由哪几部分组成的：

1. **引入 required 模块：**我们可以使用 **require** 指令来载入 Node.js 模块。
2. **创建服务器：**服务器可以监听客户端的请求，类似于 Apache 、Nginx 等 HTTP 服务器。
3. **接收请求与响应请求** 服务器很容易创建，客户端可以使用浏览器或终端发送 HTTP 请求，服务器接收请求后返回响应数据。

**如何写服务端代码呢？实际上，还是一样写js，写完后通过npm来运行。**

在某个目录下来试试，我们写一个server.js，如下：

```js
// -----------------------引入 required 模块-------------------------------
var http = require('http');			// 第一行请求（require）Node.js 自带的 http 模块，并且把它赋值给 http 变量。

// -----------------------创建服务器&&接收请求与响应请求---------------------
// 接下来我们调用 http 模块提供的函数： createServer 。
// 这个函数会返回 一个对象，这个对象有一个叫做 listen 的方法，这个方法有一个数值参数， 指定这个 HTTP 服务器监听的端口号。
http.createServer(function(request, response) {
	// 发送HTTP头部
	// HTTP状态值：200：OK
	// 内容类型：text/plain
	response.writeHead(200, {'Content-Type': 'text/plain'});
	
	// 发送响应数据"Hello World"
	response.end('Hello World\n');
}).listen(8888);

// 终端打印如下信息
console.log('服务运行在 http://127.0.0.1:8888');
```

这样就写完一个可以工作的HTTP服务器了，Node.js真强大。

然后在该目录下运行cmd

使用 **node** 命令执行以上的代码：

```shell
$ node server.js
```

然后打开浏览器访问http://127.0.0.1:8888，就可以看到打印Hello World的网页了。

## npm的使用

---

查看版本

```shell
$ npm -v
```

升级npm

```shell
$ npm install npm -g		# windos下
$ sudo npm install npm -g	# linux下
```

使用淘宝镜像

```shell
$ npm install -g cnpm --registry=https://registry.npmmirror.com
```

这样就可以使用 cnpm 命令来安装模块了：

```shell
$ cnpm install [name]
```



### 使用npm安装想要的东西（模块）

---

安装你需要的模块

```shell
$ npm install <Module Name>

# 比如
$ npm install express
```



### 本地安装和全局安装

---

本地安装，就是你比如在某个项目下/或者说某个目录下，你只想安装在这个目录下，那么就使用本地安装，只会安装在该目录下。

- 它会将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。

- 可以通过 require() 来引入本地安装的包。

全局安装，就是你一安装，全部项目都能用

- 它会将安装包放在 /usr/local 下（linux）或者你 node 的安装目录（windows）。
- 可以直接在命令行里使用。

```shell
$ npm install express      # 本地安装
$ npm install express -g   # 全局安装
```



### 查看已经安装的模块

---

可以使用以下命令来查看所有全局安装的模块：

```shell
$ npm list -g
```



对于本地的，那么可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看：

```shell
$ npm ls
```



### 关于package.json

---

每个模块都有自己的 package.json。package.json 位于模块的目录下，用于定义包的属性。

属性说明：

- **name** - 包名。
- **version** - 包的版本号。
- **description** - 包的描述。
- **homepage** - 包的官网 url 。
- **author** - 包的作者姓名。
- **contributors** - 包的其他贡献者姓名。
- **dependencies** - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。
- **repository** - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
- **main** - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
- **keywords** - 关键字



比如之前安装过的hexo模块，那么看看它的 package.json 

```json
{
  "_args": [
    [
      "hexo@5.4.0",
      "D:\\SoftWare\\HexoB"
    ]
  ],
  "_from": "hexo@5.4.0",
  "_id": "hexo@5.4.0",
  "_inBundle": false,
  "_integrity": "sha1-d9V/ocKfOrBZZP5OvZxE4j31Gzc=",
  "_location": "/hexo",
  "_phantomChildren": {
    "abbrev": "1.1.1",
    "bluebird": "3.7.2",
    "chalk": "4.1.0",
    "command-exists": "1.2.9",
    "hexo-fs": "3.1.0",
    "hexo-log": "2.0.0",
    "hexo-util": "2.4.0",
    "minimist": "1.2.5",
    "resolve": "1.20.0",
    "tildify": "2.0.0"
  },
  "_requested": {
    "type": "version",
    "registry": true,
    "raw": "hexo@5.4.0",
    "name": "hexo",
    "escapedName": "hexo",
    "rawSpec": "5.4.0",
    "saveSpec": null,
    "fetchSpec": "5.4.0"
  },
  "_requiredBy": [
    "/"
  ],
  "_resolved": "https://registry.npm.taobao.org/hexo/download/hexo-5.4.0.tgz",
  "_spec": "5.4.0",
  "_where": "D:\\SoftWare\\HexoB",
  "author": {
    "name": "Tommy Chen",
    "email": "tommy351@gmail.com",
    "url": "https://zespia.tw"
  },
  "bin": {
    "hexo": "bin/hexo"
  },
  "bugs": {
    "url": "https://github.com/hexojs/hexo/issues"
  },
  "dependencies": {
    "abbrev": "^1.1.1",
    "archy": "^1.0.0",
    "bluebird": "^3.5.2",
    "chalk": "^4.0.0",
    "hexo-cli": "^4.0.0",
    "hexo-front-matter": "^2.0.0",
    "hexo-fs": "^3.1.0",
    "hexo-i18n": "^1.0.0",
    "hexo-log": "^2.0.0",
    "hexo-util": "^2.4.0",
    "js-yaml": "^4.0.0",
    "micromatch": "^4.0.2",
    "moment": "^2.22.2",
    "moment-timezone": "^0.5.21",
    "nunjucks": "^3.2.1",
    "pretty-hrtime": "^1.0.3",
    "resolve": "^1.8.1",
    "strip-ansi": "^6.0.0",
    "text-table": "^0.2.0",
    "tildify": "^2.0.0",
    "titlecase": "^1.1.2",
    "warehouse": "^4.0.0"
  },
  "description": "A fast, simple & powerful blog framework, powered by Node.js.",
  "devDependencies": {
    "0x": "^4.10.2",
    "@easyops/git-exec-and-restage": "^1.0.4",
    "chai": "^4.2.0",
    "cheerio": "0.22.0",
    "decache": "^4.5.1",
    "eslint": "^7.0.0",
    "eslint-config-hexo": "^4.1.0",
    "hexo-renderer-marked": "^3.0.0",
    "husky": "^4.2.5",
    "lint-staged": "^10.2.0",
    "mocha": "^8.0.1",
    "nyc": "^15.0.0",
    "sinon": "^9.0.2"
  },
  "directories": {
    "lib": "./lib",
    "bin": "./bin"
  },
  "engines": {
    "node": ">=10.13.0"
  },
  "files": [
    "lib/",
    "bin/"
  ],
  "funding": {
    "type": "opencollective",
    "url": "https://opencollective.com/hexo"
  },
  "homepage": "https://hexo.io/",
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "keywords": [
    "website",
    "blog",
    "cms",
    "framework",
    "hexo"
  ],
  "license": "MIT",
  "main": "lib/hexo",
  "maintainers": [
    {
      "name": "Abner Chou",
      "email": "hi@abnerchou.me",
      "url": "https://abnerchou.me"
    }
  ],
  "name": "hexo",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/hexojs/hexo.git"
  },
  "scripts": {
    "eslint": "eslint .",
    "lint-staged": "lint-staged",
    "test": "mocha test/index.js",
    "test-cov": "nyc --reporter=lcovonly npm run test -- --no-parallel"
  },
  "version": "5.4.0"
}

```



### 更新模块

---

```shell
$ npm update <Module Name>
# 比如
$ npm update express
```



### 搜索模块

---

```shell
$ npm search <Module Name>
# 比如
$ npm search express
```



### 卸载模块

---

```shell
$ npm uninstall <Module Name>
# 比如
$ npm uninstall express
```



### 如何制作自己的模块，提供给别人用？

这就涉及到创建模块了，创建模块就需要创建package.json，package.json就需要使用到下面的命令

```shell
$ npm init
```

package.json 的信息，你需要根据你自己的情况输入。在最后输入 "yes" 后会生成 package.json 文件。

接下来我们可以使用以下命令在 npm 资源库中注册用户（使用邮箱注册）：

```shell
$ npm adduser
Username: mcmohd
Password:
Email: (this IS public) mcmohd@gmail.com
```

然后我们就用以下命令来发布自己的模块：

```shell
$ npm publish
```

这样其他人就可以使用 npm 来安装我们的模块了。



### 常用命令

npm提供了很多命令，例如`install`和`publish`，使用`npm help`可查看所有命令。

- 使用`npm help `可查看某条命令的详细帮助，例如`npm help install`。
- 在`package.json`所在目录下使用`npm install . -g`可先在本地安装当前命令行程序，可用于发布前的本地测试。
- 使用`npm update `可以把当前目录下`node_modules`子目录里边的对应模块更新至最新版本。
- 使用`npm update  -g`可以把全局安装的对应命令行程序更新至最新版。
- 使用`npm cache clear`可以清空NPM本地缓存，用于对付使用相同版本号发布新版本代码的人。
- 使用`npm unpublish @`可以撤销发布自己发布过的某个版本代码。





