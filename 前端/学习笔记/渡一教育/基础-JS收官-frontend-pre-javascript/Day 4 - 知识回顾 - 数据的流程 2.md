# Day 4 - 知识回顾 - 数据的流程 2

练习 1 ：

**1~100 求和**

```js
var sum = 0;
for (var i = 1; i <= 100; i++) {
  sum += i;
}
console.log(sum);
```

**结果**

```bash
5050
```

练习 2 ：

**求某个数的阶乘**

```js
var num = 5;
var res = 1;
if (num === 0) {
  res = 1;
} else {
  for (var i = num; i > 0; i--) {
    // 阶乘：5 * 4 * 3 * 2 * 1
    res *= i;
  }
}
console.log(res);
```

**结果**

```bash
120
```

练习 3 ：

**数组求和**

```js
var nums = [213, 3123, 4342, 121];
var sum = 0;
for (var i = 0; i < nums.length; i++) {
  sum += nums[i];
}

console.log(sum);
```

**结果**

```bash
7799
```

练习 4 ：

**求数组中的奇数的个数**

```js
var nums = [213, 3123, 4342, 121];
var oddNum = 0;
for (var i = 0; i < nums.length; i++) {
  if (nums[i] % 2 !== 0) {
    oddNum++;
  }
}

console.log(oddNum);
```

**结果**

```bash
3
```

练习 5 ：

**求数组中的奇数和**

```js
var nums = [213, 3123, 4342, 121];
var sum = 0;
for (var i = 0; i < nums.length; i++) {
  if (nums[i] % 2 !== 0) {
    sum += nums[i];
  }
}
console.log(sum);
```

**结果**

```bash
3457
```

练习 6 ：

**数组中是否存在某个数，输出 是 或 否**

```js
var num = 10;
var arr = [32, 23, 343, 67, 85, 234];
var result = false;
for (var i = 0; i < arr.length; i++) {
  if (arr[i] === num) {
    result = true;
    break;
  }
}
console.log(result ? "是" : "否");
```

**结果**

```bash
否
```

练习 7 ：

**数组中是否存在某个数，如果存在，则输出它所在的下标，如果不存在，则输出-1**

```js
var num = 10;
var arr = [32, 23, 343, 67, 85, 234];
var index = -1;
for (var i = 0; i < arr.length; i++) {
  if (arr[i] === num) {
    index = i;
    break;
  }
}
console.log(index);
```

**结果**

```bash
-1
```

**输出该数所存在的下标**

```js
var num = 10;
var arr = [10, 32, 10, 343, 10, 85, 234];
var index = []; // 用数组来存放下标
for (var i = 0; i < arr.length; i++) {
  if (arr[i] === num) {
    index.push(i);
  }
}
console.log(index);
```

**结果**

```bash
[ 0, 2, 4 ]
```

练习 8 ：

**找到数组中第一个奇数和最后一个奇数，将它们求和**

```js
// 两遍循环
var arr = [32, 24, 344, 68, 85, 234];
var sum = null;
for (var i = 0; i < arr.length; i++) {
  if (arr[i] % 2 !== 0) {
    sum += arr[i];
    break;
  }
}
for (var i = arr.length - 1; i >= 0; i--) {
  if (arr[i] % 2 !== 0) {
    sum += arr[i];
    break;
  }
}
console.log(sum);

// 一遍循环
var arr = [32, 24, 344, 67, 85, 234];
var leftIndex = -1;
var rightIndex = -1;
var sum = 0;
for (var i = 0; i < arr.length; i++) {
  // 如果是 奇数 并且 是 第一个奇数 ===> 记录左、右下标，右下标也记录是因为可能数组里只有一个奇数
  if (arr[i] % 2 !== 0 && leftIndex === -1) {
    leftIndex = i;
    rightIndex = i;
  } else if (arr[i] % 2 !== 0 && leftIndex !== -1) {
    // 如果是 奇数 但 不是 第一个奇数 ===> 记录右下标
    rightIndex = i;
  }
}
console.log(arr[leftIndex] + arr[rightIndex]);
```

**结果**

```bash
170
```

练习 9 ：

**有两个数组，看两个数组中是否都存在奇数，输出 是 或 否**

```js
var arr1 = [32, 24, 344, 84, 234];
var arr2 = [32, 24, 344, 68, 84];
var arr1Has = false;
var arr2Has = false;

for (var i = 0; i < arr1.length; i++) {
  // 存在奇数
  if (arr1[i] % 2 !== 0) {
    arr1Has = true;
    break;
  }
}

for (var i = 0; i < arr2.length; i++) {
  // 存在奇数
  if (arr2[i] % 2 !== 0) {
    arr2Has = true;
    break;
  }
}

console.log(arr1Has && arr2Has ? "是" : "否");
```

**结果**

```bash
否
```
