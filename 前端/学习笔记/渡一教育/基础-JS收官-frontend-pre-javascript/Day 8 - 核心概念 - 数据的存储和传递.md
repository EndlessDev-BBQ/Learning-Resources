# Day 8 - 核心概念 - 数据的存储和传递

## 值 与 引用 (一图理清)

![值与引用 - 一图理清](https://img.hoocode.com/i/2024/10/29/12vtcqk.webp)

### 拆分

**值类型与引用类型**

![值类型与引用类型](https://img.hoocode.com/i/2024/10/29/12vsph5.webp)

**情景 1**

![情景1](https://img.hoocode.com/i/2024/10/29/12vu5mn.webp)

**情景 2**

![情景2](https://img.hoocode.com/i/2024/10/29/12vrzwc.webp)

**情景 3**

![情景3](https://img.hoocode.com/i/2024/10/29/12vseos.webp)

**情景 4**

![情景4](https://img.hoocode.com/i/2024/10/29/12vtxyu.webp)

## 练习题

### 练习 1

**交换两个变量的值**

```js
/**
 * @param {*} a 变量1
 * @param {*} b 变量2
 */
function swap(a, b) {
  var temp = a;
  a = b;
  b = temp;
}

var n1 = 0;
var n2 = 7;

swap(n1, n2);

console.log(n1, n2);

/**
 * 以上方法无效，此题无解
 */
```

**流程图**

![javascript-basic-day-08-练习1](https://img.hoocode.com/i/2024/10/31/p5abl0.webp)

### 练习 2

**交换对象两个属性的值**

```js
/**
 * @param {Object} obj 对象
 * @param {string} key1 属性名1
 * @param {string} key2 属性名2
 */
function swap(obj, key1, key2) {
  var temp = obj[key1];
  obj[key1] = obj[key2];
  obj[key2] = temp;
}

var stu = {
  roomId: 43,
  roomIdChange: 16
};

swap(stu, "roomId", "roomIdChange");

console.log(stu); // { roomId: 16, roomIdChange: 43 }
```

**流程图**

![javascript-basic-day-08-练习2](https://img.hoocode.com/i/2024/10/31/p59yv4.webp)

### 练习 3

**交换数组两个位置的值**

```js
/**
 * @param {Array} arr 数组
 * @param {number} i1 下标1
 * @param {number} i2 下标2
 */
function swap(arr, i1, i2) {
  var temp = arr[i1];
  arr[i1] = arr[i2];
  arr[i2] = temp;
}

var arr = [1, 10];

swap(arr, 0, 1);

console.log(arr); // [ 10, 1 ]
```

**流程图与练习 2 一致**

### 练习 4

**修改对象，仅保留需要的属性**

```js
/**
 * 查找 target 是否在 obj 里存在
 * @param {Array} obj
 * @param {*} target
 * @returns {Boolean}
 */
function includes(obj, target) {
  var isFind = false;
  for (var i = 0; i < obj.length; i++) {
    if (obj[i] === target) {
      isFind = true;
      break;
    }
  }
  return isFind;
}

/**
 * 修改对象，仅保留需要的属性
 * @param {Object} obj 要修改的对象
 * @param {Array<string>} keys 需要保留的属性名数组
 */
function pick(obj, keys) {
  for (var key in obj) {
    if (!includes(keys, key)) {
      // 如果在 keys 里不包含 key 键
      delete obj[key]; // 删掉这个属性
    }
  }
}

var obj = {
  a: 1,
  b: 2,
  c: 3,
  d: 4
};

pick(obj, ["a", "c"]);

console.log(obj); // {a: 1, c: 3}
```

**流程图**

![javascript-basic-day-08-练习4](https://img.hoocode.com/i/2024/10/31/p5a42p.webp)

## 面试题

### 面试题 1

```js
// 下面代码输出什么？
var foo = {
  n: 0,
  k: {
    n: 0
  }
};
var bar = foo.k;
bar.n++;
bar = {
  n: 10
};
bar = foo;
bar.n++;
bar = foo.n;
bar++;
console.log(foo.n, foo.k.n);
```

```bash
答：1 1
```

**流程图**

![javascript-basic-day-08-面试题1](https://img.hoocode.com/i/2024/10/31/ucqvce.webp)

### 面试题 2

```js
// 下面的代码输出什么（京东）？
var foo = {
  n: 1
};

var arr = [foo];

function method1(arr) {
  var bar = arr[0];
  arr.push(bar);
  bar.n++;
  arr = [bar];
  arr.push(bar);
  arr[1].n++;
}
function method2(foo) {
  foo.n++;
}
function method3(n) {
  n++;
}
method1(arr);
method2(foo);
method3(foo.n);

console.log(foo.n, arr.length);
```

```bash
答：4 2
```

**流程图**

![javascript-basic-day-08-面试题2](https://img.hoocode.com/i/2024/10/31/ucr2pl.webp)

### 面试题 3

```js
// 下面的代码输出什么（字节）？
var foo = { bar: 1 };
var arr1 = [1, 2, foo];
var arr2 = arr1.slice(1);
arr2[0]++;
arr2[1].bar++;
foo.bar++;
arr1[2].bar++;
console.log(arr1[1] === arr2[0]);
console.log(arr1[2] === arr2[1]);
console.log(foo.bar);
```

```bash
答：false true 4
```

**流程图**

![javascript-basic-day-08-面试题3](https://img.hoocode.com/i/2024/10/31/ucqwg4.webp)
