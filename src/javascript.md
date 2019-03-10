# JS 的 7 种数据类型

## 6 种原始类型

1. **Null**  
    只有一个值： `null`
1. **Undefined**  
    一个没有被赋值的变量会有个默认值undefined，也可以手动赋值为undefined
1. **Number**
1. **Boolean**  
    只存在两个值：`true` 和 `false`
1. **String**
1. [**Symbol**](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)  
    `ES6`新增基本类型

## 1种引用类型

> 对象指内存中的可以被标识符引用的一块区域  
比如：数组、对象…

1. **Object**

# 数据类型检测之 `typeof`

对变量或值调用 `typeof` 运算符将返回(字符串)下列值之一

1. **undefined** (Undefined 类型)
1. **number** (Number 类型)
1. **boolean** (Boolean 类型)
1. **string** (String 类型)
1. **symbol** (Symbol 类型 - ECMAScript6 新增)
1. **function** (函数对象 - ECMA-262 条款中实现了)
1. **object** (引用类型或 Null 类型)

上面的返回值中的前 5 种都好理解  
但是后两种情况得注意，什么时候返回 object，什么时候返回 function

```javascript
// 当检测函数对象的时候，返回 'function'

typeof Function // 返回：function (Function 是函数对象)

// 我们通常理解一个对象实例的时候，是通过 new 关键字
// 但是这里有特殊情况，如果构造函数本身是 Function
// 那么 new 出来的实例也是一个 function 并不 object

typeof new Function // 返回：function
// new Function 依旧是是函数对象

var func = function(){};
typeof func // 返回：function
// func 是函数对象，和上一个列子一样的意思

typeof new new Function // 返回：object
// 第一次 new 返回「函数对象」，第二次 new 返回实例「引用类型」

typeof Array // 返回：function
// Array 是函数对象，等同于默认存在：function Array() {…} 这么一个函数

typeof new Array // 返回：object
// 实例化的 Array 是「引用类型」

typeof [1, 2] // 返回：obejct
// [1 ,2]是「引用类型」，等同于：new Array(1, 2)

typeof Object // 返回：function
// Object 是函数对象，等同于默认存在：function Object() {…} 这么一个函数

typeof {a: 'a'} // 返回：object
// {a: 'a'}是「引用类型」，等同于：new Object({a: 'a'})

// 综上所述，开发者要注意了
// 数组并不是数组，对象并不是对象 ^_^，容我幽默一下
```

# 数据类型检测之 `instanceof`

> 语法 `object instanceof constructor`  
instanceof 通过原型链来判断对象时候属于它的父类型，判断原则：  
先查找 object 的 `__proto__` 链，同步查找 constructor 的 `prototype` 链，如果两者相等，就返回 true

```javascript
function Func() {};
var obj = new Func;
obj instanceof Func; // 返回：true （因为 obj.__proto__ === Func.prototype）

Func.prototype = {}; // 重定向 Func 的 prototype

var obj2 = new Func;
obj instanceof Func; // 返回：false (因为 Func 的 prototype 指向一个空对象，这个空对象不在 obj 的原型链上)
obj2 instanceof Func; // 返回：true

function Func2() {};
Func2.prototype = new Func();

var obj3 = new Func2();
obj3 instanceof Func2; // 返回：true （因为 obj3.__proto__ === Func2.prototype）
obj3 instanceof Func; // 返回：true （因为 obj3.__proto__.__proto__ === Func.prototype）
```

# defer 和 async

> 开发者喜欢把 JS 文件放在 body 闭合标签之前，这是为什么呢  
因为加载 `<script src="xxx.js">` 会堵塞 `DOM` 树的解析与构建  
解析到 `<script src="xxx.js">` 时，浏览器会停止 `DOM` 树的构建，而去下载当前 JS 文件  
如果 `<script src="xxx.js">` 下载需要6秒，下载时 `DOM` 树还没有构建完成，那么页面会延迟 6 秒加载，出现 6 秒白屏

## defer「推迟」

**使用方式**：  
添加 `defer` 属性：`<script src="xxx.js" defer>`

