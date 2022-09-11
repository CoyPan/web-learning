## 常见通用属性

HTML属性的一般语法如下：

```
<标签名 属性名1="属性值" 属性名2="属性值" ... 属性名N="属性值">内容</标签名>
```

属性值使用 单引号 和 双引号 均可。

对于HTML元素来说，有几个常见的、通用的属性：class，id，style。

- class：表示元素的“类名列表”。CSS和JavaScript可以通过类选择器或者DOM方法，来选中元素。一个元素可以有多个class, 多个class之间用空格分割。

```
<div class='classname-1 classname-2 classname-3'></div>
```

- id: 表示元素的唯一标识。CSS和JavaScript可以通过类选择器或者DOM方法，来选中元素。一般来说，一个元素，只能有一个ID。一般情况下，开发者应确保ID具有**唯一性**。

```
<div class='foo'></div>
```

::: warning 注意：
如果页面有两个元素的id相同，使用`document.getElementById`获取的元素是HTML代码里靠前的那一个。使用`document.querySelectorAll`可以获取所有相同id的元素。
:::


- style: 定义元素的样式。在style属性里面定义的样式，优先级是比较高的。关于样式优先级，在后面的css教程中会进行总结。

```
<div style="color: #f00;font-size: 12px"></div>
```

class, id, style 这三个属性，是所有元素都支持的。这种所有元素都支持的属性，被称为**全局属性**。除了这几个最常用的，还有一些不太常见但很有用的全局属性：

1. accessKey: 定义键盘快捷键来激活或者聚焦元素
2. contenteditable：指出该元素的内容是否可以被编辑。
3. draggable: 定义元素是否可以被拖动。取值为："true" 或 "false"
4. title: 当鼠标悬停在元素上面时，提示框显示的文本。

## 属性的值

**HTML元素的属性值均为字符串类型**。

```
<div id='foo' class='bar' data-obj='{"foo":"bar"}'></div>
```

一些内容属性（例如required, readonly, disabled, checked）是布尔值属性。如果一个布尔值属性**存在**，则其值是 true，如果不存在，其值是 false。

HTML5规定了布尔值属性的取值规则：如果属性存在，其值必须是一个空字符串（即该属性的值未分配），或者是一个大小写无关的 ASCII 字符串，该字符串与属性名严格相同，前后都没有空格。下述例子是为一个布尔值属性取值的几个有效方式。

```
<button disabled >这是一个按钮</button> - ✅ disabled 生效
<button disabled="">这是一个按钮</button> - ✅ disabled 生效
<button disabled="true">这是一个按钮</button> - ✅ disabled 生效
<button disabled="false">这是一个按钮</button> - ✅ disabled 生效。这个需要特别注意
<button>这是一个按钮</button> - ❌ disabled 不生效
```

::: warning 注意：
draggable 属性是**枚举属性**, 不是布尔值属性。因此，想要draggable生效，必须使用 `draggable="true"`。若不设置draggable属性，则遵循浏览器默认行为。
:::


## 自定义属性

有的时候，我们需要在HTML元素上挂一下额外的信息。这就需要用到HTML元素的自定义属性。自定义属性一般使用这种格式：`data-*` 。举例如下：

```
<div data-custom="hello"></div>
<div data-obj='{"foo":"bar"}'></div>
```

## HTML元素的无障碍适配

什么是无障碍适配？这里摘抄一段MDN上的解释：

> 无障碍是一种让尽可能多的用户可以使用你的网站的做法。传统上我们认为这只与残疾人士有关，但提升网站的无障碍也可以让其他用户群体受益。比如使用移动设备的人群，那些使用低速网络连接的人群。你也可以把无障碍看成是同等地对待每一个人，给他们平等的机会，无论他们的能力或所处的环境如何。就像不能让坐轮椅的人可以进入大楼是错误的 (现代公共建筑通常有轮椅坡道或电梯)；不能让视觉有障碍的人士可以浏览我们的网站同样不正确。我们都是不同的，但我们都是人，因此享有同等的人权。

按照我自己的理解，无障碍适配就是让网页能够表达更好的语义，使得操作系统能够更好将网页内容反馈给有需要的人群。举个例子，无障碍就是要让屏幕阅读器(操作系统)能够识别出页面上的内容，朗读出来，供盲人感知。

如何更好的实现无障碍？第一点，就是书写有良好语义的HTML代码。"div标签一把梭"的形式是不推荐的。举个最简单的例子：

```
<div>这是一个按钮</div> - ❌ 这种写法不推荐
<button>这是一个按钮</button> - ✅ 这种写法更加友好
```

但是，当页面上的UI控件变得复杂时，单纯的HTML语义已经无法满足需求了。这个时候，就需要 WAI-ARIA。WAI-ARIA 是一套规范，定义了一组可用于其他元素的 HTML 特性，用于提供额外的语义化以及改善缺乏的无障碍。简单来说，就是给HTML元素加上几个属性，来补充语义信息。

- 首先是`role`属性。这个属性是说明元素是干什么的。比如，当我们的页面弹出一个弹窗时，我们可以在弹窗的DOM上加上: role='alert'。

```
<div class='modal' role='alert'>
    你好
</div>
```
读屏软件会读出：**提示 你好**

完整的role定义，可以在这里查看：[传送门](https://www.w3.org/TR/wai-aria-1.1/#role_definitions)

- 接着是 `aria-*` 属性。这一类属性是增加更多的语义说明。例如：
```
<a href="xxx" role="button" aria-label="返回上一页">返回</a>
```
读屏软件会读出：**返回上一页 按钮**

这里的 `aria-label` 换成 `title` 属性，也能达到相同的效果。

所有的aria-*属性，可以在这里查看：[传送门](https://www.w3.org/TR/wai-aria-1.1/#state_prop_def)

- 然后是元素的状态属性，尽量不要使用class类来表示状态，可以使用类似 `disabled` 或 `aria-disabled` 来表示状态

```
<div aria-disabled role="button" title="提交">确认</div>
```
