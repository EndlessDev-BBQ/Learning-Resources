# Day 9 - 核心概念 - 数据的作用域

## JS 的作用域：全局作用域 & 函数作用域

**什么是全局作用域？**

```js
/**
 * 直接写的变量定义、函数声明，它的作用范围就在全局
 */
var a;
function b() {}
var c = function () {}; // 将函数表达式赋值给变量c
c();
```

**什么是函数作用域？**

```js
function d() {
  var e; // 变量e在函数d中声明，它的作用域就在函数d中
}
```

> 小贴士：作用域在哪，关键在于变量、函数是在哪里定义的。在 JS 里只有全局作用域和函数作用域。

**Q: 这种情况下，a 的作用域在哪？**

```js
if (xxx) {
  var a;
}
```

```bash
答： a 是全局作用域。因为在 JS 里只有全局作用域和函数作用域。
```

### 1 - 内部的作用域能访问外部，反之不行

```js
/**
 * 内部访问外部
 */
var a = 1;
function b() {
  a++;
}
b();
console.log(a); // 2
```

```js
/**
 * 外部访问内部
 */
function b() {
  var c = 1;
}
console.log(c); // c is not defined
```

### - 访问时从内向外依次查找

```js
var a = 1;
function b() {
  var a = 2;
  a++; // 这里的 a 找的是函数 b 内部的 a
  console.log("内部的 a：" + a); // 3
}
b();
console.log("外部的 a：" + a); // 1
```

```js
function b(a) {
  // 这里的参数 a 也是定义在内部的
}
```

### 2 - 如果在内部的作用域中访问了外部，则会产生闭包

简单理解

```js
/**
 * 以这里为例：
 * 内部访问外部
 */
var a = 1;
function b() {
  a++; // 这里 b 访问了外部的 a，产生了闭包 —— 就这么理解（详细在笔面试里会讲）
}
b();
console.log(a); // 2
```

### 3 - 内部作用域能访问的外部，取决于函数定义的位置，和调用无关

```js
var a = 1;
function m1() {
  a++;
}
function m2() {
  var a = 3;
  m1(); // 考点：用的是全局的 a 还是 m2 函数里的 a ？
  console.log(a);
}
m2();
console.log(a);
```

```bash
答：  根据知识点：内部作用域能访问的外部，取决于函数定义的位置，和调用无关。
知  m1 定义的位置是在全局作用域里，故其函数内部调用的 a 是全局的 a，
所  m2 函数内部打印出的是：3；全局作用域打印出的是：2
```

## 作用域内定义的变量、函数声明会提升到作用域顶部 - JS 的特点

举个例子 **变量的定义**

```js
console.log(a); // undefined
var a = 10;
```

为什么输出 undefined？而不是报错？因为上面的代码就相当于

```js
var a;
console.log(a); // undefined
a = 10;
```

这就是 JS 的特点

**函数的声明也如此**

```js
console.log(a); // [Function: a]
function a() {}
```

相当于

```js
function a() {}
console.log(a); // [Function: a]
```

**附加一点**

```js
console.log(b); // undefined
var b = function () {};
```

相当于

```js
var b;
console.log(b); // undefined
b = function () {};
```

这里的 `var b = function () {};` 是指将函数表达式的结果赋值给 b 。

### 小练习

```js
/**
 * 问：输出什么？
 */
function m() {
  console.log(a, b, c);
  var a = 1;
  function b() {}
  var c = function () {};
}
m();
```

```bash
答：  undefined [Function: b] undefined
```

相当于

```js
function m() {
  var a;
  function b() {}
  var c;
  console.log(a, b, c);
  a = 1;
  c = function () {};
}
m();
```

## 面试题

### 面试题 1

```js
// 下面的代码输出什么
console.log(a, b, c);
var a = 1;
var b = function () {};
function c() {}
```

```bash
答： undefined undefined [Function: c]
```

相当于

```js
var a;
var b;
function c() {}
console.log(a, b, c);
a = 1;
b = function () {};
```

### 面试题 2

```js
// 下面的代码输出什么
var a = 1,
  b = 2;
function m1() {
  console.log(a);
  var a = 3;
  function m2() {
    console.log(a, b);
  }
  m2();
}
m1();
```

```bash
答： undefined 3 2
```

相当于