**作用效果**：  
当浏览器解析到 `<script>` 时，同时（异步）解析 `DOM` ，并且开始下载 `JS`  
当 `JS` 下载完成后，并不会马上执行  
而是继续解析 `DOM` ，当 `DOM` 构建完成(DOMContentLoaded)后再执行 `JS` 内容

## async「异步」

**使用方式**：  
添加 `async` 属性：`<script src="xxx.js" async>`

**作用效果**：  
当浏览器解析到 `<script>` 时，同时（异步）解析 `DOM` ，并且开始下载 `JS`  
当 `JS` 下载完成后，就会马上执行，并且停止 `DOM` 的解析  
当 `JS` 执行完成后，又开始解析 `DOM`

## 总结 defer、async

`defer` 和 `async` 在下载 `JS` 时是一样的，相较 `DOM` 解析都是异步  
它俩的差别在于：`JS` 下载完之后何时执行  
`defer` 执行顺序是和脚本放置位置一样  
`async` 执行则是乱序，不管脚本放置顺序如何，只要加载完了就会立刻执行  
`defer` 的效果最接近「**把脚本放在 `<body>` 闭合标签前**」  
`async` 用到的场景比较少

# 谈谈 offsetTop、offsetParent、offsetHeight

HTMLElement.offsetParent 是一个只读属性，返回一个指向最近的（closest，指包含层级上的最近）包含该元素的定位元素。如果没有定位的元素，则 offsetParent 为最近的 table, table cell 或根元素（标准模式下为 html；quirks 模式下为 body）。当元素的 style.display 设置为 "none" 时，offsetParent 返回 null。offsetParent 很有用，因为 offsetTop 和 offsetLeft 都是相对于其内边距边界的。

在 Webkit 中，如果元素为隐藏的（该元素或其祖先元素的 style.display 为 "none"），或者该元素的 style.position 被设为 "fixed"，则该属性返回 null

在 IE 9 中，如果该元素的 style.position 被设置为 "fixed"，则该属性返回 null。（display:none 无影响。

# 元素视图之 getBoundingClientRect()、getClientRects()、elementFromPoint()

## HTMLElement.prototype.getBoundingClientRect

用于判断元素尺寸和位置  
**返回值**：

```javascript
{
    // 下面的值除了 width、height 都可能为负数「元素不在视图内的时候」
    // 以下值，都是 number 类型
    // 注意：IE7以下浏览器，视口的默认原点为(2, 2)，开发者注意
    top: 0, // 元素上 border 相对于视口上边的纵坐标
    bottom: 0, // 元素下 border 相对于视口上边的纵坐标
    left: 0, // 元素左 border 相对视口左边的横坐标
    right: 0, // 元素右 border 相对视口左边的横坐标
    width: 0, // 元素宽度（border+padding+width）
    height: 0 // 元素高度（border+padding+width）
}
```

## HTMLElement.prototype.getClientRects

主要用于行内「inline」元素（如：`<a>` … ）  
可以用于判断行内元素是否换行，以及行内元素的每一行的位置偏移  
可以用于读取行内元素的行数  
**返回值**：  
一个 `TextRectangle` 对象「一个类数组对象」

```javascript
var test = element.getClientRects();
test.length; // 如果 element 是非 inline 元素，test.length 为 1，否则为元素的行数
// test[0]、test[1]…返回的值与 getBoundingClientRect 类似
```

## elementFromPoint

> 查看视口中指定位置是什么元素  
注意：返回的元素是指定坐标的最上层（z-index 最大）和最里层（最里层的子元素）的 Element 对象

**用法**：
`document.elementFromPoint(x, y)`

**返回值**：
`Element对象`

# 函数继承

> 我们知道，JS 不同于 C++ 这类语言，支持继承，但是我们可以「模仿」

