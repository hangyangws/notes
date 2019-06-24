# 算法

### DP 动态规划

```javascript
/**
 * 假设您是个土豪，身上带了足够的1、5、10、20、50、100元面值的钞票。
 * 现在您的目标是凑出某个金额w，需要用到尽量少的钞票。
 * 例如输入：[1, 5, 11] 和 15
 * 得到凑出 15 最少需要 3 张 5 元的钞票。
 */

/**
 * 返回一个数「resultNumber」最少使用多少个候选的数「selectArray」拼凑而成
 * @param {Array} selectArray 候选的数字列表
 * @param {int} resultNumber 需要拼凑的数字
 */
const getMinNumberArray = (selectArray, resultNumber) => {
  const number = [0];

  let index = 1;
  while (index <= resultNumber) {
    let cost = Infinity;

    let index2 = selectArray.length;
    while (index2--) {
      if (index - selectArray[index2] >= 0) {
        cost = Math.min(cost, number[index - selectArray[index2]]);
      }
    }

    number[index++] = cost === Infinity ? null : cost + 1;
  }

  return number;
};

const getStringArray = (selectArray, resultNumber) => {
  const number = [0];
  const path = [''];

  let index = 1;
  while (index <= resultNumber) {
    let cost = Infinity;
    let eachPath = '';

    let index2 = selectArray.length;
    while (index2--) {
      if (index - selectArray[index2] >= 0) {
        if (number[index - selectArray[index2]] + 1 < cost) {
          const preStr = path[index - selectArray[index2]]
            ? path[index - selectArray[index2]] + '、'
            : '';
          eachPath = `${preStr}${selectArray[index2]}`;
        }

        cost = Math.min(cost, number[index - selectArray[index2]] + 1);
      }
    }

    path[index] = eachPath;
    number[index++] = cost === Infinity ? null : cost;
  }

  return path;
};

console.log(getMinNumberArray([1, 5, 11], 15));
// 返回：[0, 1, 2, 3, 4, 1, 2, 3, 4, 5, 2, 1, 2, 3, 4, 3]
// 数组下标 15 的 3 表示需要 3 张钱
console.log(getStringArray([1, 5, 11], 15));
// 返回： 
// ["", "1", "1、1", "1、1、1", "1、1、1、1", "5", "1、5", "1、1、5", "1、1、1、5", "1、1、1、1、5", "5、5", "11", "1、11", "1、1、11", "1、1、1、11", "5、5、5"]
// 数组下标 15 的 "5、5、5" 表示需要 3 张 5 元

```

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


### 获取最长子串长度

```javascript
const getStrMaxSubLength = (str) => {
  if (typeof str !== 'string') {
    return;
  }

  const strLeng = str.length;
  if (strLeng < 2) {
    return strLeng;
  }

  const temp = {};
  let head = 0; // 检测子串头部指针
  let maxLength = 0;

  let index = 0;
  while(index < strLeng)  {
    if (temp[str[index]]) { // 存在
      for(;;) {
        if (str[head] === str[index]) {
          head++;
          break;
        }
        temp[str[head]] = false;
        head++;
      }
    } else { // 不存在
      temp[str[index]] = true;
    }

    maxLength = Math.max(maxLength, index - head + 1);
    index++;
  }

  return maxLength;
};
```

### 统计一个字符串出现频率最高的字母/数字

```javascript
const getMaxChar = (str) => {
  const temp = {};
  let max = 0;
  let maxValue = null;

  let index = 0;
  while(index < str.length) {
    if (temp[str[index]]) {
      temp[str[index]]++;
    } else {
      temp[str[index]] = 1;
    }

    if (temp[str[index]] > max) {
      max = temp[str[index]];
      maxValue = str[index];
    }

    index++;
  }

  return {
    max,
    maxValue
  };
};
```

### 数组去重

```javascript
const uniqueArr = (arr) => {
  return arr.filter((val, index) => {
    return arr.findIndex(item => item === val) === index;
  });
};
```

### 节流函数

```javascript
// 一定时间最多运行一次
const throttle = (fn, time) => {
  const timer = null;

  return (...res) => {
    if (!timer) {
      fn(...res);
      timer = setTimeout(() => {
        timer = null;
      }, time);
    }
  };
};
```

### 防抖函数

```javascript
// 短时间内连续触发的最算最后一次
const debunce = (fn, time) => {
  const timer = null;

  return (...res) => {
    if (timer) {
      clearTimeout(timer);
    }

    timer = setTimeout(() => {
      fn(...res);
    }, time);
  };
};
```
