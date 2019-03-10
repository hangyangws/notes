# 我遇见的哪些 CSS 中有趣的尺寸、宽高

### 写在前面

目的：「攻巩固 `CSS` 知识，发现新的『桃花源』」  
受众群体：「web 前端开发者、爱好者」  
感谢：

1. [www.w3.org](https://www.w3.org) 文献
1. [张鑫旭](www.zhangxinxu.com) 个人博客

### 不常见长度单位

- `ex` 相对于「小写字母」`x` 的 **高度**
- `ch` 相对于「数字」 `0` 的 **宽度**

利用 `ch` 单位实现「打字效果」的 [一个 Demo](http://hangyangws.win/demos/src/css/ch-typing/)  
**核心原理**：利用宽度 `ch` 刚好是一个字母的宽度实现「宽度等比增长」

- vw 相对于视窗的 **宽度**：视窗宽度是 100vw「window.innerWidth」
- vh 相对于视窗的 **高度**：视窗高度是 100vh「window.innerHeight」
- vm 取决于 `min(vw, vm)`

利用 `vw` 实现的一个「懒加载时，页面不随着滚动条抖动」的 [一个 Dome](http://hangyangws.win/demos/src/css/vw-scroll/)  
温馨提示：mac 的浏览器滚动条不占「显示区域」的宽度  
**核心原理**：利用宽度 `calc(100vm - 100%)` 刚好等于滚动条的宽度

### 实用的实体单位

- `&emsp;` 相当于 em 的宽度
- `&ensp;` 相当于 1/2 em 的宽度

**使用场景**

当我们需要给文字增加间隙的时候，可能会使用 `last-letter`，但是他会使最后一个字符后面也有间距  
所以可以使用 `string.split('').join('&ensp;')` 的方式

利用 `&emsp;` 实现的对齐效果 [一个 Demo](http://hangyangws.win/demos/src/html/emsp/)
> 另外 `text-align: justify;` 也能实现「文本两端对齐」的效果 [一个 Demo](http://hangyangws.win/demos/src/html/justify/)

### height、width、百分比、循环

先抛出一个疑问：如果没有显示的设置父元素宽高，子元素宽高的百分比有效果吗？  
[看一个 Demo](http://hangyangws.win/demos/src/html/percentage-w-h/)

为什么父元素没有显示得设置高度，子元素高度设置 100% 就没有效果呢？
在翻阅 [w3 对高度的定义](https://www.w3.org/TR/CSS21/visudet.html#propdef-height) 、[w3 对宽度的定义](https://www.w3.org/TR/CSS21/visudet.html#blockwidth)，后了解到浏览器的宽高计算原理：

**高度的计算**

如果元素 **没有显示的设置高度** 且 **非 fixed/absolute** 定位，  
其 **渲染高度** 为子元素的的高度，其 **计算高度** 为 `auto`  
所以如果未显示设置父元素高度，其高为 auto，子元素 `height: 100%` 的意思就是 `auto * 100% ≈ auto`

**宽度的计算**

如果元素 **没有显示的设置宽度** 且 **非 fixed/absolute** 定位，  
如果元素是块级元素，其 **渲染宽度** 为父元素的的宽度，其 **计算宽度** 也为父元素的的宽度  
如果元素是行级元素，其 **渲染宽度** 为子元素的的宽度，其 **计算宽度** 也为子元素的的宽度  
所以未显示设置宽度，其高为和 `渲染高度` 一样，所以子元素的百分比宽度有效果

再抛出一个疑问：[这个 Demo](http://hangyangws.win/demos/src/html/percentage-w-h/) 的父元素的宽为什么不是子元素的宽度和？  
如果父元素宽度变化，这样会不会带来渲染循环问题？

答案是不会，让我来讲解一下浏览器渲染的基本顺序和原理：

1. 下载文档内容，生成 DOM
1. 加载头部的样式资源，生成 CSSOM
1. 按照从上而下，自外而内的顺序渲染 DOM 内容「先渲染父元素，后渲染子元素」

遵循上面的原则，当浏览器渲染父元素的时候，还未渲染子元素，  
这个时候浏览器要根据子元素的宽度来计算父元素的宽度，  
不过这个时候，子元素 DOM 还没有结合 CSSOM 渲染，所以子元素就是图片和文字的宽度和，  
所以父元素的宽度就是图片和文字的宽度和

所以我们也明白了： **CSS 中父元素选择器** 久久未实现的原因，是因为这样真的会导致「循环渲染」

### padding 百分比

抛出一个问题：padding-top、padding-bottom 如果设置为百分比，是相对于什么来计算的？  
答案是相对于父元素的宽度来计算的「一脸吃惊的表情，记住就好」😑

### height:100% 和 height:inherit 的异同

如果 `height:inherit` 是继承父元素的高度，那么和 `height:100%` 不是没有什么区别么？  
一般情况他们二者没有区别，区别在于元素为「绝对定位」时  
绝对定位元素的 `height:inherit` 是相对于父元素计算；而 `height:100%` 相对于定位基准元素计算  

### width 新鲜值

作用：在原有的 display 水平不变的情况下拥有元素其他 display 值才有的特性

- fill-available

fill-available 元素会充分利用可用空间，就像 div 一样「默认 100% 宽度」。  
块级元素默认宽度表现行为就是 fill-available。
[一个 Demo 认识 fill-available](http://hangyangws.win/demos/src/css/width/fill-available)

- max-content

**假设** 我们的容器有足够的宽度，足够的空间，此时，所占据的宽度是就是 max-content 所表示的尺寸  
[一个 Demo 认识 max-content](http://hangyangws.win/demos/src/css/width/max-content)  
[一个 max-content 的实际用例](http://hangyangws.win/demos/src/css/width/max-content-2)

- min-content

min-content 元素宽度为「内部元素最小宽度值最大的那个元素的宽度」
最小宽度值的意思：  
例如图片的最小宽度值就是图片呈现的宽度；  
对于文本元素，如果全部是中文，则最小宽度值就是一个中文的宽度值；
如果包含英文，因为默认英文单词不换行，所以，最小宽度可能就是里面最长的英文单词的宽度。

- fit-content

fit-content 元素的宽度计算方式和「float、absolute、inline-block」一样  
这种计算方式被称为「shrink-to-fit」

举例一个使用场景，就拿水平居中效果举例：  
inline-block 元素需要父级使用 `text-align: center`，而本身可能还需要 `text-align: left`。😨 mdzz~~~  
而 `width: fit-content` 可以没有这些烦恼，  
因为，`width: fit-content` 可以实现元素收缩效果的同时，保持原本的 block 水平状态  
于是，就可以直接使用 `margin: 0 auto` 实现元素向内自适应同时的居中效果了
