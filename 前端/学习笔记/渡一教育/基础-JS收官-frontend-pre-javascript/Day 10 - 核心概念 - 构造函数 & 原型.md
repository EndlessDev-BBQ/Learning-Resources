# Day 10 - 核心概念 - 构造函数

## 引言 1

```js
var person = {
  firstName: "邓",
  lastName: "旭明",
  fullName: person.firstName + person.lastName
};

console.log(person); // TypeError: Cannot read properties of undefined (reading 'firstName')
```

这里是 fullName 报错，因为 fullName 下的 person.firstName 和 person.lastName 是无效的值，即 undefined。

为什么呢？

答：因为根据 函数的存储和传递 知识，fullName 在调用 person.firstName 时，这个 person 对象还没被创建出来。

那 person 什么时候被创建出来呢？

答：在对象表达式（即{...}）创建完成后，给 person 变量赋值的时候才创建出来的。

如何书写呢？

第一种方式

```js
var person = {
  firstName: "邓",
  lastName: "旭明",
  fullName: "邓旭明"
};

console.log(person);

/**
 * 打印结果
 * { firstName: '邓', lastName: '旭明', fullName: '邓旭明' }
 */
```

第二种方式

```js
var person = {
  firstName: "邓",
  lastName: "旭明"
};

person.fullName = person.firstName + person.lastName;

console.log(person);

/**
 * 打印结果
 * { firstName: '邓', lastName: '旭明', fullName: '邓旭明' }
 */
```

## 引言 2

为什么这里的打印结果不会报错？

```js
var person = {
  firstName: "邓",
  lastName: "旭明",
  // 对象方法，简称方法。其特点是属性的值是一个函数
  sayHi: function () {
    console.log("我的名字叫做：" + person.fullName);
  }
};

person.fullName = person.firstName + person.lastName;

console.log(person);

/**
 * 打印结果
 * {
    firstName: '邓',
    lastName: '旭明',
    sayHi: [Function: sayHi],
    fullName: '邓旭明'
    }
 */
```

答：因为函数被创建出来后，要被调用才会执行里面的语句。而外围 person 对象在调用 sayHi 方法时，person 对象已经创建出来了，所以 person.fullName 也创建出来了。

如何执行？

```js
var person = {
  firstName: "邓",
  lastName: "旭明",
  // 对象方法，简称方法。其特点是属性的值是一个函数
  sayHi: function () {
    console.log("我的名字叫做：" + person.fullName);
  }
};

person.fullName = person.firstName + person.lastName;

person.sayHi(); // 我的名字叫做：邓旭明

/**
 * person.sayHi 是方法
 * person.sayHi() 是方法的调用
 */
```

## 问题

每次创建一个 person ，都需要重复的写这些代码。导致代码重复性高。此时，想到函数存在的意义，那就是将重复的代码提取到一个函数里面，要用的时候，直接调用这个函数即可。这样就简化了每次创建 person 的代码了。

```js
// 创建一个名为 createPerson 的函数
function createPerson(firstName, lastName) {
  var obj = {
    firstName: firstName,
    lastName: lastName,
    fullName: firstName + lastName, // 这里用到字面量是传递过来的形参
    sayHi: function () {
      console.log("我的名字叫做：" + obj.fullName);
    }
  };

  return obj; // 这里把这个对象的地址返回
}

// 创建一个对象
var person = createPerson("章", "小小"); // person 是一个指向函数内部 obj 地址的对象

console.log(person);

/**
 * 打印结果
 * {
    firstName: '章',
    lastName: '小小',
    fullName: '章小小',
    sayHi: [Function: sayHi]
    }
 */

person.sayHi(); // 我的名字叫做：章小小
```

==> 在此基础上，对代码进一步优化，这时候可以用**构造函数**来帮我们创建对象。

## 构造函数

**什么是构造函数？**

**构造函数的作用是来帮我们创建对象的。**

**用构造函数优化后的代码**

```js
// 函数名采用大驼峰命名
function Person(firstName, lastName) {
  // var this = {}; 不需要这行，构造函数会自己帮我们加

  // 把 obj 全改为 this 关键字
  this.firstName = firstName;
  this.lastName = lastName;
  this.fullName = firstName + lastName;
  this.sayHi = function () {
    console.log("我的名字叫做：" + this.fullName);
  };

  // return this; 不需要这行，构造函数会自己帮我们加
}
```

