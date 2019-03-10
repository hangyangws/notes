# word-break、word-wrap、overflow-wrap 和 white-space 运用与差别

一些前端开发对这三个属性有一定的「困惑」，接下来对他们进行简单的解释与区别

### word-break

顾名思义，`word-break` 指定了怎样在单词内断行

**常用的值**：

- normal

「中文/日文/韩文」会自动换行，「连续的数字和英文之间」不会自动换行

- break-all

在任意字符之间换行，包括「连续的数字和英文」

- keep-all

「连续的中文/日文/韩文/英文/数字」不会自动换行

**注意**：

不论是「中文/日文/韩文」还是「英文/数字」，都有「连续」和「非连续」状态，  
比如：「我是连续中文」、「我 是 非 连 续 中 文」  
首先记住一点「任何语言，只要是非连续的，都会换行」  
还要记住一点，在一些浏览器中「例如：Chrome」支持 `break-word` 值，但是 `break-word` 不是官网属性，最好不要使用

### word-wrap、overflow-wrap

**注意**：

`word-wrap` 属性原本属于微软的一个私有属性，在 CSS3 现在的文本规范草案中已经被重名为 `overflow-wrap`  
`word-wrap` 现在被当作 `overflow-wrap` 的 「别名」  
稳定的谷歌 Chrome 和 Opera 浏览器版本支持这种新语法

**常用的值**：

- break-word

如果元素没有多余的地方容纳该单词到结尾，则单词会被强制分割换行

### white-space

`white-space` 告诉浏览器何处理元素中的空白符

**常用的值**：

- normal

「默认」忽略空白符，忽略原有换行符，自动换行

- nowrap

忽略空白符，文本不换行，直到遇到 `</br>` 标签才换行

- pre

保留空白符，保留换行符，即保留原有格式，不会自动换行  
用户从 `<textarea>` 输入内容，如果把内容放在普通标签里面，则会丢失换行符，`pre` 属性可以保持换行符

- pre-wrap

保留空白符、换行符，自动进行换行

- pre-line

合并空白符，保留换行符

# display: inline

`inline` 其实没有什么好说的，但是有个点值得注意，inline 元素设置背景只在文字下面生效  
可以利用这一特性，制作文字下划线标注效果

# 盒模型之：W3C 与 IE 的区别

区别应该很多，我直说其中一种  
如果设置 `width: 100px;`  
在IE6 及以前的混杂模式下的盒模型的效果是：「contentWidth + (borderWidth + paddingWidth) * 2 === 100」  
W3C 标准盒模型下的效果是：「contentWidth === 100」

# CSS hack

属性级 hack

```css
_color:red; /* 仅IE6 识别 */

+color:red; /* IE6、IE7 识别 */
*color:red; /* IE6、IE7 识别 */
*+color:red; /* IE6、IE7 识别 */
[color:red; /* IE6、IE7 识别 */

color:red\9; /* IE6、IE7、IE8、IE9 识别 */

color:red\0; /* IE8、IE9 识别*/

color:red\9\0; /* 仅IE9识别 */
```

选择符级 hack

```
*html #demo { color:red;} /* 仅IE6 识别 */
*+html #demo { color:red;} /* 仅IE7 识别 */
```

IE 条件注释 hack

```html
<!-- [if IE]>此处内容只有IE可见<![endif]-->
<!-- [if IE 6]>此处内容只有IE6.0可见<![endif]-->
<!-- [if !IE 6]>此处内容只有IE6不能识别，其他版本都能识别，当然要在IE5以上。<![endif]-->
<!-- [if gt IE 6]> IE6以上版本可识别,IE6无法识别 <![endif]-->
<!-- [if gte IE 6]> IE6以及IE6以上版本可识别 <![endif]-->
<!-- [if lt IE 6]> 低于IE6的版本才能识别，IE6无法识别。 <![endif]-->
<!-- [if lte IE 6]> IE6以及IE6以下版本可识别<![endif]-->
<!-- [if !IE]>此处内容只有非IE可见<![endif]-->
```

# BFC

BFC「Block formatting context - 块级格式化上下文」是 CSS 布局的一个概念，代表独立的渲染区域  
我们常说的文档流其实分为「定位流、浮动流、普通流」三种，BFC 中的 FC 指的是「普通流」  
除了`BFC` 还有：`IFC`（inline FC）、`GFC`（grid FC）……  

下列条件之一就可触发 BFC

- 根元素，即 HTML 元素
- float 的值不为 none
- overflow 的值不为 visible
- display 的值为 inline-block、table-cell、table-caption
- position 的值为 absolute 或 fixed

BFC 的特性如下：

1. BFC 里面的盒子和其外面的盒子间是不会有任何影响
1. 内部的 Box 会在垂直方向，一个接一个地放置
1. Box 垂直方向的距离由 margin 决定。属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠
1. 每个元素的 margin box 的左边，与包含块 border box 的左边相接触(对于从左往右的格式化，否则相反)。浮动元素也是如此
1. BFC 的区域不会与 float box 重叠
1. 计算 BFC 的高度时，浮动元素也参与计算

那么可以根据 BFC 的特性，做到如下效果：

- 自适应两栏布局 [点击查看代码](https://jsfiddle.net/hangyangws/1d4yj6pp/2/)

- 清除浮动 [点击查看代码](https://jsfiddle.net/hangyangws/w13kL9fy/)

- 阻止 margin 重叠 [点击查看代码](https://jsfiddle.net/hangyangws/tj5mod53/)

# line-height 以及 vertical-align

[link](http://web.jobbole.com/91180/)

无单位的 `line-height` 是 `font-size` 相关联的  
但问题是 `font-size：100px` 在不同字体时表现是不一样的