```javascript
// 定义一个简单的父类
function Father(name) {
    this.name = name || 'noName';
    this.show = function() {
        console.log(this.name);
    }
}


// 方法一：「原型链」实现继承
function Son1() {}
Son1.prototype = new Father;
// 代码测试
var son1 = new Son1();
son1.show(); // 返回：noName
son1.prototype.name = 'hangyangws';
son1.show(); // 返回：hangyangws
// 分析：（缺点）创建子类实例时，无法向父类构造函数传参
//      （缺点）无法实现多重继承


// 方法二：使用「对象冒充」实现继承（也叫「构造继承」）
function Son2(name) {
    Father.call(this, name);
}
// 上面函数还有另一种写法
function Son2(name) {
    this.Father = Father; // Son 内部的 Father 属性指向 Father 函数
    this.Father(name); // 执行 Son 内部的 Father 函数
}
// 代码测试
var son2 = new Son2('hangyangws');
son2.show(); // 返回：hangyangws
// 分析：（缺点）不能继承父类原型链上的方法
//      （缺点）实例并不是父类的实例，只是子类的实例


// 方法三：组合继承
function Son3(name) {
    Father.call(this, name);
}
Son3.prototype = new Father;
// 分析：（优点）在方法二的基础上，消除「不能继承父类原型链上的方法」缺点
//      （缺点）调用两次父类构造函数，生成两份实例（子类实例将子类原型上的那份屏蔽了）


// 方法四：升级版组合继承（我也不知道叫什么名字了好了 ^_^）
function Son4(name) {
    Father.call(this, name);
}
Son4.prototype = (new Father()).__proto__;
// 分析：（优点）在方法三的基础上，消除「调用两次父类构造函数」缺点
```

# arguments、callee、caller