**如何使用构造函数？**

**在该构造函数的名字前加 new；不加 new 也可以，但是就只是把 Person 当成普通函数调用**

```js
var person = new Person("章", "小小");

person.sayHi(); // 我的名字叫做：章小小

console.log(person);

/**
 * 打印结果
 * Person {
    firstName: '章',
    lastName: '小小',
    fullName: '章小小',
    sayHi: [Function (anonymous)]
    }
 */
```

**了解了构造函数，那就要清楚一点了！！！**

> 🎉🎉🎉 **重磅新闻！！！**
>
> **在 JS 中，所有的对象（普通对象、数组、函数），都是通过构造函数产生的！**

**新闻冲击太大，一时缓不过来，什么意思？(ツ)**

1. 普通对象

```js
// 平常用的都是语法糖
var obj = {
  name: "xxx",
  age: 18
};

console.log(obj);

/**
 * 打印结果
 * { name: 'xxx', age: 18 }
 */
```

本质

```js
// 本质上：通过构造函数创建对象
var obj = new Object(); // 创建一个空对象
obj.name = "xxx";
obj.age = 18;

console.log(obj);

/**
 * 打印结果
 * { name: 'xxx', age: 18 }
 */
```

2. 数组

```js
var arr = [1, 2, 3];

console.log(arr); // [ 1, 2, 3 ]
```

本质

```js
var arr = new Array(1, 2, 3);

console.log(arr); // [ 1, 2, 3 ]
```

3. 函数

```js
function sum(a, b) {
  return a + b;
}

console.log(sum(1, 2)); // 3
```

本质

```js
var sum = new Function("a", "b", "return a + b");

console.log(sum(1, 2)); // 3
```

## 练习

**利用构造函数创建一幅扑克牌**

```js
/**
 * Deck: 一副扑克牌
 * Pocker: 一张扑克牌
 */

/**
 * 创建一张扑克牌
 * @param {number} number 1-1, ..., 11-J, 12-Q, 13-K, 14-小王, 15-大王
 * @param {number} color 1-黑桃, 2-红桃, 3-梅花, 4-方块
 */
function Pocker(number, color) {
  this.number = number;
  this.color = color;

  this.print = function () {
    if (this.number === 14) {
      console.log("jocker");
      return;
    }
    if (this.number === 15) {
      console.log("JOCKER");
      return;
    }
    // 其他情况
    // 得到花色
    var colors = ["♠", "♥", "♣", "♦"];
    var color = colors[this.color - 1]; // 定义一个函数作用域的变量，与 this.color 不冲突

    var numbers = ["A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"];
    var number = numbers[this.number - 1];

    console.log(color + number);
  };
}

// var p1 = new Pocker(1, 1);
// p1.print(); // 输出黑桃A

/**
 * 创建一叠扑克牌
 */
function Deck() {
  this.pockers = [];
  for (var i = 1; i <= 13; i++) {
    for (var j = 1; j <= 4; j++) {
      this.pockers.push(new Pocker(i, j));
    }
  }

  this.pockers.push(new Pocker(14, 0));
  this.pockers.push(new Pocker(15, 0));

  this.print = function () {
    for (var i = 0; i < this.pockers.length; i++) {
      this.pockers[i].print();
    }
  };
}

var deck = new Deck();
deck.print(); // 输出一副扑克牌
```

```bash
♠A
♥A
♣A
♦A
♠2
♥2
♣2
♦2
♠3
♥3
♣3
♦3
♠4
♥4
♣4
♦4
♠5
♥5
♣5
♦5
♠6
♥6
♣6
♦6
♠7
♥7
♣7
♦7
♠8
♥8
♣8
♦8
♠9
♥9
♣9
♦9
♠10
♥10
♣10
♦10
♠J
♥J
♣J
♦J
♠Q
♥Q
♣Q
♦Q
♠K
♥K
♣K
♦K
jocker
JOCKER
```

> 要知道：调用函数的次数，远远大于声明函数的次数。

# 核心概念 - 原型

## 原型要解决的问题

