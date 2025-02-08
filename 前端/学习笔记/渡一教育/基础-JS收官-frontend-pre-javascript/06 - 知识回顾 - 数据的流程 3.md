# Day 6 - 知识回顾 - 数据的流程 3
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

**得到所有姓陈的学生（新数组）**

```js
var chenStu = [];
for (var i = 0; i < students.length; i++) {
  var obj = students[i];
  if (obj.name[0] === "陈") {
    // 字符串可以当数组用
    chenStu.push(obj);
  }
}
console.log(chenStu);
```

### 练习 2：

**得到所有电话号码以 1 结尾的学生（新数组）**

```js
var endWith1 = [];
for (var i = 0; i < students.length; i++) {
  var obj = students[i];
  if (obj.tel[obj.tel.length - 1] === "1") {
    endWith1.push(obj);
  }
}
console.log(endWith1);
```

### 练习 3：

**得到所有学生姓名组成的数组（新数组）**

```js
var allNameArr = [];
for (var i = 0; i < students.length; i++) {
  var name = students[i].name;
  allNameArr.push(name);
}
console.log(allNameArr);
```

### 练习 4：

**得到所有女生的姓名数组（新数组）**

```js
var allGirlsNameArr = [];
for (var i = 0; i < students.length; i++) {
  var obj = students[i];
  if (obj.sex === "女") {
    allGirlsNameArr.push(obj.name);
  }
}
console.log(allGirlsNameArr);
```

### 练习 5：

**得到所有女生的姓名和电话号码 [ {name:'monica', tel:'18122223333'} ]**

```js
var allGirlsNameAPhone = [];
for (var i = 0; i < students.length; i++) {
  var obj = students[i];
  if (obj.sex === "女") {
    allGirlsNameAPhone.push({ name: obj.name, tel: obj.tel });
  }
}

console.log(allGirlsNameAPhone);
```

### 练习 6：

**得到所有学生的年龄的总和**

```js
var sumStudentsAge = 0;
for (var i = 0; i < students.length; i++) {
  sumStudentsAge += students[i].age;
}
console.log(sumStudentsAge);
```

### 练习 7：

**得到所有学生的平均年龄**

```js
var stuSum = students.length; // 计算有多少个学生
var sumStudentsAge = 0;
for (var i = 0; i < students.length; i++) {
  sumStudentsAge += students[i].age;
}
var averageAge = sumStudentsAge / stuSum;
console.log(averageAge);
```

### 练习 8：

**得到一个对象： {name:['张三', '李四', ...], age: [17, 25, ...]}**

```js
var objStu = {
  name: [],
  age: []
};
for (var i = 0; i < students.length; i++) {
  var obj = students[i];
  var objName = obj.name;
  var objAge = obj.age;
  objStu["name"].push(objName);
  objStu["age"].push(objAge);
}
console.log(objStu);
```

### 练习 9：

**找到 id 为 796997 的学生对象**

```js
var isId = null;
for (var i = 0; i < students.length; i++) {
  var obj = students[i];
  if (obj.id === 796997) {
    isId = obj;
  }
}
console.log(isId);
```

### 练习 10：

**是否包含年龄大于 28 岁的男生**

```js
var hasBoyAgeOver28 = false;
for (var i = 0; i < students.length; i++) {
  var obj = students[i];
  if (obj.sex === "男" && obj.age > 28) {
    hasBoyAgeOver28 = true;
    break;
  }
}
console.log(hasBoyAgeOver28 ? "是" : "否");
```

### 练习 11：

**是否所有的女生年龄都在 28 岁以内**

```js
/**
 * 逻辑：只要满足 有一个女生的年龄超过28 就否
 */
var isGirlsAgeAllUnder28 = true;
for (var i = 0; i < students.length; i++) {
  var obj = students[i];
  if (obj.sex === "女" && obj.age > 28) {
    isGirlsAgeAllUnder28 = false;
  }
}
console.log(isGirlsAgeAllUnder28 ? "是" : "否");
```
