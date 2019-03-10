# 算法

### 字符串反转

```javascript
// 计算反转函数多次执行时间所用时间
const getReverseTime = (func, times = 9999) => {
  const str = 'abcdefghijklmnopqrstuvwxyz1234567890~!@#$%^&*()_+=-[]{};:,./<>?'
  const execName = `${func.name} - ${times}`
  console.time(execName)
  new Array(times).fill(undefined).forEach(() => {
    func(str)
  })
  console.timeEnd(execName)
}

const isString = str => typeof str === 'string'

// 利用数组的 reverse 方法
const stringReverse_1 = str =>
  isString(str)
    ? str.split('').reverse().join('')
    : ''

// 利用数组的 reverse 方法「ES6 版本」
const stringReverse_2 = str =>
  isString(str)
    ? [...str].reverse().join('')
    : ''

// 递归方法「左边主体」
const stringReverse_3 = str => {
  const reverse = str => {
    const length = str.length - 1

    return length > 0
      ? `${str[length]}${reverse(str.slice(0, length))}`
      : str
  }

  return isString(str)
    ? reverse(str)
    : ''
}

//  递归方法「右边主体」
const stringReverse_4 = str => {
  const reverse = str => str && `${reverse(str.slice(1))}${str[0]}`

  return isString(str)
    ? reverse(str)
    : ''
}

// 字符串倒序相加
const stringReverse_5 = str => {
  const reverse = str => {
    let length = str.length
    let tempStr = ''
    while(length--) {
      tempStr += str[length]
    }
    return tempStr
  }

  return isString(str)
    ? reverse(str)
    : ''
}

// 字符串倒序相加「数组版」
const stringReverse_6 = str => {
  const reverse = str => {
    let length = str.length
    const tempStr = []
    while(length--) {
      tempStr.push(str[length])
    }
    return tempStr.join('')
  }

  return isString(str)
    ? reverse(str)
    : ''
}

// 测试

getReverseTime(stringReverse_1)
getReverseTime(stringReverse_2)
getReverseTime(stringReverse_3)
getReverseTime(stringReverse_4)
getReverseTime(stringReverse_5)
getReverseTime(stringReverse_6)
```
