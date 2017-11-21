# 技术选型

上一章：[构建系统](./build.md)

---

既然是技术选型自然要知道有哪些选项

## HTML模板引擎

你的项目会使用到 React/Vue/Angular 其中一个，则完全不需要HTML模板引擎。否则你可以选择 `ejs` `pug(jade)` `handlebars` 等模板引擎。团队负责人应根据项目情况去选择，而不是个人喜好。*了解主流模板引擎的不同点不会花很多时间，磨刀不误砍柴工。*


## CSS 预处理器

主流的有 `less` `sass` `stylus`

预处理不像HTML模板引擎那样，不同的模板引擎有很大差异。CSS 预处理器选择最契合团队环境的就可以。*作者推荐less，原因是安装快*

> 不推荐 CSS in JS解决样式冲突，推荐使用 [CSS modules](http://www.ruanyifeng.com/blog/2016/06/css_modules.html)

## CSS框架

最为出名的CSS框架就是 Bootstrap，但是**项目中还是尽可能的不使用第三方CSS框架**。而是根据项目情况自主开发，或者沿用团队自主开发的框架。

比如 [Alice](http://aliceui.github.io/) 是支付宝的样式解决方案，其实它只适合直接用在支付宝。而不适合直接在自己项目中用。

如果一个项目只有[原型](http://www.woshipm.com/pd/144880.html)没有设计稿，就可以选择一个符合原型的CSS框架，减少开发工作量。

**应该学习一个CSS框架的设计思想和代码分类方式，而不是复制粘贴硬生生的加到项目中**

## JS框架

这个是重头戏，前端社区不断的有人在讨论JS框架的技术选型。目前主流无非就以下几个选择

`jQuery` `React` `Vue` `Angular`

### 不同框架适合不同业务场景

页面交互少，功能简单的用 jQuery 绰绰有余。

页面关联操作特别多，功能复杂的，必然要用 React/Vue/Angular。这种情况下**需要团队负责人根据项目业务场景和团队人员对框架的熟悉程度进行评估**，选择一个最可行的方案。

比如一个项目非常非常适合使用 React 开发，但是团队成员对 Angular 非常喜爱，也有了足够的积累。对 React 的了解程度只是 *单向数据流、JSX* 并且没有任何 React 方面的积累，构建系统都没搭建过。那么立即要开始的项目还是应该用 Angular 。后续再对团队成员进行引导，慢慢了解和熟悉 React。

当然有些团队很强悍，团队成员对主流框架都用的得心应手。完全可以根据项目业务场景选择最适合的框架。但是大部分的团队还是没那么强悍的。

框架没有高低之分，只有是否适合。


延伸阅读：[Vue 对比其他框架](https://cn.vuejs.org/v2/guide/comparison.html)

## ES6 和 JavaScript 超集

ES6 指的是 [ECMAScript6](http://es6.ruanyifeng.com/) JavaScript 超集指的是 [CoffeeScript](http://coffee-script.org/) 和 [TypeScript](https://www.tslang.cn/)

### ES6

构建系统应当支持ES6，团队成员也必须学习ES6。因为它是标准。

### JavaScript 超集

这个也是**需要团队负责人根据项目业务场景和团队人员对框架的熟悉程度进行评估**。超集会增加团队学习成本、招聘成本和新人的培训成本，如果超集确实能提高开发效率和提高项目稳定性还是应该推行使用的。


## 字体图标平台

[iconfont](http://iconfont.cn/) [fontawesome](http://fontawesome.io/) 等平台都会提供一些字体图标，建议团队使用 [iconfont](http://iconfont.cn/) 由设计师创建和维护图标项目的内容。由前端维护图标的 class 和尺寸的调整。

扩展阅读： [iconfont - Font class 是否可以提供 less 文件](https://github.com/thx/iconfont-plus/issues/390)


## 兼容性

> 因为兼容性限制了技术选型所以专门提一下

技术负责人一定要**在商业需求和开发成本中达到一个平衡，服务好客户让公司赚到钱是第一位。**

这个很重要，主要决定权不在技术团队，而是在于需求方。

如果要兼容IE8只能使用 jQuery 1.x 和 React 0.14.x ，要兼容 IE6 IE7 就老老实实的用 jQuery 1.x  [regularjs](http://regularjs.github.io/guide/zh/index.html)  [Avalon](http://avalonjs.coding.me/home.html)

因为要兼容低版本浏览器，构建系统也需要做一些兼容处理 [support-ie8](https://github.com/onface/support-ie8)

> 吐槽：要知道有些单元测试框架都不支持IE8了，写个代码不容易。