> 这三种都在严格模式（use strict）下禁用了，开发者请[注意](https://zhidao.baidu.com/question/1385936076596542060.html)

## arguments

> arguments是：函数在调用时，内部创建的一个类似数组的对象

```javascript
function func1() {
    console.log(arguments);
}
function func2(param) {
    console.log(arguments);
}
func1(); // 打印：[]
func1(1, 2); // 打印：[1, 2]
func2(); // 打印：[]
func2(1, 2, 3); // 打印：[1, 2, 3]
// 总结：arguments「长得像数组」
// arguments存储的是：传递给函数的参数，并不局限于函数声明的参数列表，即使没有声明参数也可以
```

## callee

> `callee` 是 `arguments` 的一个属性，表示当前执行的函数

```javascript
function numAdd(num) {
    if (num < 1) {
        return 0;
    }
    return num + arguments.callee(--num);
}
numAdd(3); // 返回：6

// 总结：callee 是 arguments 对象的参数
// arguments.callee 就是当前执行的函数
// 可以借助 arguments.callee 实现 **自身调用** 或者 **递归调用**
// 切记：不要弄成死循环咯 ^_^
```

## caller

> 每个函数在 **执行过程中** 都有一个 `caller` 属性  
表示当前函数执行上下文所在的函数，如果没有则返回 `null`

```javascript
(function() {
    function child() {
        // 返回外层的匿名函数，毕竟匿名函数也是函数 ^_^
        console.log(child.caller);
    }
    child();
})();

function parent() {
    function child() {
        // 返回parent函数，因为 child 是在 parent 内部执行的
        console.log(child.caller);
    }
    child();

    // 返回 null，因为 parent 在全局环境执行，并没有「父函数」
    console.log(parent.caller);
};
parent();
```

# 怎么找到 this

> 我对this的定义：拥有当前 **执行上下文**（context）的一个对象  
开发者需要知道的是：当前的 this 是哪一个对象  

## 全局环境中找 this

> 全局函数内部 this 指向 undefined  
在非严格模式中，当 this 指向 undefined 时，它会被自动指向全局对象

```javascript
console.log(this); // 返回：window

function getThis() {
    'use strict';
    console.log(this);
}
getThis(); // 返回：undefined

// 备注：在严格模式下，全局环境 this 为 undefined
// 另外，nodejs 环境下，this 既不是 window 也不是 undefined，开发者可以自行谷歌
```

## 在执行语句前面有点 `•` 、 有明确父级执行对象的情况找 this

> 是谁在执行语句，语句内部的 this 就是谁

```javascript
var father = {
    getThis: function() {
        console.log(this);
    }
};
father.getThis(); // 返回：father 对象（因为是 father 在执行 getThis 函数）

function getThis() {
    'use strict'; // 启动严格模式
    console.log(this);
}
getThis(); // 返回：undefined
window.getThis(); // 返回：window

// 注意下面的情况
var myThis = father.getThis;
myThis();// 返回：window

// 上面二行代码可以理解为：
// 定义个全局（window）myThis 变量，这个变量指向 father.getThis 函数
// 在执行 myThis 时，myThis 的父级执行对象是 window ，所以内部this就为 window
```

## 函数内部的函数找 this

> 函数内部的函数，没有明确的父级执行对象，this 默认绑定到全局

```javascript
var test = {
    getThis: function() {
        var _getThis = function() {
            console.log(this);
        }
        console.log(this);
        _getThis();
    }
}

test.getThis(); // 依次返回：test、window
```

## 存在 call、apply 和 bind 的情况找 this

```javascript
function getThis() {
    console.log(this);
}

getThis(); // 普通情况，返回：window

var Test = { test: 'test' }; // 定义一个对象：Test

getThis.call(Test); // 返回：Test
getThis.apply(Test); // 返回：Test

var myGetThis = getThis.bind(Test);
myGetThis(); // 返回：Test
```

## 有 `new` 关键字的情况找 this

> new一个对象，对象内部的 this 就是当前对象  
new 的权级要高于 bind

```javascript
function getThis() {
    console.log(this);
}

new getThis; // 返回：getThis

var myGetThis = getThis.bind(window);
myGetThis(); // 返回：window
new myGetThis; // 返回：getThis

// 注意上面 2 行代码
// myGetThis 的内部 this 已经绑定了 widnow
// 但是在 new 过后，内部 this 还是 getThis
```

## DOM 绑定事件监听函数找 this

> 在 DOM 事件处理函数中，this 始终指向这个处理函数绑定的 DOM 节点

```javascript
var $dom = document.querySelector('#test');

$dom.addEventListener('click', function() {
    console.log(this === $dom);
});

// 当 $dom 元素被点击的时候，打印：true
```

## ES6的箭头函数找 this

> 函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象  
箭头函数 this 指向的固定化，不是因为箭头函数内部有绑定 this 机制  
而是箭头函数根本没有自己的 this ，导致内部的 this 就是外层代码块的 this  
正是因为它没有 this ，所以箭头函数不能用作构造函数

```javascript
var Test = {
    getThis: function() {
        setTimeout(() => {
            console.log(this);
        }, 100);
    },
    getThis2: function() {
        setTimeout(function() {
            console.log(this);
        }, 100);
    }
}

Test.getThis(); // 返回：Test
Test.getThis2(); // 返回：window

// setTimeout 的回调参数是普通函数（this 在调用时指向全局）
// setTimeout 的回调参数是箭头函数 (this 在定义时指向 Test)
```

# call、apply 和 bind

> 三者都是改变函数内部 this 指向  
区别一：apply 传递参数为数组形式，call 与 bind 为枚举形式  
区别二：call 与 apply 立即执行，而 bind 不是

## Function.prototype.call

```javascript
function getThis() {
    console.log(this);
}

getThis.call(); // 返回：window。严格模式下返回：undefined
getThis.call(null); // 返回：window。严格模式下返回：null
getThis.call(undefined); // 返回：window。严格模式下返回：undefined

var name = 'window',
    obj = {
        name: 'obj'
    };

function getName(_name) {
    console.log(this.name, _name);
}
getName(); // 返回："window", undefined
getName.call(obj, 'call'); // 返回："obj", "call"
```

## Function.prototype.apply

```javascript
var obj = {
        name: 'obj'
    };
function getName(_param1, _param2) {
    console.log(this.name, _param1, _param2);
}
getName.apply(obj, ['apply1', 'apply2']); // 返回："obj", "apply1", "apply2"
```

## Function.prototype.bind

> IE6/7/8 不支持 bind

```javascript
var obj = {
        name: 'obj'
    };
function getName(_param1, _param2) {
    console.log(this.name, _param1, _param2);
}

// 传参方式1：(绑定时传)
var tempGetName1 = getName.bind(obj, 'bind1', 'bind2');
tempGetName1(); // 返回："obj", "bind1", "bind2"

// 传参方式2：（执行时传）
var tempGetName2 = getName.bind(obj);
tempGetName2('bind1', 'bind2'); // 返回："obj", "bind1", "bind2"
```

# 设置元素样式

## setAttribute 方式设置元素样式

```javascript
// 只能用于某些属性（比如 height ）
element.setAttribute('height', 100);
element.setAttribute('height', '100px');

// 直接设置 style 属性
element.setAttribute('style', 'max-height: 100px !important');
element.setAttribute('style', 'height: 100px');
```

## element.style 方式设置元素样式

使用 **element.style** 对象设置样式
某些情况此方法设置 `!important` 值无效  
如果属性有 `-` 号，就写成 `驼峰` 形式  
如果想保留 `-` 号，就 `中括号` 的形式

```javascript
// 驼峰
element.style.maxHeight = '100px';
// 中括号
element.style['max-height'] = '100px';
```

使用 **element.style.setProperty** 方法  
此方式只能用 `中括号` 的形式这是样式

```javascript
// 第三个参数不是必须
element.style.setProperty('max-height', '100px', 'important');
// 注意：下面的方法无效
element.style.setProperty('maxHeight', '100px', 'important');
```

使用 **element.style.cssText** 方法  

```javascript
element.style.cssText = 'height: 100px !important';
element.style.cssText += 'max-height: 100px !important';
```

## className 方式设置元素样式

> 前提是已经定义好一些类名

```javascript
element.className = 'blue';
element.className += 'red pink';
```

## 使用 addRule、insertRule 方式设置元素样式

```javascript
// 在原有样式操作
document.styleSheets[0].addRule('.box', 'height: 100px');
document.styleSheets[0].insertRule('.box {height: 100px}', 0);

// 或者插入新样式时操作
var styleEl = document.createElement('style'),
    styleSheet = styleEl.sheet;

styleSheet.addRule('.box', 'height: 100px');
styleSheet.insertRule('.box {height: 100px}', 0);

document.head.appendChild(styleEl);
```

# IIFE

> IIFE（immediately invoked function expression）  
IIFE 又称为 **自执行函数**、**立即执行函数**  
我们知道函数需要「调用」，才能执行，比如：

```javascript
var func = function(name) { // 定义一个函数
    console.log(name);
};

func('hangyangws'); // 调用函数
```

执行普通的函数需要二个步骤：  
第一，定义；第二，调用  
IIFE函数调用和普通函数一样也需要这二个步骤  
但是，是把这二个步骤合二为一了

```javascript
// 普通函数执行过程：
// 定义一个变量，变量值是一个函数
// 然后在以小括号「()」的形式调用这个变量
// 我们试想一下，把小括号直接写在函数后面不就可以了么，不需要通过一个变量当做「中间组件」
// 比如这样：

function(name) { // 定义一个函数
  console.log(name);
}('hangyangws') // 直接在函数后面写上小括号，调用函数

// 但是，这样是行不通的，会报错
// 因为，前面定义的函数，不是可以自行的对象
// 那么怎么办
// 解决方案就是“强制转换”，把函数强制转换为可执行对象
// 转换方法很多，最常用的还是小括号
// 比如：

(function(name) { // 定义一个函数，并且用小括号包起来，这样就把函数强制转换为可执行对象了
  console.log(name);
})('hangyangws') // 这样就可以安全的调研前面定义的函数

// 然后我突然发现
// 哟喂，竟然可以不写函数名，直接调用一个函数
// 感觉就像函数自己把自己执行了一样
```

其实“强制转换”的方式很多，比如  
```javascript
// 最外层用小括号包起来
(function(name) {
  console.log(name);
}('hangyangws'))

// 用加号（同理们可以用减号）
+ function(name) {
  console.log(name);
}('hangyangws')

// 用非（不建议用）
! function(name) {
  console.log(name);
}('hangyangws')

// 最建议开发者用的方式，还是小括号
```

# 闭包

> 闭包实际上设计一个对象的属性，何时被 gc 处理的问题 闭包和 gc 是相关联的

[link](http://mp.weixin.qq.com/s/kxKon7JsAHAuHQw6OY0T9Q)

# 位运算符

> & |

# 数组相关

## 数组的长度是根据下标的最大而确定的

```javascript
var arr = [];
arr['test'] = 'test'; // 数组的下标可以死字符串
arr.length; // 返回：0 // 字符串下标不计入数组长度
arr[10] = 10;
arr.length; // 返回：11
```

## 手动赋值数组长度可以删减多余元素

```javascript
var arr = [1, 2, 3, 4];
arr.length = 2;
console.log(arr); // 返回：[1, 2]
```

## Array.prototype.reduce()

> reduce中文是减少的意思  
reduce可以作用于把数组累计操作然后返回为一个数

先来个累加的列子，一探究竟

```javascript
var result = [11, 22, 32].reduce(function(count, nowVal) {
    console.log(...arguments);
    return count + nowVal;
});
console.log(result);
// 打印：
// 11, 22, 1, [11, 22, 32]
// 33, 32, 2, [11, 22, 32]
// 6

// 从代码与打印结果分析，
// reduce 方法的第一个参数为函数，函数接受 4 个参数
// 第一个参数「count」为上一次计算 return 的结果，第二个参数 nowVal 为当前的值
// 第三参数为 nowVal 的下标索引，第四个参数为原来的数组
// reduce 方法返回最后一个计算 return 的值
```

我们再看一个和上面的相似的累加例子

```javascript
var result = [11, 22, 33].reduce(function(count, nowVal) {
    console.log(...arguments);
    return count + nowVal;
}, 1);
console.log(result);
// 打印：
// 1, 11, 0, [11, 22, 33]
// 12, 22, 1, [11, 22, 33]
// 34, 33, 2, [11, 22, 33]

// 从代码与打印结果分析，
// reduce 方法还可以接受第二个参数
// 这个参数作为计算初始值
```

## in 操作符

[link](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/in)

# Object.defineProperty

[MDN链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

## 意义与使用场景

> `Object.defineProperty`可以监听某个对象的属性的读写，并且可以自定义相关监听函数  
PS：对象初始化的值会被清空，定义初始值只能在函数内部定义

## 语法

```javascript
Object.defineProperty(objName, propName, descriptor);
```

## 参数

> PS: 数据描述符和存取描述符不能混合使用。比如 get 和 value 不可以共存。

1. objName
    需要定义的属性的对象
1. propName
    需定义或修改的属性的名字
1. descriptor
    将被定义或修改的属性的描述符：

```javascript
{
    configurable: false, // 当且仅当 configurable 为 true 时，当前 propName 才能够被改变，也能够被删除。默认为 false。
    enumerable: false, // 当且仅当 enumerable 为 true 时，当前 propName 才能够出现在对象的枚举属性中。默认为 false。
    value: null, // 当前 propName 对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。这就是解释了为什么：「对象初始化的值会被清空，定义初始值只能在函数内部定义。」
    writable: false // 当且仅当 writable 为 true 时，当前 propName 才能被赋值运算符改变。默认为 false。
    // getter 方法。该方法返回值被用作属性值。默认为 undefined。
    get: function() {
        // 其他的代码…
        return 'self define value'; // 也可以没有返回值，默认为 undefined
    },
    // setter 方法。如果没有 setter 则为 undefined。该方法将接受唯一参数，并将该参数的新值分配给该属性。
    set: function(_val) {
        // 其他的代码…
    }
}
```

## 返回值

返回传入函数的对象，即第一个参数 obj

# Window.URL

# 宽度相关

window.innerWidth 表示网页的宽度，通常和 html、body 元素一样宽  
window.outerWidth 表示浏览器的宽度，通常和包括浏览器的滚动条，开发者工具…  
同理 window.innerHeight、window.innerHeight 是一样的道理  

**注意**：Chrome 浏览器 window.innerWidth 和 window.outerWidth 通常没有区别「因为浏览器左右无边框」

screen.width 表示显示器的分辨率，和浏览器几乎没有关系
