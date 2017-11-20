## 文件目录

项目代码可以分为两类

1. 模块代码 module
2. 视图代码 view

> 注意是**视图代码**而不是*业务逻辑代码*，因为业务逻辑可以封装成模块。

当你刚入行开发一个企业站时，只需要写视图代码。比如

```js
$(function () {
    $('.js-btn').on('click', function () {
        $('.js-text').toggleClass('light')
    })
})
```
随着技能的提升会将某些可复用的业务代码写成一个函数以便以重复使用。比如

```js
function switchClass(element, target, className){
    $(element).on('click', function () {
        $(target).toggleClass(className)
    })
}
```

> 实际工作中会对一些复杂的模块进行封装，不像 这里的 `switchClass()` 这么简单。

也会对某些CSS进行封装便于复用

```html
<link href="./btn.css">
<span class="m-btn">确认</span>
<span class="m-btn m-btn--danger">删除</span>
```

```css
/* btn.css */
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
```

很多项目的代码会根据文件类型的不同存放在不同的文件夹下

```
css/
    - common.css    # 将 btn 的样式写在 common.css 中
img/
    - logo.png
js/
    - common.js     # 将 switchClass()写在 common.js 中
    - news.js
    - login.js
    - index.js
html/
    - news.html
    - login.html
    - index.html
```

这种方式在页面越来越多时会难以维护，有时还需要在多个目录不停的切换。

为了便于检索，我们可以将模块代码和视图代码分别放在 `m` 和 `view` 文件夹下。

**相关资源就近维护**

```shell
m/
    btn/
        - loading.png
        - index.css
        - README.html   # 编写 btn 的 html 示例
    switchClass/
        - index.js
        - README.html   # 编写 switchClass 的使用示例
view/                   # 调用模块代码和一些不需要封装的逻辑代码
    pc/
        - base.js       # 底层库(jQuery React Vue 等)
        - index.js      # 公用界面代码（控制导航栏搜索展开，点击底部客服弹出窗口） — 单页应用则不需要 pc/index.js pc/index.css —
        - index.css     # 存放 normalize.css 和少量公用样式，（不要使用 CSS Reset）
    login/
        - index.js      # 调用 m/swtich/index.js 提供的 swtich()
        - index.html    # 引入 m/btn/index.css 和使用 <span class="m-btn">确认</span>
        - logo.png
```

不命名为 `view/common` 而命名为 `view/pc` 是因为随着项目需求的变化，可能会出现 `view/mobile` 。

不只是相关资源就近维护了，还增加了 `/m/btn/README.html` 和 `/m/switch/README.html` 文件。

README.html 的文件内容是：

```html
<!-- /m/btn/index.html -->
<link href="./index.css">
<span class="m-btn">确认</span>
<span class="m-btn m-btn--danger">删除</span>
```

```html
<!-- /m/switchClass/index.html -->
<button class="js-btn">click</button>
<span class="js-text">Abcdef</span>

<script src="./index.js" ></script>
<script style="display:block;" >
switchClass('.js-btn', '.js-text', 'light')
</script>
```

此处的两个 README.html 文件中编写当前模块的使用示例。这样其他同事想要使用这个模块时可以通过文档快速了解用哪些用法，不需要询问开发者。

继续维护这个模块时，我们会需要对已有的模块代码进行修改。不知道模块代码会被如何使用的情况下是不敢轻易修改的，但是参照着示例修改就能知道本次修改会不会影响到某一种示例。（复杂的模块最好写单元测试）

**模块再小都要写示例，磨刀不误砍柴工。**

## 小结

1. 资源就近维护
2. 代码分为模块代码和视图代码
3. 模块一定要编写文档和示例
4. 使用 `/view/pc` 文件夹替代 `/common`，便于后续增加 `/view/mobile`

```shell
m/
    btn/
    switchClass/
view
    pc/
        - base.js
        - **.**
    login/    
```
