# 流程规范

## 开发规范

### 为什么需要开发规范

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

如果开发规范统一采用 A 的命名方式，使用驼峰命名，使用 `-` 分割词组。`统一代码风格`大家就能更好的协同开发了。

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

> 这里只是用 CSS 命名举例， JS不制订开发规范也会遇到类似问题。

扩展阅读：[Trees - 基于树结构的CSS命名规范](https://github.com/onface/trees)

### 如何制订适合自己团队的开发规范

以制订 CSS 命名规范为例，很多前端都去搜索引擎搜索过CSS命名规范。

会有一些看起来很有道理，**实际上解决不了根本问题的规范**。

比如：

> CSS书写顺序：位置属性 -> 大小 -> 文字 -> 背景 -> 其他

> 尽量使用CSS缩写属性

> 16进制颜色代码缩写

> CSS样式表文件命名：主要的 master.css - 模块 module.css - 基本共用 base.css - 布局、版面 layout.css - 主题 themes.css - 专栏 columns.css - 文字 font.css - 表单 forms.css - 补丁 mend.css - 打印 print.css

这些规范，你遵守或不遵守都对项目没什么太大影响。

**开发规范需要自己根据团队实际情况去制订**

举几个例子

1、在[文件目录](./directory.md)章节所描述目录结构中。后期维护时在HTML中看到一个 `<div class="some"></div>` 代码。想要找到 `.some` 的样式在哪儿，却找不到。可以制订规范，所有 `m/` 文件夹下的 css 都要以 `.m-` 作为前缀。这样看到一个 class 如果是 `.m-box` 则直接去找 `m/box/index.css` ，看到  `.some` 则直接找HTML同级目录的 css 文件。

2、后期维护时样式总是CSS选择器权重过高无法通过新增 class 的方式覆盖样式，则约定完全不要使用ID选择器，class 选择器使用 `.m-box-hd-title {}` 这种结构命名法降低权重。保持大部分选择器权重都是 `0, 0, 1, 0`

3、有时删除了一个 class ，JS 绑定的事件就失效了，则可以将所有用于 JS 选择的 class 都以 `.js-` 作为前缀。例如： `.js-submit` `.js-list-remove`

**先遇到问题，再制订能解决这个问题的规范**


### 新规范的试验期

当遇到一个新问题时，根据问题制订一个规范。当不是很确定这个规范能解决这个问题时。不要着急将新的规范加入开发规范文档。而是找一些经验丰富的同事先在一个项目中试验一次。项目结束后经过讨论觉得可行时再正式加入开发规范文档。**开发规范反复变更会增加一线开发人员学习和记忆成本**。


## 需求合理性


<!--
## 开发流程
1. 需求
2. 全局数据结构设计 store
3. HTML + CSS 静态页面
4. 页面编码
5. 交付测试
-->
