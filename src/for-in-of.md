# for-in 与 for-of

for-in 和 for-of 都是迭代一些数据，它们的主要区别是迭代方式的不同。

for-in 迭代对象的 [可枚举属性](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)。  
注意：是迭代对象的 **属性**。

比如：

```javascript
const iterableArray = ['one']

iterableArray.__proto__.two = 'two'

iterableArray.three = 'three'

for(const prop in iterableArray) {
  console.log(prop)
}
// 打印结果：
// 0
// three
// two
```

for-of 遍历 [可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators#Iterables) 要迭代的数据。  
注意：是迭代对象的 **数据**。

比如：

```javascript
const iterableArray = ['one']

iterableArray.__proto__.two = 'two'

iterableArray.three = 'three'

for(const prop of iterableArray) {
  console.log(prop)
}
// 打印结果：
// one
```
