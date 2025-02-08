# 文字滚动效果

**这里纠正一下做效果时的心态，不要急，慢慢来，一点点改动，没有任何效果是一下就可以做出来的，通过不断的测试，不断的更改代码，才能形成最终的效果。放平心态！加油！你可以哒！(ง •_•)ง💪**

## 页面结构分析

![html结构解析-1](https://img.hoocode.com/i/2024/11/19/10s68qk.webp)
![html结构解析-2](https://img.hoocode.com/i/2024/11/19/10s6kbz.webp)
![html结构解析-3](https://img.hoocode.com/i/2024/11/19/10s6q0x.webp)

完整代码：

HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>文字滚动效果</title>
    <link rel="stylesheet" href="./index.css">
    <script src="./index.js" defer></script>
</head>
<body>
    <div class="container">
        <h1 class="fl title">Latest News</h1>
        <ul class="fl list">
            <li>Lorem ipsum dolor sit.</li>
            <li>1. Perferendis possimus nobis tempora?</li>
            <li>2. Velit laudantium nostrum impedit?</li>
            <li>3. Pariatur, ea? Eveniet, cupiditate?</li>
        </ul>
    </div>
</body>
</html>
```

这里 JS 的引入用了另一种新的方法：`<script src="./index.js" defer></script>` 。这种与在 body 结束前加 JS 有什么区别呢？
> 延迟加载： defer属性使得脚本在整个文档解析完毕后、在DOMContentLoaded事件触发前执行。适合那些依赖于DOM的脚本，确保HTML完全加载后再执行。
> 另一种属性：async属性。

CSS

```css
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.fl {
    float: left;
}

.container {
    background: lightblue;
    margin: 15px;
    overflow: hidden; /* 注1：给父容器添加上 bfc，这么做与单独设置 clearfix 的效果是一样的 */
    padding: 25px 10px;
}

.title {
    font-size: 16px;
    font-weight: normal;
    height: 23px;
    line-height: 23px;
    padding-right: 10px;
    border-right: 2px solid #bbb;
}

.list li {
    list-style-type: none;
    padding: 0;
    height: 23px;
    line-height: 23px;
}

.list {
    height: 23px;
    margin-left: 40px;
    overflow: hidden;
}
```

## 实现思路分析

> **思考**
> 1. 初始化 - 一开始做什么
> 2. 跟用户有什么交互，交互的内容是什么

在这个效果中，没有用户的交互，故不需要考虑第二点。

### 初始化，一开始做什么呢？
答：为了不全局污染，一开始就应该把要写的代码放到一个立即执行函数里。为了方便测试，可以等到最后再统一放进去。

**为了实现“无缝滚动”的效果，我们应该把列表中第一项克隆到最后一项，这样给用户的观感就是“无缝切换”，顺~**

#### 1. 将列表中的第一个元素，克隆到列表的最后一个

```js
(function(){
    // 初始化：一开始做什么
    var list = document.querySelector('.list');
    // 1. 将列表中的第一个元素，克隆到列表的最后一个
    var firstNode = list.children[0];
    var cloneNode = firstNode.cloneNode(true); // [注 1]
    console.log(cloneNode); // [注 2]
    list.appendChild(cloneNode); // 添加到列表的最后
})();
```

注 1：[dom.cloneNode()](https://developer.mozilla.org/en-US/docs/Web/API/Node/cloneNode) 这是一个克隆 dom 节点的函数。如果不传入参数，则返回的是浅度克隆；如果传入参数 true，则返回的是深度克隆。
注 2：如果是 firstNode.cloneNode()，则这里打印的就是 **\<li>\</li>**，如果是 firstNode.cloneNode(true)，则这里打印的就是 **\<li>Lorem ipsum dolor sit.\</li>**。前者是浅度克隆传递过来的元素，后者是深度克隆传递过来的元素。

#### 2. 滚动：每隔一段时间，将列表滚动到下一个位置

从逻辑上可知，我们需要一个计时器

尽量避免硬编码

**硬编码（hard code）：把字面量直接写死到某一个位置。这会造成维护的困难。解决方法就是使用变量来保存字面量。**

```js
// 2. 滚动：每隔一段时间，将列表滚动到下一个位置
var duration = 2000; // 滚动的时间间隔
setInterval(moveNext, duration);

// 将列表滚动到下一个位置
function moveNext() {
    // *本质上就是调整列表元素的 scrollTop
};
```

![list-scrollTop-1](https://img.hoocode.com/i/2024/11/19/k4rten.gif)

规律：
第 0 项 --> 滚动到 0；第 1 项 --> 滚动到 1 * ( li 的高度 )；第 2 项 --> 滚动到 2 *  ( li 的高度 )；第 3 项 --> 滚动到 3 * ( li 的高度 ) ...

所以这里我们需要知道当前滚动到第几项，用一个 `curIndex` 变量来记录当前滚动到第几项。

```js
// 2. 滚动：每隔一段时间，将列表滚动到下一个位置
var duration = 2000; // 滚动的时间间隔
setInterval(moveNext, duration);

var curIndex = 0; // 目前展示的是第几项

var itemHeight = 23; // 每一项的高度

// 将列表滚动到下一个位置
function moveNext() {
    // *本质上就是调整列表元素的 scrollTop
    // 利用 curIndex 来计算出 scrollTop
    curIndex ++ ; // 滚动到下一项
    var to = curIndex * itemHeight; // 下一项的滚动高度
    list.scrollTop = to;
    console.log("当前滚动到：" + list.scrollTop); 
};
```

让它自己自动跑起来

![list-scrollTop-2](https://img.hoocode.com/i/2024/11/19/o0sptv.gif)

#### 3. 考虑动画：现在想让每一项在切换的时候，是慢慢的过去，而不是突然的就到达下一项。

分析：什么是慢慢过去？
答：就是位置**一点点改变**，最终达到目标位置。

所以，我们需要知道的有 **最开始的值** 和 **最终位置的值**。

##### Ⅰ. 先拿到 **最开始的值** 和 **最终位置的值**

- 最开始的值：当还没滚动到下一项的位置。所以要在 curIndex 还没加的时候拿到这个值。
- 最终位置的值：前面已经拿到了，就是 to 里的值

```js
// 2. 滚动：每隔一段时间，将列表滚动到下一个位置
var duration = 2000; // 滚动的时间间隔
setInterval(moveNext, duration);

var curIndex = 0; // 目前展示的是第几项

var itemHeight = 23; // 每一项的高度

// 将列表滚动到下一个位置
function moveNext() {
    // *本质上就是调整列表元素的 scrollTop
    var from = curIndex * itemHeight;
    // 利用 curIndex 来计算出 scrollTop
    curIndex ++ ; // 滚动到下一项
    var to = curIndex * itemHeight; // 下一项的滚动高度
    list.scrollTop = to;
    console.log("从" + from + "到" + to); 
};
```

控制台：
```bash
从0到23     index.js:29
从23到46    index.js:29 
从46到69    index.js:29 
从69到92    index.js:29 
```

##### Ⅱ. 慢慢变过去:  **在一段时间内，每隔一小段间隔，变化一点** ，本质上就是 **计时器** 。

比如说：从 0 一点点地变到 23。
要求：变化的总时间是：500ms；每间隔 10ms 变一点；总共需要变 500 / 10 次；每次变化的像素量：(23 - 0) / 50；

故需要准备的变量有：变化的总时间、变化的间隔时间、变化的次数、每次变化的量

```js
// 2. 滚动：每隔一段时间，将列表滚动到下一个位置
var duration = 2000; // 滚动的时间间隔
setInterval(moveNext, duration);

var curIndex = 0; // 目前展示的是第几项

var itemHeight = 23; // 每一项的高度

// 将列表滚动到下一个位置
function moveNext() {
    // *本质上就是调整列表元素的 scrollTop
    var from = curIndex * itemHeight; // 起始的滚动高度
    // 利用 curIndex 来计算出 scrollTop
    curIndex ++ ; // 滚动到下一项
    var to = curIndex * itemHeight; // 下一项的滚动高度

    // 让 scrollTop，慢慢地从 from 变到 to
    // 慢慢地：在一段时间内，每隔一小段间隔，变化一点
    var totalDuration = 500; // 变化的总时间
    var duration = 10; // 变化的间隔时间
    var times = totalDuration / duration; // 变化的次数
    var dis = (to - from) / times; // 每次变化的量
};
```

开始设计计时器

```js
// 将列表滚动到下一个位置
function moveNext() {
    // *本质上就是调整列表元素的 scrollTop
    var from = curIndex * itemHeight; // 起始的滚动高度
    // 利用 curIndex 来计算出 scrollTop
    curIndex ++ ; // 滚动到下一项
    var to = curIndex * itemHeight; // 下一项的滚动高度（目标的滚动高度）

    // 让 scrollTop，慢慢地从 from 变到 to
    // 慢慢地：在一段时间内，每隔一小段间隔，变化一点
    var totalDuration = 500; // 变化的总时间
    var duration = 10; // 变化的间隔时间
    var times = totalDuration / duration; // 变化的次数
    var dis = (to - from) / times; // 每次变化的量

    // 设置计时器
    var timerId = setInterval(function(){
        from += dis; // 每隔 duration 加一点像素
        if (from >= to) { // 当加到 超过 最终位置的像素值时
            clearInterval(timerId); // 停止计时
        }
        console.log("滚滚滚..."+from);
    }, duration);
};
```

控制台：
```bash
index.js:42 滚滚滚...0.46
index.js:42 滚滚滚...0.92
index.js:42 滚滚滚...1.3800000000000001
index.js:42 滚滚滚...1.84
index.js:42 滚滚滚...2.3000000000000003
index.js:42 滚滚滚...2.7600000000000002
index.js:42 滚滚滚...3.22
index.js:42 滚滚滚...3.68
index.js:42 滚滚滚...4.140000000000001
index.js:42 滚滚滚...4.6000000000000005
index.js:42 滚滚滚...5.0600000000000005
index.js:42 滚滚滚...5.5200000000000005
index.js:42 滚滚滚...5.98
index.js:42 滚滚滚...6.44
index.js:42 滚滚滚...6.9
index.js:42 滚滚滚...7.36
index.js:42 滚滚滚...7.82
index.js:42 滚滚滚...8.280000000000001
index.js:42 滚滚滚...8.740000000000002
index.js:42 滚滚滚...9.200000000000003
index.js:42 滚滚滚...9.660000000000004
index.js:42 滚滚滚...10.120000000000005
index.js:42 滚滚滚...10.580000000000005
index.js:42 滚滚滚...11.040000000000006
index.js:42 滚滚滚...11.500000000000007
index.js:42 滚滚滚...11.960000000000008
index.js:42 滚滚滚...12.420000000000009
index.js:42 滚滚滚...12.88000000000001
index.js:42 滚滚滚...13.34000000000001
index.js:42 滚滚滚...13.800000000000011
index.js:42 滚滚滚...14.260000000000012
index.js:42 滚滚滚...14.720000000000013
index.js:42 滚滚滚...15.180000000000014
index.js:42 滚滚滚...15.640000000000015
index.js:42 滚滚滚...16.100000000000016
index.js:42 滚滚滚...16.560000000000016
index.js:42 滚滚滚...17.020000000000017
index.js:42 滚滚滚...17.480000000000018
index.js:42 滚滚滚...17.94000000000002
index.js:42 滚滚滚...18.40000000000002
index.js:42 滚滚滚...18.86000000000002
index.js:42 滚滚滚...19.32000000000002
index.js:42 滚滚滚...19.780000000000022
index.js:42 滚滚滚...20.240000000000023
index.js:42 滚滚滚...20.700000000000024
index.js:42 滚滚滚...21.160000000000025
index.js:42 滚滚滚...21.620000000000026
index.js:42 滚滚滚...22.080000000000027
index.js:42 滚滚滚...22.540000000000028
index.js:42 滚滚滚...23.00000000000003
```

计时器设计正确后可更改 list 的滚动高度

```js
// 设置计时器
var timerId = setInterval(function(){
    from += dis; // 每隔 duration 加一点像素
    if (from >= to) { // 当加到 超过 最终位置的像素值时
        clearInterval(timerId); // 停止计时
    }
    console.log("滚滚滚..."+from);
    list.scrollTop = from; // 更改 list 的滚动高度
}, duration);
```

![list-scrollTop-3](https://img.hoocode.com/i/2024/11/19/pdk3ju.gif)

#### 4. 当滚动到最后一项时，要做处理

当滚动到最后一项时，需要重新回到第一项

怎么回到第一项呢？
答：scrollTop 要回到 0，并且 curIndex 也要回到 0。

```js
// 设置计时器
var timerId = setInterval(function(){
    from += dis; // 每隔 duration 加一点像素
    if (from >= to) { // 当加到 超过 最终位置的像素值时
        clearInterval(timerId); // 停止计时
        // 滚动完成后，如果是最后一项
        if (curIndex === list.children.length - 1) {
            // list.scrollTop = 0; // [注]
            from = 0; // 回到初始位置
            curIndex = 0; // 回到第一项
        }
    }
    list.scrollTop = from;
}, duration);
```

注：这里为什么不能直接用 list.scrollTop = 0 呢？
答：因为判断外的 list.scrollTop = from 会将这条语句覆盖掉，导致设跟没设一样。所以此时可以直接给 from 设置成 0。

那么，(☞ﾟヮﾟ)☞到这里这个效果就做完啦！☜(ﾟヮﾟ☜)

完整代码：

JavaScript

```js
(function(){
    // 初始化：一开始做什么
    var list = document.querySelector('.list');
    // 1. 将列表中的第一个元素，克隆到列表的最后一个
    function cloneFirstItem() {
        var firstNode = list.children[0];
        var cloneNode = firstNode.cloneNode(true); // [注 1]
        // console.log(cloneNode); // [注 2]
        list.appendChild(cloneNode); // 添加到列表的最后
    }
    cloneFirstItem();

    // 2. 滚动：每隔一段时间，将列表滚动到下一个位置
    var duration = 2000; // 滚动的时间间隔
    setInterval(moveNext, duration);

    var curIndex = 0; // 目前展示的是第几项

    var itemHeight = 23; // 每一项的高度

    // 将列表滚动到下一个位置
    function moveNext() {
        // *本质上就是调整列表元素的 scrollTop
        var from = curIndex * itemHeight; // 起始的滚动高度
        // 利用 curIndex 来计算出 scrollTop
        curIndex ++ ; // 滚动到下一项
        var to = curIndex * itemHeight; // 下一项的滚动高度（目标的滚动高度）

        // 让 scrollTop，慢慢地从 from 变到 to
        // 慢慢地：在一段时间内，每隔一小段间隔，变化一点
        var totalDuration = 500; // 变化的总时间
        var duration = 10; // 变化的间隔时间
        var times = totalDuration / duration; // 变化的次数
        var dis = (to - from) / times; // 每次变化的量

        // 设置计时器
        var timerId = setInterval(function(){
            from += dis; // 每隔 duration 加一点像素
            if (from >= to) { // 当加到 超过 最终位置的像素值时
                clearInterval(timerId); // 停止计时
                // 滚动完成后，如果是最后一项
                if (curIndex === list.children.length - 1) {
                    // list.scrollTop = 0; // [注]
                    from = 0; // 回到初始位置
                    curIndex = 0; // 回到第一项
                }
            }
            list.scrollTop = from;
        }, duration);
    };
})();
```

完整效果：

![show](https://img.hoocode.com/i/2024/11/19/ps9g8l.gif)
