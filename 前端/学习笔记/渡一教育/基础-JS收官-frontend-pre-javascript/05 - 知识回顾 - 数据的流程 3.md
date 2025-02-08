# Day 5 - 知识回顾 - 数据的流程 3

## 练习 1：

**输出一个对象的所有键值对**

```js
var obj = {
  a: 1,
  b: 23,
  c: "kashgjd"
};
for (var key in obj) {
  // 这里的 key 就是键
  //   console.log(key); // a b c
  console.log(key + " = " + obj[key]); // 不能用 obj.key，因为 obj.key = obj['key']
}
```

**结果**

```bash
a = 1
b = 23
c = kashgjd
```

## 练习 2：

**计算对象中字符串属性的数量**

```js
var obj = {
  a: 1,
  b: "2",
  c: "abbs",
  d: "bcd"
};

var count = 0; // 计数器

for (var key in obj) {
  // 判断 obj[key] 是不是字符串：typeof
  if (typeof obj[key] === "string") {
    count++;
  }
}

console.log(count);
```

**结果**

```bash
3
```

## 练习 3：

**将一个对象所有的数字属性，转换为字符串，并在其前面加上￥**

```js
例如：
{
    name:"xxx",
    balance: 199.8, //余额
    taken: 3000 //消费
}
-->
{
    name:"xxx",
    balance: '￥199.8', //余额
    taken: '￥3000' //消费
}
*/
```

```js
// 法一：需改动原对象的值
var obj = {
  name: "xxx",
  balance: 199.8, //余额
  taken: 3000 //消费
};

for (var key in obj) {
  // 判断 obj[key] 是不是数字属性
  if (typeof obj[key] === "number") {
    obj[key] = "￥" + obj[key];
  }
}

console.log(obj);
```

```js
// 法二：不改动原对象的值 ---- IMMUTABLE
var obj = {
  name: "xxx",
  balance: 199.8, //余额
  taken: 3000 //消费
};

var newObj = {}; // 存储改动后的对象
for (var key in obj) {
  var value = obj[key];
  if (typeof value === "number") {
    newObj[key] = "￥" + obj[key];
  } else {
    newObj[key] = obj[key];
  }
}

console.log(newObj);
```

## 练习 4：

**按照下面的要求进行转换**

```js
[1, 2, 3]
-->
[
    {number:1, doubleNumber: 2},
    {number:2, doubleNumber: 4},
    {number:3, doubleNumber: 6},
]
```

```js
var arr = [1, 2, 3];
var newArr = [];

for (var i = 0; i < arr.length; i++) {
  /**
   * 1. 生成的新数组对象个数与原数组元素个数保持一致
   * 2. 每一项都是一个对象
   * 3. number: arr[i]
   * 4. doubleNumber: arr[i] * 2
   */
  newArr.push({ number: arr[i], doubleNumber: arr[i] * 2 });
}

console.log(newArr);
```

## 综合题：
**数据格式**
```js
var students = [
  {
    id: 988985,
    name: "梁平",
    sex: "女",
    age: 15,
    address: "安徽省 淮南市",
    tel: "12957961008"
  },
  ...
  ...
  ...
];  
```

### 练习 1：

**遍历输出学生的姓名**

```js
for (var i = 0; i < students.length; i++) {
  // students[i] 就是每个对象
  // students[i].name === students[i]['name'] 这里不能用 students[i][name]，因为 students[i][name] === students[i]['梁平']，但是没有这个属性
  console.log(students[i].name);
}
```

### 练习 2：

**得到所有女生（新数组）**

```js
var girlsArr = [];
for (var i = 0; i < students.length; i++) {
  // students[i] 就是每个对象
  // 判断 students[i].sex 是否是女生
  if (students[i].sex === "女") {
    // 把这个对象插入到新数组中
    var obj = students[i];
    girlsArr.push(obj);
  }
}
console.log(girlsArr);
```

### 练习 3：

**得到所有年龄在 25 岁以下的女生（新数组）**

```js
var girlsAgeUnder25 = [];
for (var i = 0; i < students.length; i++) {
  // students[i] 就是每个对象
  // 判断 students[i].sex 是否是女生 并且 students[i].age 是否小于25
  if (students[i].sex === "女" && students[i].age < 25) {
    var obj = students[i];
    girlsAgeUnder25.push(obj);
  }
}
console.log(girlsAgeUnder25);
```
