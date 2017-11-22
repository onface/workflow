# 流程规范

## 为什么需要代码规范

团队需要开发规范，每个人都按照开发规范编码便于协同开发。每一个人都是不同的个体，对于什么样的代码是优秀的代码都有自己的理解。开发规范能一定程度上**统一代码风格**，**避免写出不易于维护的代码**和提供一些**好的编程技巧**。

举个反例：

公司有四名前端，对于 class 命名他们的代码分别是

```html
<!-- A -->
<div class="articleBox">
    <h2 class="articleBox-title">标题</h2>
    <div class="articleBox-cnt">内容</div>
</div>

<!-- B -->
<div class="article_box">
    <h2 class="article_box-title">标题</h2>
    <div class="article_box-cnt">内容</div>
</div>

<!-- C -->
<div class="article-box">
    <h2 class="article-box_title">标题</h2>
    <div class="article-box_cnt">内容</div>
</div>

<!-- D -->
<div class="articlebox">
    <h2 class="title">标题</h2>
    <div class="cnt">内容</div>
</div>
```

这四个人同时维护一个项目，他们对于 class 命名的方式都不相同。B维护C的代码非常别扭，因为B使用 `_` 连接单词，C是使用驼峰命名连接。B使用 `-` 分割词组，C使用 `_` 分割词组。完全相反。非常不利于协同开发。

如果代码规范统一采用 A 的命名方式，使用驼峰命名，使用 `-` 分割词组。`统一代码风格`大家就能更好的协同开发了。

> 这个例子中 A B C 关于单词连接方式和分割词组方式只是风格不同，任选其一就可以了。

而 D 的代码有严重问题，按照 D 的命名方式写出的 CSS 是：

```css
.articlebox { ... }
.articlebox .title{ ... }
.articlebox .cnt{ ... }
```

如果在 `<div class="cnt">` 又增加了一些 HTML 就会出现问题

```html
<div class="articlebox">
    <h2 class="title">标题</h2>
    <div class="cnt">
        <div class="tab">
            <span class="trigger">a</span>
            <span class="trigger">b</span>
            <div class="cnt">1</div>
            <div class="cnt">2</div>
        </div>
    </div>
</div>
```

```css
.articlebox { ... }
.articlebox .title{ ... }
.articlebox .cnt{
    color: red;
    border: 1px solid skyblue
}
.tab .cnt {
    color: blue;
}
```

打开浏览器会看到 `.tab` 下面的 `.cnt` 也出现了 蓝色的边框，因为 `.articlebox .cnt{...}` 规则命中了 `<div class="cnt">1</div><div class="cnt">2</div>`

想要解决这个问题就需要改成

```css
.articlebox>.cnt {
    color: red;
    border: 1px solid skyblue
}
```

但是随着层级越来越多，CSS权重就越来越高。权重越来越高以后会造成**用更高的权重覆盖其他规则的恶性循环**。

改为 A B C 所使用的结构命名法能`避免写出不易于维护的代码`。

> 这里只是用 CSS 命名举例， JS不制订代码规范也会遇到类似问题。

扩展阅读：[Trees - 基于树结构的CSS命名规范](https://github.com/onface/trees)

## 如何制订适合自己团队的代码规范



<!--
## 开发流程
1. 需求
2. 全局数据结构设计 store
3. HTML + CSS 静态页面
4. 页面编码
5. 交付测试
-->
