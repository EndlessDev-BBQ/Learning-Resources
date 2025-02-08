# Day 7 -知识回顾 - 流程的切割

## 函数的作用

使用函数切割流程，不仅可以减少重复代码、还可以有效的降低整体复杂度

![函数的作用](https://img.hoocode.com/i/2024/10/28/110nnwa.webp)

## 函数的常见问题

### **如何理解函数的参数、返回值、函数体？**

![理解函数的函数体、参数和返回值](https://img.hoocode.com/i/2024/10/28/110nw5s.webp)

参数：表示完成流程所需的**必要信息**

返回值：表示完成流程后**产生的结果**

函数体：表示具体的流程

**函数的参数、返回值只取决于函数的作用，与函数体无关**

### 为什么我觉得有了函数之后，程序反而变得更复杂了？

函数的核心作用，是为了让某一段复杂的流程变得简单。

如果在函数的帮助下，反而觉得流程变得复杂了，极有可能的原因是开发思想没有做相应的切割，导致思想负担过重。

**始终记住以下两点**：

1. 定义函数时，只需要考虑这个函数如何实现即可，完全不需要考虑其他无关的东西。
2. 调用函数时，只需要考虑向其传递什么参数，如何使用它的返回结果，完全无需考虑函数的具体实现。

函数具有**三要素**：函数名、参数、返回值

只要具备三要素，就能书写函数体；只要具备三要素，就能完成函数调用。

### 学习函数时不知道该如何切割流程怎么办？

要完成一个函数声明，分为两步：

1. 设计函数

   设计函数就是如何切割流程，具体来说就是设计出函数的三要素，这一步是最难的，目前无须同学们掌握，老师会帮你把函数设计好。

2. 书写函数体

   根据设计的三要素完成函数体，这一步就是现阶段练习的重点。

## 练习 1

**完成下面的函数**

```js
/**
 * 得到某个数的阶乘
 * 如果数小于了1，则得到0
 * @param {number} n 要求阶乘的数
 * @return {number} 阶乘结果
 */
function factorial(n) {
  if (n < 1) {
    return 0;
  }
  var res = 1;
  for (var i = 2; i <= n; i++) {
    res *= i; // res = res * i;
  }
  return res;
}

console.log(factorial(0)); // 测试: 0
console.log(factorial(5)); // 测试: 120
```

## 练习 2

**利用上面的函数，完成下面的练习题**

```js
/* 
1. 输出5的阶乘
*/
console.log(factorial(5)); // 120

/* 
2. 求5和6的阶乘之和，然后输出
*/
console.log(factorial(5) + factorial(6)); // 120 + 720 = 840

/* 
3. 输出阶乘结果不超过1000的所有数字
*/

for (var i = 1; i <= 1000; i++) {
  if (factorial(i) <= 1000) {
    console.log(i);
  }
}

/**
 * 1
 * 2
 * 3
 * 4
 * 5
 * 6
 */
```

## 练习 3

**完成下面的函数**

```js
/**
 * 在arr中寻找是否存在target
 * @param {Array} arr 要遍历寻找的数组
 * @param {any} target 要寻找的目标
 * @return {boolean} 是否找到
 */
function includes(arr, target) {
  var isFind = false; // 默认为false
  for (var i = 0; i < arr.length; i++) {
    if (arr[i] === target) {
      isFind = true; // 找到了为true，跳出循环
      break;
    }
  }
  return isFind;
}

console.log(includes([2, 5, 10, 6, 33], 10)); // 测试: true
console.log(includes([2, 5, 10, 6, 33], 8)); // 测试: false
```

## 练习 4

**利用上面的函数，完成下面的练习题**

```js
var nums = [1, 3, 8, 2, 5, 1, 9];
/* 
1. 判断nums中是否存在8，输出是或否
*/
console.log(includes(nums, 8) ? "是" : "否"); // 是

var nums2 = [6, 3, 2, 7, 11, 33];
/* 
2. 判断数字2是否同时存在于nums和nums2中，输出是或否
*/
console.log(includes(nums, 2) && includes(nums2, 2) ? "是" : "否"); // 是

var nums3 = [2, 5, 10];
/* 
3. 思考题：判断nums3中是否所有数字都在nums中存在，输出是或否
*/
/**
 * 逻辑：
 * 利用 inlcudes 函数，遍历 nums3 中的每一个数字，但凡找到一个结果为 false（也就是没有找到）的，就可以直接return false了
 */
var iStatus = true; // 默认找到了
for (var i = 0; i < nums3.length; i++) {
  iStatus = includes(nums, nums3[i]); // 一个个拿去看是否存在
  if (iStatus === false) {
    // 一个数遍历了一遍，不存在
    console.log("否");
    break;
  }
}
if (iStatus === true) {
  console.log("是");
}

// 否
```
