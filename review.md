## js 基础

1. 原型 & 原型链 (Object.prototype.__proto__ === null)
2. 继承
> 原型链式继承
```js

function Person () {
  this.name = ['a', 'b', 'c'];
}

function Child () {

}

Child.prototype = new Person();

const c1 = new Child();

c1.name // ['a', 'b', 'c']
```

问题： 1， 引用类型的属性被所有实例共享，其中一个修改，导致全部被修改； 2， 实例化 子函数的 时候无法向父函数传参

> 构造函数式继承
```js

function Person (name) {
  this.name = name;
}

function Child (name) {
  Person.call(this, name);
}

const c1 = new Child('a');
const c2 = new Child('b');
```
解决了 原型链继承的问题； 缺点： 方法都在构造函数中定义，每次创建实例都会创建一遍方法。

> 组合继承

原型继承 + 构造函数继承 （js 最常用）;

> 原型继承

> ES6 Object.create() 模拟实现

```js

function CreateObj (o) {
  function F () {};
  F.prototype = o;

  return new F();
}

// 将传入的对象作为 创建对象的 原型
```
> 寄生式继承

创建一个仅用于封装继承过程的函数，该函数在内部以某种形式来做增强对象，最后返回对象。

```js

function createObj (o) {
    var clone = Object.create(o);
    clone.sayName = function () {
        console.log('hi');
    }
    return clone; 
}
```
缺点：跟借用构造函数模式一样，每次创建对象都会创建一遍方法。

> 寄生组合式继承

解决原始 组合继承 两次调用 父级构造函数的缺点，以及在子级实例原型上面创建多余属性问题

3. 作用域
由多个执行上下文 对象构成的链表 就叫做作用域链
4. 闭包
嵌套的两个函数，内部函数访问外部函数变量； 外部函数调用 返回内部函数；
5. 变量提升
6. this 问题
7. 立即执行函数（IIFE）
8. typeof ,instanceof 原理
> 判断数据类型 Object.prototype.toString.call(xx);
> instanceof 判断原理 leftVal.__proto === rightVal.prototype
9. bind 实现

```js

Function.prototype.bind2 = function (context) {

  const self = this;
  const arg1 = Array.prototype.slice.call(arguments, 1);
  function noop = () {};
  noop.prototype = this.prototype;
  function fun () {
    const arg2 = Array.prototype.slice.call(arguments);

    return self.call(this instanceof noop ? this : context, ...arg1.concat(arg2));
  }

  fun.prototype = new noop();

  return fun;
}
```

10. call and apply

```js

Function.prototype.call1 = function (context) {
  var context = context || window;
  context.fn = this;
  const arg = Array.prototype.slice(arguments, 1);
  const res = context.fn(...arg);
  delete context.fn;

  return res;
}

Function.prototype.apply1 = function (context, arr = []) {
  var context = context || window;
  context.fn = this;

  const res = context.fn(...arr);

  delete context.fn;

  return res;
}
```
11. 函数柯里化
> 将一个使用多个参数的 函数转换成一系列 使用单个参数的函数的方法

```js

function curry1 (fn) {
  const l = fn.length;
  const arg1 = Array.prototype.slice.call(arguments, 1);
  return function () {
    const arg2 = Array.prototype.slice.call(arguments);
    return [...arg1, ...arg2].length < l ? curry1.call(this, fn, ...[...arg1, ...arg2]) : fn.call(this, ...[...arg1, ...arg2]);
  }
}
```

12. V8 垃圾回收机制
13. 浮点数精度问题
> 计算机中数字的存储和计算都是二进制表示，64 位字节 存储 一个浮点数，存在有些数无法有限表示，此时只能模仿十进制进行四舍五入了，但是二进制只有 0 和 1 两个，于是变为 0 舍 1 入。出现精度丢失；
64 位 字节存储一个浮点数， 表示的 最大 数 Math.pow(2, 53) // 2 ** 53

