## 第一个vue-cli项目

### 1. 什么是vue-cli

 vue-cli 官方提供的一个脚手架,用于快速生成一个 vue 的项目模板;

 预先定义好的目录结构及基础代码，就好比咱们在创建 Maven 项目时可以选择创建一个骨架项目，这个骨架项目就是脚手架,我们的开发更加的快速;

 **主要功能：**

- 统一的目录结构
- 本地调试
- 热部署
- 单元测试
- 集成打包上线

### 2. 需要的环境

- Node.js : http://nodejs.cn/download/

  安装就无脑下一步就好,安装在自己的环境目录下

- Git : https://git-scm.com/downloads
  镜像:https://npm.taobao.org/mirrors/git-for-windows/

**确认nodejs安装成功:**

- cmd 下输入 `node -v`,查看是否能够正确打印出版本号即可!
- cmd 下输入 `npm-v`,查看是否能够正确打印出版本号即可!

这个npm,就是一个软件包管理工具,就和linux下的apt软件安装差不多!

*npm* 是 JavaScript 世界的包管理工具,并且是 Node.js 平台的默认包管理工具。通过 *npm* 可以安装、共享、分发代码,管理项目依赖关系。

**安装 Node.js 淘宝镜像加速器（cnpm）**

这样子的话,下载会快很多~

```txt
# -g 就是全局安装
npm install cnpm -g

# 若安装失败，则将源npm源换成淘宝镜像
# 因为npm安装插件是从国外服务器下载，受网络影响大
npm config set registry https://registry.npm.taobao.org

# 然后再执行
npm install cnpm -g
```

### 3. 安装vue-cli

```txt
#在命令台输入
cnpm install vue-cli -g
#查看是否安装成功
vue list
```

### 4. 第一个 vue-cli 应用程序

创建一个Vue项目,我们随便建立一个空的文件夹在电脑上。

 我这里在D盘下新建一个目录D:\Project\vue-study;

创建一个基于 webpack 模板的 vue 应用程序

```txt
# 这里的 myvue 是项目名称，可以根据自己的需求起名
vue init webpack myvue
```

一路都选择no即可;

 初始化并运行

```txt
cd myvue
npm install
npm run dev
```

## 十、Webpack

 WebPack 是一款**模块加载器兼打包工具**，它能把各种资源，如 JS、JSX、ES6、SASS、LESS、图片等**都作为模块来处理和使用。**

```t
npm install webpack -g
npm install webpack-cli -g
```

 测试安装成功: 输入以下命令有版本号输出即为安装成功

```txt
webpack -v
webpack-cli -v
```

### 1. 什么是Webpack

 本质上，webpack是一个现代JavaScript应用程序的静态模块打包器(module bundler)。当webpack处理应用程序时，它会递归地构建一个依赖关系图(dependency graph),其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个bundle.
 Webpack是当下最热门的前端资源模块化管理和打包工具，它可以将许多松散耦合的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分离，等到实际需要时再异步加载。通过loader转换，任何形式的资源都可以当做模块，比如CommonsJS、AMD、ES6、 CSS、JSON、CoffeeScript、LESS等;
 伴随着移动互联网的大潮，当今越来越多的网站已经从网页模式进化到了WebApp模式。它们运行在现代浏览器里，使用HTML5、CSS3、ES6 等新的技术来开发丰富的功能，网页已经不仅仅是完成浏览器的基本需求; WebApp通常是一个SPA (单页面应用) ，每一个视图通过异步的方式加载，这导致页面初始化和使用过程中会加载越来越多的JS代码，这给前端的开发流程和资源组织带来了巨大挑战。
 前端开发和其他开发工作的主要区别，首先是前端基于多语言、多层次的编码和组织工作，其次前端产品的交付是基于浏览器的，这些资源是通过增量加载的方式运行到浏览器端，如何在开发环境组织好这些碎片化的代码和资源，并且保证他们在浏览器端快速、优雅的加载和更新，就需要一个模块化系统，这个理想中的模块化系统是前端工程师多年来一直探索的难题。

### 2. 使用Webpack

1. 先创建一个包 交由idea打开 会生成一个.idea文件 那么就说明该文件就交由idea负责
2. 在idea中创建modules包，再创建hello.js,hello.js 暴露接口 相当于Java中的类

```js
//暴露一个方法
exports.sayHi = function () {
    document.write("<h1>狂神说ES6</h1>>")
}
```

创建main.js 当作是js主入口 , main.js 请求hello.js 调用sayHi()方法

```js
var hello = require("./hello");
hello.sayHi()
```

在主目录创建webpack-config.js , webpack-config.js 这个相当于webpack的配置文件

enrty请求main.js的文件

output是输出的位置和名字

```js
module.exports = {
    entry: './modules/main.js',
    output: {
        filename: './js/bundle.js'
    }
}
```

1. 在idea命令台输入webpack命令（idea要设置管理员启动）
2. 完成上述操作之后会在主目录生成一个dist文件 生成的js文件夹路径为/ dist/js/bundle.js
3. 在主目录创建index.html 导入bundle.js
   index.html

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="dist/js/bundle.js"></script>
</head>
<body>
</body>
</html>
```