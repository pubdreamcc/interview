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
23. 