14. new 操作符
```js

function objectFactory () {
  const obj = new Object();
  var Constructor = [].shift.call(arguments);
  obj.__proto__ = Constructor.prototype;
  const res = Constructor.call(obj, ...arguments);
  return typeof res === 'object' ? res : obj;

}
```
15. 事件循环机制
16. Promise
17. generators (生成器)
```js

function* test () {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
}

const test1 = test() // test1 -- 生成器对象

test1.next() // {value: 1, done: false}
test1.next() // {value: 2, done: false}
test1.next() // {value: 3, done: false}
test1.next() // {value: 4, done: false}
test1.next() // {value: undefined, done: true}
```
18. async / await

## CSS 基础

1. css 盒模型
2. css 选择器
> 元素选择器，id，类，组合选择器，属性选择器，伪类选择器，伪元素选择器
3. BFC
4. position
5. flex 布局
6. 选择器优先级
> !important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性。
权重值越大，优先级越高.
权重值一样，就近原则. (浏览器从上往下执行css代码，后面的会覆盖前面的样式)
7. 双飞翼布局/圣杯布局
> 左右两列固定宽度，中间列自适应布局的三列布局

* 圣杯布局 （使用float布局框架 ， 用margin为负值 ， position: relative定位）
* 双飞翼布局： float定位，中间列嵌套一个div 使用inner Div 的margin left，right 来布局，么有使用 position 相对定位
8. css3 新特性（animation, transition, transform, background-clip: border-box(默认), padding-box, content-box; ...）
> css3 提供多行超出省略号：(遗憾就是这个暂时只支持webkit浏览器)
```css
  /* ... */
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow : hidden;
  text-overflow: ellipsis;
  /* ... */
```
9. css 模块化
10. css 性能优化
```
1. 合并css文件，如果页面加载10个css文件,每个文件1k，那么也要比只加载一个100k的css文件慢。
2. 减少css嵌套，最好不要嵌套三层以上。
3. 不要在ID选择器前面进行嵌套，ID本来就是唯一的而且权限值大，嵌套完全是浪费性能。
4. 建立公共样式类，把相同样式提取出来作为公共类使用。
5. 减少通配符*或者类似[hidden="true"]这类选择器的使用，挨个查找所有...这性能能好吗？
6. 巧妙运用css的继承机制，如果父节点定义了，子节点就无需定义。
7. 拆分出公共css文件，对于比较大的项目可以将大部分页面的公共结构样式提取出来放到单独css文件里，这样一次下载 后就放到缓存里，当然这种做法会增加请求，具体做法应以实际情况而定。
8. 不用css表达式，表达式只是让你的代码显得更加酷炫，但是对性能的浪费可能是超乎你想象的。
9. 少用css rest，可能会觉得重置样式是规范，但是其实其中有很多操作是不必要不友好的，有需求有兴趣，可以选择normolize.css。
10. cssSprite，合成所有icon图片，用宽高加上background-position的背景图方式显现icon图，这样很实用，减少了http请求。
11. 善后工作，css压缩(在线压缩工具 YUI Compressor)
12. GZIP压缩，是一种流行的文件压缩算法。
```
11. 层叠上下文 （z-index）
12. div 居中
13. float 属性
14. 页面间通信
15. history & hash 路由两种模式
16. Event 事件模型
17. 浏览器缓存
> 强制缓存 & 协商缓存
强缓存：expires 服务器返回 强缓存的到期时间。缺点：客户端与服务器时间不一致，时间不准确问题
http1.1 =》 cache-control max-age=600 (使用相对时间来做)，600s 再次请求命中强缓存
> 协商缓存
协商缓存生效，状态码 304
Last-Modified / If-Modified-Since & Etag / If-None-Match
18. 浏览器架构
19. 内存泄漏
> 意外的全局变量； 遗忘的定时器，dom 引用未清除，Set， Map 引用未消除，闭包。
20. 浏览器性能优化
21. 重绘&重排
22. 白屏优化
> DNS解析优化：DNS缓存优化，DNS预加载策略
> 服务端优化：redis缓存，gzip压缩等，数据存储优化
> 浏览器下载，解析，渲染页面优化：html文件结构简洁，css文件结构优化，js 代码位置等
23. 大量图片加载优化，(延迟加载 & 减少图片文件大小)
24. 输入 url 到页面展现的过程
> DNS 解析 > 建立tcp连接 > http请求&服务器响应数据 > 浏览器下载，解析，渲染页面。
25. 浏览器动画性能优化
> 精简DOM结构， 合理布局； 使用transform 替代left，right，减少重排的次数；使用更友好的 requestAnimationFrame 语法。
26. 模块化机制
> commonJS, AMD, CMD, ES6 module
27. tree-shaking
28. uglifyJS 原理
> code => AST; AST -> 更加精简的AST； AST -》 code
29. babel 原理, babel plugin 
30. webpack 
31. webpack plugin, loader 机制
32. React 合成事件
33. virtual DOM
34. setState 过程
> setState 是异步还是同步？
> 在 React 合成事件中/ 生命周期钩子中 是异步的， 在 setTimeout/ 原生事件中 是同步的。

