# 神奇的正则

> 提示：正则表达式有个属性：`lastIndex` ，表示从字符串的哪一个下标开始匹配，默认为 `0`

## RegExp.prototype.exec

> exec 返回的是数组，如果没有匹配返回为 null（注意：不是空数组）  
正则有个属性：lastIndex（默认为 0 ），只有在全局匹配模式才改变

```javascript
var str = 'xxabxxabbxx';

/**
 * 普通正则的情况
 */

var reg = /ab*/;
reg.lastIndex; // 返回：0
// 从下标 0 开始匹配，找到「ab」即返回
reg.exec(str); // 返回：["ab"]
// 因为正则没有定义全局匹配，所以 lastIndex 不会改变，依旧为 0
reg.lastIndex; // 返回：0

/**
 * 带有子元素正则情况
 */

var reg = /a(b*)/;
reg.lastIndex; // 返回：0

// 从下标 0 开始匹配，找到「ab」，数组存入第一个元素
// 根据「(b*)」再找到「b」，数组存入第二个元素
reg.exec(str); // 返回：["ab", "b"]

// 因为正则没有定义全局匹配，所以 lastIndex 不会改变，依旧为 0
reg.lastIndex; // 返回：0

/**
 * 带有子元素且全局配备的情况
 */

var reg = /a(b*)/g;
reg.lastIndex; // 返回：0

// 从下标 0 开始匹配，找到「ab」，数组存入第一个元素
// 根据「(b*)」找到「b」，数组存入第二个元素
reg.exec(str); // 返回：["ab", "b"]

// 因为正则定义了全局匹配，所以 lastIndex 改为下一次开始匹配的下标
reg.lastIndex; // 返回：4

// 从下标 4 开始匹配，找到「abb」，数组存入第一个元素
// 根据「(b*)」找到「bb」，数组存入第二个元素
reg.exec(str); // 返回：["abb", "bb"]
```

## RegExp.prototype.test

> test方法执行一个检索，用来查看正则表达式与指定的字符串是否匹配，返回 true 或 false  
test类似于 `String.prototype.search()` 方法  
差别在于 test 返回一个布尔值，而 search 返回索引（如果找到）或者 -1（如果没找到)

```javascript
var str = 'xxabxxabbxx';
var reg = /ab*/;
reg.test(str); // 返回：true

// 注意：
/undefined/.test(); // 返回：true
/undefined/.test('undefined'); // 返回：true
```

## String.prototype.match

> 正则没有 `g` 标志，str.match() 返回和 RegExp.exec() 相同  
正则没有 `g` 标志，返回的 Array 有一个 `input` 属性（解析的原始字符串）  
正则没有 `g` 标志，返回的 Array 有一个 `index` 属性（匹配结果在原字符串中的索引「以 0 开始」）  
正则有 `g` 标志，match 返回的是数组，如果没有匹配返回为 null（注意：不是空数组）

```javascript
// 没有 g 的列子
var str = 'OK abc 1.2.3',
    reg = /abc (\d+(\.\d)*)/i;

console.log(str.match(reg));
// 返回：
// [
//    "abc 1.2.3", // 整个匹配结果
//    "1.2.3",     // 被 (\d+(\.\d)*) 捕获
//    ".3",        // 被 (\.\d) 捕获的最后一个值
//    "index",     // 值为：3。 整个匹配从 0 开始的索引
//    "input"      // 值为：'OK abc 1.2.3'。被解析的原始字符串
// ]

// 有g的列子
var str = 'ABCDabcd',
    reg = /[A-C]/gi;
console.log(str.match(reg));
// 返回：["A", "B", "C", "a", "b", "c"]
```

> 如果 match 参数为非 RegExp 对象，则会隐式地使用 new RegExp(obj) 将其转换为一个 RegExp  
如未提供参数，那么你会得到一个包含空字符串的 Array ：[""]

```javascript
var str = "+Infinity 10";
str.match(Infinity); // 返回：["Infinity"]
str.match(+10); // 返回：["10"]
str.match(); // 返回：[""]
```

## String.prototype.search

> 如果 match 参数为非 RegExp 对象，则会隐式地使用 new RegExp(obj) 将其转换为一个 RegExp  
如果匹配成功，则 search() 返回正则表达式在字符串中首次匹配项的索引。否则，返回 -1
