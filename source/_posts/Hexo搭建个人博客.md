---
title: Hexo搭建个人博客
categories: 折腾
tags: 
- Hexo
- 教程
abbrlink: d87f8e00
date: 2021-04-02 17:01:57
---

## 前言

本篇文章，将写下如何使用 Hexo 搭建个人博客，以及对于我使用的主题 `yun` 的记录。

## 什么是Hexo？

官网：[https://hexo.io/zh-cn/](https://hexo.io/zh-cn/)

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 准备

安装 `Hexo` 之前需要准备这些东西。

- Node.js (Node.js 版本需不低于 10.13，建议使用 Node.js 12.0 及以上版本)
- Git（有了 Git 还需要注册 Github，之后可以将博客托管到 Github 上，当然也可以使用 Gitee ）

## 安装Hexo

可以全局安装或者局部安装，我这里就使用全局安装了。

```shell
$ npm install -g hexo-cli
```

完事。

## Github上创建远程仓库

登录上Github，新建仓库：

点击右上角的 `+` -> `New repository`

![](D:%5CBin%5CDesktop%5CKnowledge%5CJava%5CHexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.assets%5Cimage-20220313221943878.png)

仓库名称格式 `你的用户名.github.io`，用户名是英文，大小写无所谓，但建议统一小写。（当然如果你的用户名有大小写，想修改成全小写，那么可以看我另一篇文章，毕竟我之前踩过这个坑）



![](D:%5CBin%5CDesktop%5CKnowledge%5CJava%5CHexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.assets%5Cimage-20220313222206617.png)

然后点击 `Create repository` 完成仓库的创建。

## 开搞

### 初始搭建

此时，在你的电脑上你喜欢的位置，创建一个用于存放网站代码的文件夹。

我创建的文件夹为 Hexo，所在位置：`D:\SoftWare\Hexo`

`cd` 进入该文件夹目录。（或者通过 `cmd`，或者通过右键 `Git Bash Here` ）

输入下面的命令，执行过程需要一点时间。

```shell
hexo init 你的用户名.github.io
```

> 正是因为我们之前安装了 `hexo-cli` 这一个包，所以我们可以在终端中使用 `hexo` 这一命令。
>
> `init` 初始化博客的模版文件。后面跟的是你要新建的文件夹，最好和你此前新建的仓库名一致。

接下来。

```shell
# 进入你的博客文件夹
cd 你的名字.github.io
# 默认安装所有 `package.json` 文件中提到的包
npm install
# 你也可以缩写成 hexo s
hexo server
```

通过 `hexo server`  这个命令，就能够在本地开启 Hexo 服务器了，在浏览器访问 `localhost:4000` 就能够看到本地的网站了。按 `Ctrl + C` 中断服务器的运行。

以上总的来说主要就是3个步骤就能够完成基本的博客网站模板，即

```shell
hexo init <folder>
cd <folder>
npm install
```

### 了解目录结构

[https://hexo.io/zh-cn/docs/setup](https://hexo.io/zh-cn/docs/setup)

```shell
.
├── _config.yml	# 网站的 配置 信息，可以在此配置大部分的参数。
├── package.json # 应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装
├── scaffolds # 模版 文件夹。当你新建文章时，Hexo 会根据 scaffold 来建立文件。
├── source # 资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。
|   ├── _drafts
|   └── _posts
└── themes # 主题 文件夹。Hexo 会根据主题来生成静态页面。
```

### 下载Hexo主题

我使用的主题：[hexo-theme-yun](https://github.com/YunYouJun/hexo-theme-yun)

`cd` 进入 `xxx.github.io` ，当然你可以用其他方式进入终端，可以使用 Git Bash，也可以在 VS Code 中使用终端。

通过 `npm` 安装主题

```shell
npm i hexo-theme-yun
```

### 进行配置主题

在 `_config.yml` 中找到 `theme` 这个字段（大概在第100行的位置）

```yml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape
```

将其后的 `landscape` 修改为 `yun`

```yml
theme: yun
```

`yun` 这个主题使用了 pug 和 stylus，而 Hexo 自带的一般是 ejs 与 stylus，所以你可能还需要输入以下命令安装渲染器。

```shell
npm install hexo-render-pug hexo-renderer-stylus
# 如果出问题，可以换 yarn 安装试试。
```

> pug 是一种模板引擎，可以渲染为 HTML 字符串。类似的还有 ejs，swig 等，语法和设计理念有所不同。
> stylus 是一种 CSS 预处理器，可以渲染为 CSS。类似的还有 scss，less，同样只是语法和设计理念有所差异。

这时再像此前一样使用 `hexo server` 重新启动服务器，你就可以看到新的主题风格的页面了。

## 配置记录

> 「配置指南」：[https://yun.yunyoujun.cn/guide/](https://yun.yunyoujun.cn/guide/)

在 Hexo 工作目录（即 xxx.github.io）下新建 `_config.yun.yml` 文件。（与你的 Hexo 配置 `_config.yml` 在同一目录）



### 文章属性

[详情看这里](https://yun.yunyoujun.cn/guide/page.html#%E6%96%87%E7%AB%A0)

#### 头部字段

- `title`: 设置页面标题（可以对默认标题进行覆盖）
- `icon`: 页面标题前的图标

额外的头部字段

- `author`: 设置作者则会显示
- `email`: 自动根据邮箱获取 [Gravatar (opens new window)](https://en.gravatar.com/site/implement/images/)头像
- `toc`: 是否显示目录（文章 `post` 默认显示，页面 `post` 默认不显示）
- `readmore`: 将会首页卡片摘要末尾强制显示一个 `阅读更多` 按钮

```md
---
title: xxx
author: xxx
email: xxx
readmore: true
---
```

#### type

```md
---
title: xxx
type: bilibili
url: https://www.bilibili.com/video/av8153395/
---
```

在文章标题前将会出现 bilibili 的图标，点击标题会跳转至对应的链接。

默认支持以下类型（哔哩哔哩、豆瓣、GitHub、网易云音乐、推特、微信公众号、微博、语雀、知乎、Notion、外链）：

```
types:
  link:
    color: blue
    icon: icon-external-link-line
  bilibili:
    color: "#FF8EB3"
    icon: icon-bilibili-line
  douban:
    color: "#007722"
    icon: icon-douban-line
  github:
    color: black
    icon: icon-github-line
  netease-cloud-music:
    color: "#C10D0C"
    icon: icon-netease-cloud-music-line
  notion:
    color: black
    icon: icon-notion
  twitter:
    color: "#1da1f2"
    icon: icon-twitter-line
  wechat:
    color: "#1AAD19"
    icon: icon-wechat-2-line
  weibo:
    color: "#E6162D"
    icon: icon-weibo-line
  yuque:
    color: "#25b864"
    icon: icon-yuque
  zhihu:
    color: "#0084FF"
    icon: icon-zhihu-line
```



#### reward

可以在某篇文章的首部单独设置是否开启打赏。

```
reward: true
```

#### sticky

置顶，需要 `hexo-generator-index`

```  shell
npm install hexo-generator-index
```

数值越高，优先级越高。

```md
---
title: xxx
sticky: 100
---
```

#### aplayer

播放器，需要 `hexo-tag-aplayer`

```shell
npm install hexo-tag-aplayer
```

```yaml
---
title: xxx
aplayer: true
---
```

插入某首网易云音乐的歌

```md
{% meting "497572729" "netease" "song" "theme:#C20C0C" %}
```

## 开始部署

### 生成静态文件

使用以下命令先来生成站点的静态文件。

```shell
# 如果进行多次生成，为了避免受错误缓存影响，最好使用 hexo clean 先清除一遍。
hexo generate
# 缩写为 hexo g
```

此时你的文件夹目录下会出现 `public` 这个文件夹，里面存放的就是你站点的静态文件。

### 与远程仓库关联

接下来我们将本地的仓库与之前在 GitHub 上创建的仓库建立关联。

```shell
git init # 初始化 Git 仓库
```

在将它部署到 GitHub Pages 上之前，我们最好先建立一个分支。

> 部署后，GitHub Pages 将默认使用你的 master 分支作为静态文件部署。
> 所以我们最好新建一个 hexo 分支（命名无所谓）用来存储 Hexo 地源代码，master 分支则用来存储部署后的静态文件。

这里你先随便添加并提交一个文件。

```shell
git add test.txt
git commit -m "test"
```

然后创建分支。

```sh
git branch hexo
```

这时便成功建立了一个 hexo 分支。（此后的工作都将在 hexo 分支下进行）

可通过`git branch` 查看当前分支，使用`git checkout 分支名 `切换分支

```shell
git remote add origin https://github.com/god23bin/god23bin.github.io
```

现在push可能会出现问题

```shell
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
fatal: Authentication failed for 'https://github.com/god23bin/god23bin.github.io/'

```

如何解决？看这里：[https://blog.csdn.net/yjw123456/article/details/119696726](https://blog.csdn.net/yjw123456/article/details/119696726)



### 部署

同样，需要安装 `hexo-deployer-git`

```shell
npm install hexo-deployer-git
```

```yml
deploy:
  type: git
  repo: https://github.com/god23bin/god23bin.github.io
  branch: master # 默认使用 master 分支
  message: Update Hexo Static Content # 你可以自定义此次部署更新的说明
```

执行部署

```shell
hexo deploy
```

等待完成后，访问 `https://你的名字.github.io` 就能看到你的线上网站了

### 部署遇到的问题

执行完 `hexo deploy` 后访问`https://god23bin.github.io`出现 404

可能原因

1. Github 仓库名有误，需要修改成 xxx.github.io，但是这不是我出现的问题。
2. `_config.yml` 中的 `deploy` 配置有误，这也不是我出现的问题。
3. 分支问题，这就是我出现问题的地方

分支问题：

背景：我创建了一个分支 `hexo`，所以总共的分支有 `master` 和 `hexo` 这两个分支

 `master` 用于存放生成的静态网站代码，`hexo` 用于存放源代码

~~不知被我何种操作，Github上默认的分支为 `hexo`，而不是 `master`，所以 Github Pages 找不到 index.html~~

~~所以需要修改 `master` 作为主分支~~

~~修改完后重新`hexo clean`和`hexo deploy`，发现还是 404。~~

之后**直接找到 Github 中的 Github Pages 设置，进行 `source` 修改，选中分支为 `master`，保存**，重新部署访问，就OK了。

（Github上默认分支我改回来了，改为`hexo`为默认分支，这样后面自动部署的时候，配置文件即可保存在`hexo`分支中。）

### 减少操作

#### 更新的脚本

在xxx.github.io目录下写一个脚本，将我们使用git的动作写下来，之后更新的话，只需要在终端执行 `sh update.sh` 即可。

update.sh

```sh
# 如果没有消息后缀，默认提交信息为 `:pencil: update content`
info=$1
if ["$info" = ""];
then info=":pencil: update content"
fi
git add -A
git commit -m "$info"
git push origin hexo
```



#### 自动部署

详细可见：[初探无后端静态博客自动化部署方案](https://blog.ichr.me/post/automated-deployment-of-serverless-static-blog/#%E6%9C%9F%E6%9C%9B)

使用 Github Actions 进行自动部署

1. 生成公钥私钥

```shell
ssh-keygen -t rsa
```

```shell
ssh-keygen -t rsa
Generating public/private rsa key pair.
# 输入你要保存的公钥私钥的位置，我这里保存在 /d/SoftWare/.ssh/，名字为 id_ras_ghpage
Enter file in which to save the key (/c/Users/Bin/.ssh/id_rsa): /d/SoftWare/.ssh/id_ras_ghpage
# 直接回车
Enter passphrase (empty for no passphrase):
# 直接回车
Enter same passphrase again:
# 私钥位置
Your identification has been saved in /d/SoftWare/.ssh/id_ras_ghpage
# 公钥位置
Your public key has been saved in /d/SoftWare/.ssh/id_ras_ghpage.pub
The key fingerprint is:
SHA256:6rsGhW7WiEjQYC75qyeyxDjXiQvcILzN4DN3ZvkyRaY Bin@DESKTOP-C0A0FVG
The key's randomart image is:
+---[RSA 3072]----+
|.+               |
|+..              |
|+.   .           |
|oo  . .o         |
|o=.o ++ S        |
|B O+*Eoo         |
|+O+Bo*o          |
|+==.++o          |
|++.  .*+         |
+----[SHA256]-----+
```

将**公钥**添加到 GitHub 上的`「Settings -> SSH and GPG keys」`；

将**私钥**添加到你博客源码仓库的`「Settings -> Secret」`，起一个名字，例如我让他叫 `HEXO_DEPLOY_KEY` 。

2. 点击 GitHub 仓库控制栏中的 「`Actions -> New workflow-> Set up a workflow yourself」`，并将下面的代码**替换掉**原来的代码。

```yml
name: Hexo Deploy Automatically

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [14.x]

    steps:
      - name: Checkout
        uses: actions/checkout@v2
        
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}
      
      - name: Generate
        run: |
          npm i && npx hexo g
      
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: master
          publish_dir: ./public
          force_orphan: true
```

`publish_branch: master` ：就是部署到`master`这个分支

3. 这样就完成了配置。后面使用 Git 推送便可直接自动部署。

如果推送的时候有冲突

```shell
git push origin hexo
To https://github.com/god23bin/god23bin.github.io.git
 ! [rejected]        hexo -> hexo (fetch first)
error: failed to push some refs to 'https://github.com/god23bin/god23bin.github.io.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```

那么先拉取下来，再推送一次

```shell
git pull origin hexo
git push origin hexo
```

不过，有了更新脚本后，我们只需执行更新脚本就OK了。

所以，整个流程是这样的：

源代码推送到 `hexo` 分支的同时，Github Actions 会自动部署我们的博客到 `master` 分支。