> 直接传递对象的setstate会被合并成一次
> 使用函数传递state不会被合并
35. React fiber
> 1. React的渲染分为协调和提交两个阶段，协调就是遍历dom树，进行domdiff，收集差异的阶段。提交就是修改真实dom，进行页面绘制的阶段。
> 2. React 会递归遍历节点，比对VirtualDOM树，找出需要变动的节点，然后同步更新它们。这个过程 React 称为Reconcilation(协调)
在Reconcilation期间，React 会一直占用着浏览器资源，一则会导致用户触发的事件得不到响应, 二则会导致掉帧，用户可能会感觉到卡顿
> 3. fiber使用链表数据结构，通过浏览器requestIdleCallbackapi，让协调过程实现了可中断执行，分片完成协调任务
在浏览器空闲时去执行协调过程，遍历节点，收集变动的节点，避免了界面卡顿

36. HOC (高阶组件)
37. React 错误处理 (Error boundaries, componentDidCatch)
38. React 性能优化
* PureComponent/ShouldComponentUpdate
* React.memo (函数组件)
* 善用 React.useMemo
* 合理使用 React.useCallback
* 谨慎使用 Context，Context 完全没有任何方案可以避免无用的渲染，我们无法阻止 Context 触发的 render。
* 小心使用 Redux

39. Redux
> 动作 和状态 统一管理（有什么动作给你什么状态）
> 每个state 变化可以监测
> ui 只负责渲染页面，数据管理交给 redux

