# docsify 文档网站搭建教程

docsify 文档环境配置搭建教程。

-----

## 目录

<!-- vim-markdown-toc GFM -->

* [1. 安装 Node.js 和 npm](#1. 安装 Node.js 和 npm)
* [2. docsify 介绍](#2. docsify 介绍)
* [3. 引入 docsify 方式](3.  引入 docsify 方式)
  * [3.1 手动创建`index.html`并引入docsify文件](#3.1 手动创建`index.html`并引入docsify文件)
  * [3.2 使用`docsify-cli`来开发](# 使用`docsify-cli`来开发)
* [4. 关于每个页面和URL路径说明](#4. 关于每个页面和URL路径说明)
* [5. 侧边栏设置](#5. 侧边栏设置)
  * [5.1 定制侧边栏](# 5.1定制侧边栏)
  * [5.2 关于侧边栏`_sidebar.md`文件的说明](#5.2 关于侧边栏`_sidebar.md`文件的说明)
  * [5.3 显示页面目录(当前页面的标题)](#5.3 显示页面目录(当前页面的标题))
    * [5.3.1 页面的标题不在侧边栏目录显示](#5.3.1 页面的标题不在侧边栏目录显示)
* [6. 导航栏配置](6. 导航栏配置)
  * [6.1 在`index.html`中定义导航栏](#6.1 在`index.html`中定义导航栏)
  * [6.2 通过配置文件定义导航栏](#6.2 通过配置文件定义导航栏)
  * [6.3 导航栏嵌套](#6.3 导航栏嵌套)
* [7. 设置封面](#7. 设置封面)
  * [7.1 自定义封面背景](#7.1 自定义封面背景)
  * [7.2 把封面作为封面](#7.2 把封面作为封面)
  * [7.3 多个封面](#7.3 多个封面)
* [8. 网站部署到GitHub Pages](#8. 网站部署到GitHub Pages)
  * [8.1 使用 docsify 的例子](#8.1 使用 docsify 的例子)
* [9. 一些插件](#9. 一些插件)
  * [9.1 搜索插件](#9.1 搜索插件)
  * [9.2 评论插件 Gitalk](#9.2 评论插件 Gitalk)
  * [9.3 样式插件](#9.3 样式插件)
* [参考资料](# 参考资料)

<!-- vim-markdown-toc -->

## 1. 安装 Node.js 和 npm

###  安装 Node.js

由于Node.js平台是在后端运行JavaScript代码，所以，必须首先在本机安装Node环境。

目前Node.js的最新版本是12.16.3 LTS。首先，从[Node.js官网](https://nodejs.org/)下载对应平台的安装程序。

在Windows上安装时务必选择全部组件，包括勾选`Add to Path`。

安装完成后，在Windows环境下，请打开命令提示符，然后输入`node -v`，如果安装正常，你应该看到`v7.6.0`这样的输出：

```cmd
C:\Users\yuanl>npm -v
6.14.4
```



### npm

在正式开始Node.js学习之前，我们先认识一下npm。

npm是什么东东？npm其实是Node.js的包管理工具（package manager）。

为啥我们需要一个包管理工具呢？因为我们在Node.js上开发时，会用到很多别人写的JavaScript代码。如果我们要使用别人写的某个包，每次都根据名称搜索一下官方网站，下载代码，解压，再使用，非常繁琐。于是一个集中管理的工具应运而生：大家都把自己开发的模块打包后放到npm官网上，如果要使用，直接通过npm安装就可以直接用，不用管代码存在哪，应该从哪下载。

更重要的是，如果我们要使用模块A，而模块A又依赖于模块B，模块B又依赖于模块X和模块Y，npm可以根据依赖关系，把所有依赖的包都下载下来并管理起来。否则，靠我们自己手动管理，肯定既麻烦又容易出错。

讲了这么多，npm究竟在哪？

其实npm已经在Node.js安装的时候顺带装好了。我们在命令提示符或者终端输入`npm -v`，应该看到类似的输出：

```cmd
C:\Users\yuanl>npm

Usage: npm <command>

where <command> is one of:
    access, adduser, audit, bin, bugs, c, cache, ci, cit,
    clean-install, clean-install-test, completion, config,
    create, ddp, dedupe, deprecate, dist-tag, docs, doctor,
    edit, explore, fund, get, help, help-search, hook, i, init,
    install, install-ci-test, install-test, it, link, list, ln,
    login, logout, ls, org, outdated, owner, pack, ping, prefix,
    profile, prune, publish, rb, rebuild, repo, restart, root,
    run, run-script, s, se, search, set, shrinkwrap, star,
    stars, start, stop, t, team, test, token, tst, un,
    uninstall, unpublish, unstar, up, update, v, version, view,
    whoami

npm <command> -h  quick help on <command>
npm -l            display full usage info
npm help <term>   search for help on <term>
npm help npm      involved overview

Specify configs in the ini-formatted file:
    C:\Users\yuanl\.npmrc
or on the command line via: npm <command> --key value
Config info can be viewed via: npm help config

npm@6.14.4 C:\Program Files\nodejs\node_modules\npm
```



## 2. docsify 介绍

docsify 是一个动态生成文档网站的工具。不同于 GitBook、Hexo 的地方是它不会生成将 .md 转成 .html 文件，所有转换工作都是在运行时进行。

这将非常实用，如果只是需要快速的搭建一个小型的文档网站，或者不想因为生成的一堆 .html 文件“污染” commit 记录，只需要创建一个 index.html 就可以开始写文档而且直接部署在 GitHub Pages。

## 3. 引入docsify方式

### 3.1 手动创建`index.html`并引入docsify文件

docsify使用方式很简单，只需要在项目中创建一个`index.html`文件，内容能够如下：

```html
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta charset="UTF-8">
  <link rel="stylesheet" href="//unpkg.com/docsify/themes/vue.css">
</head>
<body>
  <div id="app"></div>
  <script>
    window.$docsify = {
      //...
    }
  </script>
  <script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
</body>
</html>
```

然后在项目中创建一个`README.md`文件：

```markdown
## 我是首页

这是我的首页介绍
```

如果你的系统安装了Python 的话，可以使用Python来启动一个服务：

```shell
python -m SimpleHTTPServer 3000

Serving HTTP on 0.0.0.0 port 3000 ...
127.0.0.1 - - [03/Jan/2019 13:45:27] "GET / HTTP/1.1" 200 -
```

然后在浏览器中输入`http://localhost:3000/`查看浏览效果。

如果没有Python，还是可以使用`http-server`启动服务，
可在终端输入`npm install http-server -g`来安装`http-server`。

```powershell
http-server 

Starting up http-server, serving ./
Available on:
  http://127.0.0.1:8080
  http://172.24.70.142:8080
Hit CTRL-C to stop the server
```

然后在浏览器中输入`http://127.0.0.1:8080`查看浏览效果。

![docsify](https://itx-man.github.io/img/source/Snipaste_2020-05-21_09-57-33.png)

>***注意：\***
>1.在此模式下编辑文件保存后，需要手动刷新浏览器才能看见修改的效果，下面介绍的`docsify-cli`可实现自动查看效果。
>2.强烈建议把`index.html`文件中的`docsify.min.js`和`vue.css`文件复制到本地项目，然后使用如下方式引入：
>
>```html
><link rel="stylesheet" href="./vue.css">
><script src="./docsify.min.js"></script>
>```

### 3.2 使用`docsify-cli`来开发

docsify-cli 工具，可以方便创建及本地预览文档网站。

#### 3.2.1 安装

docsify需要本地先安装`node`, 如果没有安装node，可在node官网选择对应操作系统下载安装：https://nodejs.org/zh-cn/

终端输入`npm i docsify-cli -g`进行全局安装：

```shell
yuanl@DESKTOP-5OU69QB MINGW64 /c/Yuanlele/docsify
$ npm i docsify-cli -g
npm WARN deprecated chokidar@2.1.8: Chokidar 2 will break on node v14+. Upgrade to chokidar 3 with 15x less dependencies.
npm WARN deprecated fsevents@1.2.13: fsevents 1 will break on node v14+ and could be using insecure binaries. Upgrade to fsevents 2.
npm WARN deprecated resolve-url@0.2.1: https://github.com/lydell/resolve-url#deprecated
npm WARN deprecated urix@0.1.0: Please see https://github.com/lydell/urix#deprecated
C:\Users\yuanl\AppData\Roaming\npm\docsify -> C:\Users\yuanl\AppData\Roaming\npm\node_modules\docsify-cli\bin\docsify

> docsify@4.11.3 postinstall C:\Users\yuanl\AppData\Roaming\npm\node_modules\docsify-cli\node_modules\docsify
> opencollective-postinstall

Thank you for using docsify!
If you rely on this package, please consider supporting our open collective:
> https://opencollective.com/docsify/donate

npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@^1.2.7 (node_modules\docsify-cli\node_modules\chokidar\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.13: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

+ docsify-cli@4.4.0
added 316 packages from 170 contributors in 174.984s
```

安装结束后使用docsify -v查看是否安装成功：

```shell
docsify -v

docsify-cli version:
  4.4.0
```



#### 3.2.2 初始化一个项目

首先需要创建一个项目目录：

```powershell
mkdir docsify
```

进入项目目录后，使用`docsify init ./`来初始化一个项目：

```powershell
cd docsify

docsify init ./

Initialization succeeded! Please run docsify serve ./
```

```powershell
tree -a

.
├── .nojekyll
├── README.md
└── index.html
```

初始化成功后,`docsify`目录会生成如下几个文件：

> 1. `index.html`入口文件
> 2. `README.md`会做为主页内容渲染
> 3. `.nojekyll`用于阻止 GitHub Pages 会忽略掉下划线开头的文件、
>
> `.nojekyll`文件很重要，如果网站部署到`GitHub Pages`时，一定要注意这个文件。

直接编辑 ./README.md 就能更新网站内容，当然也可以添加其他`.md`文件。



#### 3.2.3 启动本地服务

终端输入`docsify serve ./`来启动服务：

```powershell
docsify serve ./

Serving /Users/dragon/tmp/docsify now.
Listening at http://localhost:3000
```

然后浏览器打开`http://localhost:3000`就能看见效果。

![docsify](https://itx-man.github.io/img/source/Snipaste_2020-05-22_21-30-08.png)

当修改文件保存后， `docsify serve ./`服务会自动实时更新。



## 4. 关于每个页面和URL路径说明

如果需要创建多个页面，或者需要多级路由的网站，在`docsify`里也能很容易的实现。例如创建一个`guide.md`文件，那么对应的路由就是`/#/guide`。
如果你的目录结构如下：

```powershell
-| ./
  -| README.md
  -| guide.md
  -| zh-cn/
    -| README.md
    -| guide.md
```

那么对应的访问页面将是:

```powershell
./README.md        => http://domain.com
./guide.md         => http://domain.com/guide
./zh-cn/README.md  => http://domain.com/zh-cn/
./zh-cn/guide.md   => http://domain.com/zh-cn/guide
```



## 5. 侧边栏设置

默认情况下，侧边栏会根据当前文档的标题生成目录。

![docsify](https://itx-man.github.io/img/source/Snipaste_2020-05-22_21-57-47.png)

### 5.1 定制侧边栏

首先需要在`index.html`文件中的`window.$docsify`添加`loadSidebar: true,`选项：

```powershell
<script>
  window.$docsify = {
    loadSidebar: true
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

接着在项目根目录创建`_sidebar.md`文件，内容格式如下：

```markdown
* [home1](home1)
* [home2](home2)
* [bar](bar/)
* [bar/a](bar/a)
```

![docsify](https://itx-man.github.io/img/source/Snipaste_2020-05-22_22-05-59.png)

***注：***配置了`loadSidebar`后就不会生成默认的侧边栏了。

### 5.2 关于侧边栏`_sidebar.md`文件的说明

- 如果只在根目录有一个`_sidebar.md`文件，那么所有页面都将使用这个一个配置，也就是所有页面的侧边栏都一样。
- 如果一个子目录中有`_sidebar.md`文件，那么这个子目录下的所有页面将使用这个文件的侧边栏。
- `_sidebar.md`的加载逻辑是从每层目录下获取文件，如果当前目录不存在该文件则回退到上一级目录。例如当前路径为`/zh-cn/more-pages`则从`/zh-cn/_sidebar.md`获取文件，如果不存在则从`/_sidebar.md`获取。

如果子目录有`_sidebar.md`,但你就想使用根目录的`_sidebar.md`，
可在`index.html`文件中的`window.$docsify`添加`alias`字段：

```html
<script>
  window.$docsify = {
    loadSidebar: true,
    alias: {
      '/.*/_sidebar.md': '/_sidebar.md'
    }
  }
</script>
```

配置`alias`字段后，所有页面都会显示项目根目录`_sidebar.md`文件的配置作为侧边栏，子目录的`_sidebar.md`文件会失效。



### 5.3 显示页面目录(当前页面的标题)

定制的侧边栏仅显示了页面的链接。
还可以设置在侧边栏显示当前页面的目录(标题)。
需要在`index.html`文件中的`window.$docsify`添加`subMaxLevel`字段来设置：

```html
<script>
  window.$docsify = {
    loadSidebar: true,
    subMaxLevel: 3
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

![docsify](https://itx-man.github.io/img/source/Snipaste_2020-05-22_22-12-55.png)

subMaxLevel说明：

> subMaxLevel类型是number(数字)，表示显示的目录层级
> **注意：**如果md文件中的第一个标题是一级标题，那么不会显示在侧边栏，如上图所示

| 值   | 说明                                           |
| ---- | ---------------------------------------------- |
| 0    | 默认值，表示不显示目录                         |
| 1    | 显示一级标题(`h1`)                             |
| 2    | 显示一、二级标题(`h1` ~ `h2`)                  |
| 3    | 显示一、二、三级标题(`h1` ~ `h3`)              |
| n    | n是数字，显示一、二、....n 级标题(`h1` ~ `hn`) |

在md文件中标题的写法：

```markdown
# 这是一级标题，对应HTML中<h1>标签

## 这是二级标题，对应HTML中<h2>标签

### 这是三级标题，对应HTML中<h3>标签

#### 这是四级标题，对应HTML中<h4>标签
```

#### 5.3.1 页面的标题不在侧边栏目录显示

***注意：*** 如果md文件的第一个标题是一级标题，那么默认已经忽略了。

当设置了 subMaxLevel 时，默认情况下每个标题都会自动添加到目录中。如果你想忽略特定的标题，可以给它添加 {docsify-ignore} ：

```markdown
# Getting Started

## Header {docsify-ignore}

该标题不会出现在侧边栏的目录中。
```

要忽略特定页面上的所有标题，你可以在页面的第一个标题上使用 {docsify-ignore-all} :

```markdown
# Getting Started {docsify-ignore-all}

## Header

该页面所有标题都不会出现在侧边栏的目录中。
```

在使用时， {docsify-ignore} 和 {docsify-ignore-all} 都不会在页面上显示。



## 6. 导航栏配置

docsify默认是没有导航栏的，可以通过配置来显示导航栏。

### 5.1 在`index.html`中定义导航栏

如果导航的链接少，则可以直接在`index.html`文件直接定义导航栏，要注意链接要以`#/`开头：

```html
<body>
  <nav>
    <a href="#/">项目</a>
    <a href="#/home1">home1</a>
    <a href="#/bar/a">bar/a</a>
  </nav>
</body>
```

![docsify](https://itx-man.github.io/img/source/Snipaste_2020-05-22_22-18-41.png)

### 5.2 通过配置文件定义导航栏

首先需要在`index.html`文件中的`window.$docsify`添加`loadNavbar: true,`选项：

```php+HTML
<script>
  window.$docsify = {
    loadNavbar: true
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

接着在项目根目录创建`_navbar.md`文件，内容格式如下：

```markdown
* [home1](home1)
* [home2](home2)
* [bar](bar/)
* [bar/a](bar/a)
```

![docsify](https://itx-man.github.io/img/source/Snipaste_2020-05-22_22-18-41.png)

>***注意：***
>1.如果使用配置文件来设置导航栏，那么在`index.html`中定义的导航栏只有在定制的首页才会生效，其他页面会被覆盖。
>2.如果只在根目录有一个`_navbar.md`文件，那么所有页面都将使用这个一个配置，也就是所有页面的导航栏都一样。
>3.如果一个子目录中有`_navbar.md`文件，那么这个子目录下的所有页面将使用这个文件的导航栏。
>4.`_navbar.md`的加载逻辑是从每层目录下获取文件，如果当前目录不存在该文件则回退到上一级目录。例如当前路径为`/zh-cn/more-pages`则从`/zh-cn/_navbar.md`获取文件，如果不存在则从`/_navbar.md`获取。

### 5.3 导航栏嵌套

如果导航内容过多，可以写成嵌套的列表，会被渲染成下拉列表的形式：

```markdown
* 根目录
  * [home1](home1)
  * [home2](home2)
  * [guide](guide)

* bar目录
  * [bar](bar/)
  * [a文件](bar/a)
  * [b文件](bar/b)

* foo目录
  * [one](foo/one)
```

![docsify](https://itx-man.github.io/img/source/Snipaste_2020-05-22_22-23-26.png)



## 6. 设置封面

docsify默认是没有封面的，默认有个首页`./README.md`。
通过设置`coverpage`参数，可以开启渲染封面的功能。

首先需要在`index.html`文件中的`window.$docsify`添加`coverpage: true`选项：

```php+HTML
<script>
  window.$docsify = {
    coverpage: true
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

接着在项目根目录创建`_coverpage.md`文件，内容格式如下：

```markdown
![logo](_media/icon.svg)
# 我的文档网站
## 个人文档网站
> 一个神奇的文档网站生成巩固

* Simple and lightweight (~12kb gzipped)
* Multiple themes
* Not build static html files

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#quick-start)
[Get Started](#quick-start)
```

![docsify](https://itx-man.github.io/img/source/Snipaste_2020-05-22_22-24-52.png)

> ***注：***一份文档只会在根目录下加载封面，其他页面或者二级目录下都不会加载。

#### 6.1 自定义封面背景

目前的背景是随机生成的渐变色，每次刷新都会显示不同的颜色。
docsify封面支持自定义背景色或者背景图，在`_coverpage.md`文档末尾添加：

```markdown
<!-- 背景图片 -->
![](_media/bg.png)

<!-- 背景色 -->
![color](#2f4253)
```

![docsify](https://itx-man.github.io/img/source/Snipaste_2020-05-22_22-26-43.png)

>***注意：***
>1.自定义背景配置一定要在`_coverpage.md`文档末尾。
>2.背景图片和背景色只能有一个生效.
>3.背景色一定要是`#2f4253`这种格式的。

#### 6.2 把封面作为首页

配置了封面后，封面和首页是同时出现的，封面在上面，首页在下面。
通过设置`onlyCover`参数，可以让docsify网站首页只显示封面，
原来的首页通过`http://localhost:3000/#/README`访问。

在`index.html`文件中的`window.$docsify`添加`onlyCover: true,`选项：

```html
<script>
  window.$docsify = {
    coverpage: true,
    onlyCover: true,
  }
</script>
<script src="./docsify.min.js"></script>
```

> 通过此配置可以把`./README.md`文件独立出来，当成项目真正的README介绍文件。

#### 6.3 多个封面

如果你的文档网站是多语言的，或许你需要设置多个封面。
例如你的文档目录结构如下

```markdown
.
└── docs
    ├── README.md
    ├── guide.md
    ├── _coverpage.md
    └── zh-cn
        ├── README.md
        └── guide.md
        └── _coverpage.md
```

那么你可以在`index.html`文件中的`window.$docsify`这么配置:

```html
window.$docsify = {
  coverpage: ['/', '/zh-cn/']
};
```

或者具体指名文件名:

```html
window.$docsify = {
  coverpage: {
    '/': 'cover.md',
    '/zh-cn/': 'cover.md'
  }
};
```

## 参考链接

- [docsify中文官网](https://docsify.js.org/#/zh-cn/)
- [docsify - 生成文档网站简单使用教程](https://segmentfault.com/a/1190000017576714)