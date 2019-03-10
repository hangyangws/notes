# fetch

> fetch 提供一个获取资源的接口(包括跨网络)  
fetch 可看做 ES6 对 `XMLHttpRequest` 的升级方法  

fetch 请求默认不带 cookie ，需设置 fetch(url, {credentials: 'include'})  
当服务器返回`400、500`状态码时并不会 reject  
只有网络错误这些导致请求不能完成时， fetch 才会被 reject

# import

# super

[link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super)

# class

[link](https://github.com/ruanyf/es6tutorial/blob/a5ed53c5399c14cfaea4ca7e97957b999fba4807/docs/class.md)  
[link](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)  
[link](http://es6.ruanyifeng.com/#docs/class#Class 基本语法)

ES6 明确规定， Class 内部只有静态方法，没有静态属性  
ES7 有一个静态属性的提案，目前 Babel 转码器支持

```JS
class Foo {}

Foo.prop = 1
Foo.prop // 1
//上面的写法为 Foo 类定义了一个静态属性 prop

// 以下两种写法都无效
class Foo {
  // 写法一
  prop: 2

  // 写法二
  static prop: 2
}

Foo.prop // undefined
```

# rest 参数

> rest 参数，形式为「...变量名」，用于获取函数多余参数

## 一个 Demo

```javascript
const restFunction = (...rest) => {
  console.log(rest)
}

restFunction(1, 2)
// 打印：[1, 2]
```

## 注意

1. rest 参数只能放在形参的最后一位
1. 箭头函数中，单个 rest 参数不能省略小括号

比如：

```javascript
const restFunction = (...rest, nextParam) => {}
// 报错：Rest parameter must be last formal parameter

const restFunction = ...rest => {}
// 报错：Unexpected token ...
```

# 扩展运算符「spread」

扩展运算符，看起来和 [rest 参数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) 外观相似，也是三个点「...」，  
不过和 rest 参数功能可是不一样呢

扩展运算符号，分二种情况

1. iterable「一般情况为数组」
1. enumerable「一般情况为对象」

在标准的 ES2015 中，只有针对 iterable 数据实现了扩展运算符  
它把 iterable 数据的数据序列转换为用 **逗号分割的参数序列**

比如：

```javascript
let array = [5, 12]
let arrayCopy = [11, ...array]
// 此行代码类似于：let arrayCopy = [11, 5, 12]
// arrayCopy ==> [11, 5, 12]

console.log(...[5, 12])
console.log(5, 12)
// 上面 2 行代码意义一样
// 输出结果都是是：5 12
```

经过上面的 2 个列子，  
应该能更好的理解「扩展运算符的结果是 **逗号分隔的参数序列**」的含义

不过有个需要注意的点：  
非 iterable 数据执行扩展运算符，会报错

在 ES7 的 [某个提案](https://github.com/tc39/proposal-object-rest-spread) 中，  
讲扩展运算符引入 enumerable 数据

比如：

```javascript
let obj = {name: 'hangyangws'}
let objCopy = {...obj}
console.log(objCopy) // 输出：{name: 'hangyangws'}
```

其实 enumerable 数据的扩展运算符底层实现是利用了 [Object.assign](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

Object.assign(target, ...sources) 我们比较熟悉，有 2 个特点：

1. sources 参数如果是「原始类型」会被包装为「对象」
1. sources 参数如果是 null 和 undefined 会被忽略

比如：

```javascript
Object.assign({}, null) // 结果为：{}
Object.assign({}, undefined) // 结果为：{}

Object.assign({}, 0) // 结果为：{}
Object.assign({}, 'FJ') // 结果为：{0: "F", 1: "J"}
// 所以有个点可以注意一下：
// 只有字符串的包装对象才可能有自身可枚举属性
// 对于「数字」，结果和 null、undefined 类似
```

既然扩展运算符有 2 种情况，那么 JS 解释器怎么知道使用哪一种？  
所以扩展运算符会根据代码的具体的 **执行上下文** 判断

比如：

```javascript
let test = [...null]
// 报错：null is not iterable

let test = {...null}
// test ===> {}
```