```js
var a = 1,
  b = 2;
function m1() {
  var a;
  // 疑问?
  function m2() {
    console.log(a, b); // 3 2
  }
  console.log(a); // undefined
  a = 3;
  m2();
}
m1();
```

> 疑问：为什么这里的 m2 不是打印的 undefined 2，而是 3 2？
> 答：因为这里只是创建出 m2 这个函数，还没有调用它。当 m2 在 a = 3 之后调用自己这个函数的时候，a 就已经赋值了。

直观感受下

```js
var a = 1,
  b = 2;
function m1() {
  var a;
  function m2() {
    console.log(a, b); // undefined 2
  }
  console.log(a); // undefined
  m2(); // 调换位置
  a = 3;
}
m1();
```

> 这里 m2 在 a 被赋值之前就调用自己这个函数了，然后根据 **作用域内定义的变量、函数声明会提升到作用域顶部 - JS 的特点** ，此时 a 还只是被定义的状态，所以在函数内部打印 a 的值为 undefined。

### 面试题 3

```js
// 下面的代码输出什么？(百度)
var a = 1;
function m1() {
  a++;
}
function m2() {
  var a = 2;
  m1();
  console.log(a);
}
m2();
console.log(a);
```

```bash
答： 2 2
```

相当于

```js
var a = 1;
function m1() {
  a++; // 这里的 a 找的是全局作用域里的 a
}
function m2() {
  var a = 2;
  m1(); // m1 定义在全局作用域里，里面访问的是全局的 a
  console.log(a); // 这里遵循“由内而外找寻”，找的 a 是 m2 函数里的 a，是函数作用域的 a。
}
m2();
console.log(a); // 这里找的是全局作用域里的 a。
```

# 核心概念 - 全局对象

## 全局对象

无论是浏览器环境，还是 node 环境，都会提供一个全局对象

**浏览器环境：window**

