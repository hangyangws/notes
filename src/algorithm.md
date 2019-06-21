# 算法

### DP 动态规划

```javascript
/**
 * 给出一些具有不同面额的硬币，例如 [1, 2, 5]，并测试他们是否可以弥补一定的数量「N」，
 * 假设你在每种面额上使用无限数量的硬币。
 * 例如：
 * 如果 coin = [1, 2, 5] 且 N = 11，则返回 true；
 * 如果 coin = [3, 77] 且 N = 100，则返回 false。
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
        cost = Math.min(cost, number[index - selectArray[index2]] + 1);
      }
    }

    number[index++] = cost;
  }

  return number;
};

const isFill = (selectArray, resultNumber) => {
  const minNumberArray = getMinNumberArray(selectArray, resultNumber);

  return minNumberArray[resultNumber] !== Infinity;
};

console.log(isFill([1, 2, 5], 1)); // true
console.log(isFill([3, 77], 100)); // false
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