![原型要解决的问题](https://img.hoocode.com/i/2024/11/01/fqj4t.webp)

上图中，通过构造函数可以创建一个用户对象

这种做法有一个严重的缺陷，就是每个用户对象中都拥有一个`sayHi`方法，对于每个用户而言，`sayHi`方法是完全一样的，没必要为每个用户单独生成一个。

要解决这个问题，必须学习原型

## 原型是如何解决问题的

![原型是如何解决问题的](https://img.hoocode.com/i/2024/11/01/fqp29.webp)

1. **原型**

   每个函数都会自动附带一个属性`prototype`，这个属性的值是一个普通对象，称之为原型对象

```js
function m() {}

console.log(m.prototype); // {}
```

2. **实例**

   instance，通过`new`产生的对象称之为实例。

   > 由于 JS 中所有对象都是通过`new`产生的，因此，严格来说，JS 中所有对象都称之为实例

3. **隐式原型**

   每个实例都拥有一个特殊的属性`__proto__`，称之为隐式原型，它指向构造函数的原型

```js
function User() {}

User.prototype.a = 1; // User.prototype = { a: 1 }
User.prototype.b = 2; // User.prototype = { a: 1, b: 2 }

console.log(User.prototype); // { a: 1, b: 2 }

var u1 = new User();

console.log(u1.__proto__); // { a: 1, b: 2 }

console.log(u1.__proto__ === User.prototype); // true
```

这一切有何意义？

**当访问实例成员时，先找自身，如果不存在，会自动从隐式原型中寻找**

**这样一来，我们可以把那些公共成员，放到函数的原型中，即可被所有实例共享**

![这一切有何意义](https://img.hoocode.com/i/2024/11/01/fqsra.webp)

**例子**

```js
function User(name, age) {
  this.name = name;
  this.age = age;
}

User.prototype.sayHi = function () {
  console.log("你好，我是" + this.name + "，今年" + this.age + "岁了");
};

var u1 = new User("monica", 17);
var u2 = new User("邓哥", 77);
var u3 = new User("成哥", 18);

u1.sayHi(); // 你好，我是monica，今年17岁了
/**
 * u1 在自身的属性内没找到 sayHi 属性，就自动到隐式原型中寻找了
 */
```

## 练习

**使用原型重构之前的扑克牌程序**

```js
/**
 * Deck: 一副扑克牌
 * Pocker: 一张扑克牌
 */

/**
 * 创建一张扑克牌
 * @param {number} number 1-1, ..., 11-J, 12-Q, 13-K, 14-小王, 15-大王
 * @param {number} color 1-黑桃, 2-红桃, 3-梅花, 4-方块
 */
function Pocker(number, color) {
  this.number = number;
  this.color = color;
}

// 放入原型中
Pocker.prototype.print = function () {
  if (this.number === 14) {
    console.log("jocker");
    return;
  }
  if (this.number === 15) {
    console.log("JOCKER");
    return;
  }
  // 其他情况
  // 得到花色
  var colors = ["♠", "♥", "♣", "♦"];
  var color = colors[this.color - 1]; // 定义一个函数作用域的变量，与 this.color 不冲突

  var numbers = ["A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"];
  var number = numbers[this.number - 1];

  console.log(color + number);
};

// var p1 = new Pocker(1, 1);
// p1.print(); // 输出黑桃A

/**
 * 创建一叠扑克牌
 */
function Deck() {
  this.pockers = [];
  for (var i = 1; i <= 13; i++) {
    for (var j = 1; j <= 4; j++) {
      this.pockers.push(new Pocker(i, j));
    }
  }

  this.pockers.push(new Pocker(14, 0));
  this.pockers.push(new Pocker(15, 0));
}

// 放入原型中
Deck.prototype.print = function () {
  for (var i = 0; i < this.pockers.length; i++) {
    this.pockers[i].print();
  }
};

var deck = new Deck();
deck.print();
```

```bash
♠A
♥A
♣A
♦A
♠2
♥2
♣2
♦2
♠3
♥3
♣3
♦3
♠4
♥4
♣4
♦4
♠5
♥5
♣5
♦5
♠6
♥6
♣6
♦6
♠7
♥7
♣7
♦7
♠8
♥8
♣8
♦8
♠9
♥9
♣9
♦9
♠10
♥10
♣10
♦10
♠J
♥J
♣J
♦J
♠Q
♥Q
♣Q
♦Q
♠K
♥K
♣K
♦K
jocker
JOCKER
```

> **小贴士：**
>
> **在实际开发中，95%的情况下会将构造函数内的方法放到原型中**