![浏览器全局对象window-console控制台打印结果](https://img.hoocode.com/i/2024/10/30/12i4f1y.webp)

**node 环境：global**

![node环境全局对象global-console控制台打印结果](https://img.hoocode.com/i/2024/10/30/12i4pvw.webp)

##### 全局对象有下面几个特点

1. 全局对象的属性可以被直接访问

![window-console全局对象的属性可以被直接访问](https://img.hoocode.com/i/2024/10/30/12i4tnb.webp)

2. 给未声明的变量赋值，实际就是给全局对象的属性赋值

![给未声明的变量赋值-实际就是给全局对象的属性赋值](https://img.hoocode.com/i/2024/10/30/12i49zj.webp)

> 小贴士：永远别这么干

3. 所有的全局变量、全局函数**都会**附加到全局对象

什么意思？举个例子

> File
>
> |---- index.html
>
> |---- 1.js
>
> |---- 2.js

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- 引入 -->
    <script src="./1.js"></script>
    <script src="./2.js"></script>
  </body>
</html>
```

`1.js`

```js
var a = 1;
function b() {
  console.log("b");
}
var c = function () {
  console.log("c");
};
```

此时运行 html 文件，在控制台可以看到

![所有的全局变量、全局函数都会附加到全局对象-1](https://img.hoocode.com/i/2024/10/30/12pyi8h.webp)

这样做有什么意义呢？答：可以在 2.js 里调用。因为此时这些变量、函数都已是全局对象的属性了，可以直接调用。

`2.js`

```js
console.log(a);
console.log(b);
console.log(c);
b();
c();
```

在控制台可以看到

![所有的全局变量、全局函数都会附加到全局对象-2](https://img.hoocode.com/i/2024/10/30/12shhbt.webp)

> 这种现象称之为全局污染，又称之为全局暴露，或简称污染、暴露
>
> 如果要避免污染，需要使用**立即执行函数**改变其作用域
>
> 立即执行函数又称之为 IIFE，它的全称是 Immediately Invoked Function Expression。最本质的作用就是改变其作用域
>
> **IIFE 通常用于强行改变作用域**

##### 立即执行函数

**思考：** 如何避免污染？

简单粗暴的方法：**改变作用域**

假设不想让 a 暴露，那么就给它放到一个函数里

`1.js`

```js
function init() {
  var a = 1; // 不希望污染全局
}
init();
```

**问题：** 这个函数 init 同样也暴露了。多此一举。

**提升：** 声明一个函数，然后不要给它赋值，而是立即执行。

`1.js`

```js
// 声明了一个匿名函数，声明完就立即执行它。
(function () {
  var a = 1;
})();
```

`2.js` 就接收不到了

```js
console.log(a);
/**
 * Uncaught ReferenceError: a is not defined
    at 2.js:1:13
 */
```

**思考：** 如果要在匿名函数内部暴露其他的变量、函数，又该如何操作？

`1.js`

```js
(function () {
  var a = 1;
  var b = 2;
  // 我要暴露 c
  function c() {
    console.log(a + b);
  }
  c();
})();
```

写成原来的格式

`1.js`

```js
var init = function () {
  var a = 1;
  var b = 2;
  // 我要暴露 c
  function c() {
    console.log(a + b);
  }
  c();
  return c; // 这里只要 return 就可以把 c 暴露出去
};

console.log(init());
/** 打印结果
  3
  1.js:14 ƒ c() {
      console.log(a + b);
    }
 */

console.log(init);
/** 打印结果
  ƒ () {
    var a = 1;
    var b = 2;
    function c() {
      console.log(a + b);
    }
    c();
    return c;
  }
 */
```

**小贴士：** 这里 return 的是 c 函数的声明，所以 init() 打印出来的是 c 函数的声明，init()() 打印的才是 c 函数的执行。

**提升：** 写回匿名函数的形式

`1.js`

```js
var abc = (function () {
  var a = 1; // 隐藏
  var b = 2; // 隐藏

  // 暴露 c
  function c() {
    console.log(a + b);
  }

  c();
  return c; // 整个函数执行结束，返回 c 函数的声明。这里就把 c 暴露出去了，由 abc 这个变量接收。
})();
```

`2.js`

```js
(function () {
  console.log(abc); // abc 是全局变量，故可以调用。而 abc 又能接收到 c 函数的声明，故实现了 c 函数的暴露。
})();
/** 打印结果
 3
 ƒ c() {
      console.log(a + b);
  }
 */
```

**思考：** 如果要暴露多个变量、函数，该如何操作？

`1.js`

```js
var abc = (function () {
  var a = 1; // 隐藏
  var b = 2; // 隐藏

  // 暴露
  function c() {
    console.log(a + b);
  }

  // 暴露
  var e = 1;

  // 暴露多个属性：封装成对象就好了
  return {
    e_item: e,
    c_item: c
  };
})();
```

`2.js`

```js
(function () {
  console.log(abc.e_item); // 1
  abc.c_item(); // 3
})();
```

> **✨ 总结**
>
> **如果不想造成污染，就写成立即执行函数的形式（立即声明，立即调用）**
>
> **如果想在立即执行函数里暴露变量、函数，直接用 renturn 返回，外层用一个全局变量接收**
>
> **如果想暴露多个变量、函数，就把 return 封装成包含这些变量和函数的对象，外层用一个全局变量接收**

## 练习

> File
>
> |---- index.html
>
> |---- 1.js
>
> |---- 2.js

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- 引入 1.js 和 2.js -->
    <script src="./js/1.js"></script>
    <script src="./js/2.js"></script>
  </body>
</html>
```

`1.js`

```js
var a = 1; // 避免污染
var b = 2; // 避免污染
// 暴露为：sayHi
function hello() {
  console.log("hello world");
}
// 暴露为：count
var count = 1;
```

`2.js`

```js
var a = 3; // 避免污染
var b = 4; // 避免污染

// 使用 1.js 暴露的函数和变量
```

---

**解答**

`1.js`

```js
// 暴露为 abc 这个对象
var abc = (function () {
  var a = 1; // 避免污染
  var b = 2; // 避免污染
  // 暴露为：sayHi
  function hello() {
    console.log("hello world");
  }
  // 暴露为：count
  var count = 1;

  // 暴露对象
  return {
    sayHi: hello,
    count: count
  };
})();
```

`2.js`

```js
(function () {
  var a = 3; // 避免污染
  var b = 4; // 避免污染

  // 使用 1.js 暴露的函数和变量
  abc.sayHi(); // sayHi 是函数的声明，所以要用 () 调用函数
  console.log(abc.count); // count 是变量
})();
```

控制台输出结果

```bash
hello world
1
```