40. 编程题目
```js

String.prototype.trim1 = function () {
  const str =  this.replace(/^\s*/, '').replace(/\s*$/, '');
  return str;
}

// deepClone
// 乞丐版本
JSON.stringify(JSON.parse(target));

// 基础版本
function clone (target) {
  let targetObj = {};
  for (const key in target) {
    targetObj[key] = target[key];
  }
  return targetObj;

}

// 递归
function clone (target) {
  if (typeof target === 'obejct') {
    let targetObj = {};
    for (const key in target) {
      targetObj[key] = clone(target[key]);
    }
    return targetObj;
  } else {
    return target;
  }
}

// 考虑数组

function clone (target) {
  if (typeof target === 'obejct') {
    // 加了种判断数组的类型
    let targetObj = Array.isArray(target) ? [] : {};
    for (const key in target) {
      targetObj[key] = clone(target[key]);
    }
    return targetObj;
  } else {
    return target;
  }
}


// curry (函数柯里化)

function curry (fn, ...args) {
  return args.length >= fn.length ? fn(...args) : (...args1) => curry(fn, ...args, ...args1);
}

// 大数相加 (转化成字符串的形式)

function add (a, b) {
  // 将a，b 转换为字符串
  a = a.toString();
  b = b.toString();
  // 补齐长度
  let maxLength = Math.max(a.length, b.length);
  a = a.padStart(maxLength, 0);
  b = b.padStart(maxLength, 0);
  // 开始相加
  let t = 0, sum = '', f = 0;
  for (let i = maxLength - 1; i >= 0; i--) {
    t = parseInt(a[i]) + parseInt(b[i]) + f;
    f = Math.floor(t / 10); // 进位
    sum += t % 10; 
  }
  if (f === 1) {
    sum += '1' + sum;
  }
  return Number(sum);
}

// 小数相加减操作，基本思想：
// 小数 -》 转化为 整数 然后操作，再转换回来 （a * m + b * m）/ m

// 数组拍平 (实现flat())

Array.prototype.myFlat = function (count = 1) {
  if (Number(count) <= 0) {
    return this;
  }
    let arr = [];
    // 利用 forEach 方法会跳过空位
  this.forEach(value => {
    if (Array.isArray(value)) {
      arr = arr.concat(value.myFlat(count - 1));
    } else {
      arr.push(value);
    }
  });

  return arr;
}

// 防抖 函数

function debounce (fun, await) {
  var timer;
  return function () {
    if (timer) clearTimeout(timer);
    timer = setTimeout(fun, await);
  }
}

// 节流
// 定时器版

function throttle (fun, await) {
  var timer;

  return function () {
    if (!timer) {
      timer = setTimeout(fun, wait);
      timer = null;
    }
  }
}

// 时间戳版本

function throttle (fun, await) {
  let previous = 0;

  return function () {
    let now =  Date.now();
    if (now - previous > wait) {
      fun();
      previous = now;
    }
  }
}
// 反转字符串（双指针）

function reverseString（s）{
  let left = 0;
  let right = s.length-1;
  while(left < right) {
    //原地修改数组
    [s[left], s[right]] = [s[right], s[left]];
    left++;
    right--;
  }
  return s;
}

// 数组去重，双层内外循环
// indexOf // ES6 Set // 先排序，后去重。。。

function unique (arr) {
  const res = [];
  for (let i = 0, s = arr.length; i < s; i++) {
    const curr = arr[i];
    if (res.indexOf(curr) === -1) {
      res.push(curr);
    }

  }
  return res;
}
// 实现数字千分位分隔符
function formatNum (num) {
  const arr  = num.toString().split('.');
  const const arr1 = arr[0].split('').reverse();
  const res = []
  for (let i = 0, s = arr1.length; i < s; i++) {
    if (i % 3 === 0 && i !== 0) {
      res.push(',');
    }
    res.push(arr1[i]);
  }
  res.reverse();
  if (arr[1]) {
    return res.join('').concat('.' + arr[1]);
  } else {
    return res.join('');
  }
}

// 或者使用 js 内置 toLocalString()

// 判断回文数字

var isPalindrome = function(x) {
    if (x < 0) return false;
    if (x === 0) return true;
    const number = +(String(x).split('').reverse().join(''));
    if (number === x) return true;
    return false
}

// 判断一个数为素数

// 质数/素数：只能被自身或者1 整除
function isPrimeNumber(num) {
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) {
      return false
    }
  }
  return true;
}

var countPrimes = function(n) {
    if(n<=2){
        return 0
    }
    let count=0
    //厄拉多塞筛法
    let flag=new Array(n).fill(0).map(v=>true)
    for(let num=2;num<n;num++){
        if(flag[num]){
            count++
            for(let help=num+num;help<n;help+=num){
                flag[help]=false
            }
        }
    }
    return count
}

```

41. 基础

* 进程 & 线程
> 进程：计算资源分配调度的基本单位。 线程：是操作系统能够进行运算调度的最小单位
* 进程间通信
* 数据链路层-》 网络层-》传输层 -》 应用层
> (物理层 -》 数据链路层 -》 网络层 -》 传输层 -》 会话层 =》 表示层 =》 应用层)
* http1.1 http2 http3 （串行请求 =》 并行请求(头部压缩)（对头阻塞问题） =》 udp）
> HTTP/1.1中的pipeline中如果有一个请求block了，那么队列后请求也统统被block住了；HTTP/2 多请求复用一个TCP连接，一旦发生丢包，就会block住所有的HTTP请求。这样的问题很讨厌。好像基本无解了。
* https：HTTP下加入SSL层
* tcp 三次握手，四次挥手
* websocket
* 常用设计模式
* MVVM
> MVC 模式，单向通信，modal（数据）， view（视图）， controller（业务逻辑） 视图交互 -》 controller -> modal（改变数据） -》 视图更新（view）  频繁操作dom，代码量大，难以维护
> MVVM 模式， 数据双向绑定 ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，ViewModel里面包含DOM Listeners和Data Bindings，DOM Listeners和Data Bindings是实现双向绑定的关键。DOM Listeners监听页面所有View层DOM元素的变化，当发生变化，Model层的数据随之变化；Data Bindings监听Model层的数据，当数据发生变化，View层的DOM元素随之变化。
> 常用框架 Vue，React，Angular
