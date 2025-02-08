# Day 11 - 核心概念 - 这是啥？

不同的场景，**这** 指代的含义不同，JS 中的**this**关键字也是如此：

- 在全局代码中使用 this，指代全局对象 (在 node 环境下：<ref \*1> Object [global] {...}；在 windows 环境下： Window {...})。

  > 在真实的开发中，很少在全局代码使用 this

- **在函数中使用 this，它的指向完全取决于函数是 如何被调用 的**

  | 调用方式          | 示例                | 函数中的 this 指向 |
  | ----------------- | ------------------- | ------------------ |
  | **通过 new 调用** | `new method()`      | 新对象             |
  | **直接调用**      | `method()`          | 全局对象           |
  | **通过对象调用**  | `obj.method()`      | 前面的对象         |
  | **call**          | `method.call(ctx)`  | call 的第一个参数  |
  | **apply**         | `method.apply(ctx)` | apply 的第一个参数 |

## 情形一：直接调用 —— this 指代全局对象

```js
function User(name, age) {
  console.log(this);
}

User(); // 在 node 环境下：<ref *1> Object [global] {...}；在 windows 环境下： Window {...}
```

## 情形二：通过 new 调用 —— this 指代新对象

```js
function User(name, age) {
  console.log(this);
}

new User(); // User {}
```

## 情形三：通过 对象 调用 —— this 指代前面的对象

```js
var obj = {
  a: 1,
  b: 2,
  method: function () {
    console.log(this);
  },
  c: {
    m: function () {
      console.log(this);
    }
  }
};

obj.method(); // this 指代的就是 obj 这个对象

obj.c.m(); // this 指代的就是 obj.c 这个对象
```

骚操作

```js
var obj = {
  a: 1,
  b: 2,
  method: function () {
    console.log(this);
  },
  c: {
    m: function () {
      console.log(this);
    }
  }
};

var x = obj.c.m;
x(); // window
```

**创建函数过后，还可以往函数里加东西**

```js
function m(a, b) {
  console.log(this, a, b);
}

m.abc = function () {
  console.log(this);
};

m.abc(); // this 指向 m 对象
```

## 情形四：通过 call 或 apply 调用 —— this 指代 call 或 apply 的第一个参数

```js
function m(a, b) {
  console.log(a, this, b);
}

m.call(); // 就是调用函数的意思；本质上就是 m(); 所以里面的 this 就是打印的 window
// 在函数调用的时候可以更改 this 的指向

var arr = [1, 2, 3];

m.call(arr, 1, 2); // 调用 m 函数，让它里面的 this 指向 arr
/**
 * 打印结果：
 * 1 [ 1, 2, 3 ] 2
 */

m.apply(arr, [1, 2]); // 只是传递方式不一样，但是效果是完全一样的

/**
 * 打印结果：
 * 1 [ 1, 2, 3 ] 2
 */
```

## 练习 1

```js
var person1 = {
  name: "monica",
  age: 17,
  sayHi: function () {
    // 完成该方法，打印姓名和年龄
    console.log(this.name + " " + this.age);
    /**
     * 用 this 的好处：
     * 哪个对象调用 sayHi 这函数，就打印哪个对象的。避免了定义对象所对应的变量名字变动所带来的麻烦。
     *
     * 开发小贴士：
     * 以后在对象里面写方法的时候，要调用到这个对象里的其他属性时就需要使用 this
     */
  }
};

person1.sayHi(); // monica 17
```

## 练习 2

```js
// 为所有对象添加方法print，打印对象的键值对

/**
 * 根据《构造函数和原型》核心概念，我们可以借助 “在 JS 中，所有的对象（普通对象、数组、函数），都是通过构造函数产生的！”解决这个问题
 */

/**
 * 本质上
 *  var obj1 = new Object();
    var obj2 = new Object();
*/

Object.prototype.print = function () {
  for (var key in this) {
    // 打印对象的键值对
    console.log(key + ":" + this[key]);
  }
};

var obj1 = {
  a: 1,
  b: 2
};

var obj2 = {
  a: "a",
  b: "b",
  c: "c"
};

obj1.print();
obj2.print();
```

打印结果

```bash
a:1
b:2
print:function () {
  for (var key in this) {
    // 打印对象的键值对
    console.log(key + ":" + this[key]);
  }
}
a:a
b:b
c:c
print:function () {
  for (var key in this) {
    // 打印对象的键值对
    console.log(key + ":" + this[key]);
  }
}
```

**这里有一点，就是会打印出原型里的 print 函数，但是我们只想打印在对象里的键值对，应该如何做呢？**

`obj.hasOwnProperty(key)`这个方法能很好的帮助我们解决这个问题

举个例子

```js
Object.prototype.abc = 1;

var obj = {
  a: 1,
  b: 2
};

// hasOwnProperty 这个函数会判断属性是否属于对象本身
console.log(obj.hasOwnProperty("a")); // true
console.log(obj.hasOwnProperty("b")); // true
console.log(obj.hasOwnProperty("abc")); // false

for (var key in obj) {
  // 判断这个属性是不是属于对象本身，而不是在隐式原型上
  if (obj.hasOwnProperty(key)) {
    console.log(key); // a b
  }
}
```

小知识：另一个判断属性是否在对象及其隐式原型上的方法

```js
Object.prototype.abc = 1;

var obj = {
  a: 1,
  b: 2
};

// 属性名 in 对象 ---> 判断 属性名 是否在对象自身及其隐式原型上
console.log("a" in obj); // true;
console.log("b" in obj); // true;
console.log("abc" in obj); // true;
```

本题优化

```js
// 为所有对象添加方法print，打印对象的键值对

/**
 * 根据《构造函数和原型》核心概念，我们可以借助 “在 JS 中，所有的对象（普通对象、数组、函数），都是通过构造函数产生的！”解决这个问题
 */

/**
 * 本质上
 *  var obj1 = new Object();
    var obj2 = new Object();
*/

Object.prototype.print = function () {
  for (var key in this) {
    // 判断 key 有没有属于对象本身
    if (this.hasOwnProperty(key)) {
      // 打印对象的键值对
      console.log(key + ":" + this[key]);
    }
  }
};

var obj1 = {
  a: 1,
  b: 2
};

var obj2 = {
  a: "a",
  b: "b",
  c: "c"
};

obj1.print();
obj2.print();
```

打印结果

```bash
a:1
b:2
a:a
b:b
c:c
```

## 练习 3

```js
function User(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.fullName = firstName + lastName;
}

// 能否不使用new，通过User函数创建对象（不能更改User函数）

var obj = {};

User.call(obj, "zhang", "san");

console.log(obj.fullName); // zhangsan
```

> 小贴士：
>
> 这种做法与 new 是有差异的，具体差异在哪，敬请期待，下一节课将揭晓
