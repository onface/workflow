# 构建系统

上一章：[文件目录](./directory.md)

## 为什么需要构建系统

在上一章[文件目录](./directory.md)中的文件结构，存在一些未解决的问题。

`/m/btn/index.less` 如果要在视图代码中使用，不使用构建系统分别可以通过以下3种方式使用。

1. 在 `/view/login/index.html` 中通过 `<link href="../../m/btn/index.css" />` 引用
2. 在 `/view/login/index.css` 中通过 `@import url(../../m/btn/index.css)`; 引用
3. 复制 `/m/btn/index.css` 文件内容到 `/view/login/index.css` 中

1和2两种方式都会增加一个新的HTTP请求数，页面需要加载 `/m/btn/index.css` 和 `/view/login/index.css`。第三种方式很傻不易于维护 btn 样式一旦修改就要手动同步到多个地方。

可能读者会有疑问：

> 直接把 btn 代码写到 view/pc/index.css 然后在所有页面调用这个css文件就解决了资源重复加载和手动同步的问题。

但是 btn 模块的代码放在 `view/pc/index.css` 中，违反了[文件目录](./directory.md)中提到的**资源就近维护**的原则。

这时我们就要使用CSS预处理器来帮助我们解决这个问题。比如 [Less](http://less.bootcss.com/) [Sass](https://www.sass.hk/)。

比如在 Less 中可以通过 `@import (less) "../m/btn/index.css";` 的方式引入 css 。

```less
// view/login/index.less
@import (less) "../m/btn/index.css";
.login-title {
    font-size:20px;
}
```

通过编译文件会变成

```css
/* view/login/index.less */
.m-btn {
	border:1px solid #EEE;
	background-color: #fafafa;
	color:#333;
	box-sizing: border-box;
}
.m-btn--danger {
	color:pink;
	border-color:red;
}
.login-title {
    font-size:20px;
}
```

还需要让构建系统检测 `/m/btn/index.css` 和 `/view/login/index.less` 被修改时立即编译 less。

而 `/m/switchClass/index.js` 也会遇到跟 `/m/btn/index.css` 相同的问题，这在JS方面统一使用 webpack 解决。[webpack-book](https://github.com/onface/webpack-book)

## 构建系统不等于前端工程

本书的章节顺序是[文件目录](./directory.md)在**构建系统**的前面，是为了纠正一个问题。

一定要先知道问题是什么，然后用构建工具解决这个问题。而不是学会了构建工具的使用，就按照固定的使用方式解决问题。

构建工具教程的的使用示例都只是单一场景，因为它是教程所以没法做到覆盖所有项目情况。需要使用者自己根据项目情况深思熟虑后判断如何使用。

## 构建工具对源码的侵入越少越好

**px to rem**(不要用这个方案)

```css
/* 源码 */
body {
    border-top: 1px;
    border-bottom: 10px;
    padding: 10px; /* @norem */
    background-size: 10px 10px; /* @rem */
}
```

```css
/* 输出 */
body {
    border-top: 1px;
    border-bottom: 0.5557rem;
    padding: 10px;
    background-size: 0.5557rem 0.5557rem;
}
```

直接阅读源码会认为 body的各项单位设置的就是px，会带来误导。而且 `/* @norem */` 的标记非常麻烦。

**less function**

编译 rem 这种需求应该使用CSS预处理器的函数功能解决

[less-rem](https://github.com/onface/less-rem)

```less
// 源码
@import "./rem";
.demo {
    width:rem(640);
    height:rem(100);
    box-shadow: rem(11) rem(22) rem(33) pink;
    background: #eee;
}
```

```less
// rem.less
.function {
    .rem(@size) {
        // 640 是设计稿宽度
        // 640 is psd width
        return: @size/(640/320*16)+0rem;
    }
}
```

```css
/* 输出 */
.demo {
  width: 20rem;
  height: 3.125rem;
  box-shadow: 0.34375rem 0.6875rem 1.03125rem pink;
  background: #eee;
}
```

直接阅读源码可以追踪到 `rem.less` 就会明白这个项目的 rem 计算标准是什么


## 构建工具的选择

**因为自己搭建前端工程的构建系统才需要看这一部分所以内容移动到了 [issues](https://github.com/onface/workflow/issues/1)**，各种坑都在 issues 中。

## 构建工具的组合

gulp fis webpack 应该组合使用，因为：

1. gulp 虽然有 [gulp-webpack](https://www.npmjs.com/package/gulp-webpack) 但是构建速度完全比不上**单独启动一个服务器加上 [webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware)** 。
2. fis3 虽然有 [fis3-parser-webpack](https://www.npmjs.com/package/fis3-parser-webpack) 但是做不到异步加载和热更新。
3. webpack 虽然能[提取单独样式文件](https://github.com/onface/webpack-book/tree/gh-pages/6-extract-text) 但是不可能用它提取所有文件。还是需要 fis3 或者 gulp 构建非JS文件。

那么就要选择 gulp 还是 fis3 作为构建工具。因为 [静态资源映射表](http://fis.baidu.com/fis3/docs/lv3.html#%E5%9F%BA%E4%BA%8E%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E7%9A%84%E6%A8%A1%E5%9D%97%E5%8C%96%E6%96%B9%E6%A1%88%E8%AE%BE%E8%AE%A1) 的原因，作者选择 fis3。当然你也可以选择 gulp ，毕竟可以改造它们以满足自己的需求。**只是时间成本和技术成本都很高，对两个构建工具做详细了解后选择改造工作量最小的**。时间充裕的情况下多花点时间了解（磨刀不误砍柴工）。


## markrun

[markrun](https://github.com/markrun/markrun) 主要的功能是

**源码**

    ````js
    document.title = "js - basic a 18"
    ````

**输出**

```html
<pre>
    document.title = "js - basic a 18"
</pre>
<script>
    document.title = "js - basic a 18"
</script>
```

[![](https://github.com/markrun/markrun/raw/master/doc/media/preview.png)](https://markrun.github.io/markrun/)

利用 markrun 配合构建系统就能在 `/m/**/README.md` 中 编写一份代码，生成的html中出现 `pre` 和 `script/style/html`

单独列出 markrun 是因为提高了模块的文档编写速度开发人员就更愿意写文档，能在开发模块的同时完成简单的使用示例编写。
