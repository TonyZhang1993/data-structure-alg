## 知识回顾

### 闭包

> 闭包让你可以在一个内层函数中访问到其外层函数的作用域

```js
function outer(x) {
  return function(y) {
    return x + y
  }
}
```

### 闭包使用场景

- 创建私有变量
- 延长变量的生命周期

> 一般函数的词法环境在函数返回后就被销毁，但是闭包会保存对创建时所在词法环境的引用，即便创建时所在的执行上下文被销毁，但创建时所在词法环境依然存在，以达到延长变量的生命周期的目的

```js
//  例子1
function makeSizer(size) {
  return function() {
    document.body.style.fontSize = size + 'px';
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);

document.getElementById('size-12').onclick = size12;
document.getElementById('size-14').onclick = size14;
document.getElementById('size-16').onclick = size16;
```

在JavaScript中，没有支持声明私有变量，但我们可以使用闭包来模拟私有方法

```js
var makeCounter = function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  }
};

var Counter1 = makeCounter();
var Counter2 = makeCounter();
console.log(Counter1.value()); /* logs 0 */
Counter1.increment();
Counter1.increment();
console.log(Counter1.value()); /* logs 2 */
Counter1.decrement();
console.log(Counter1.value()); /* logs 1 */
console.log(Counter2.value()); /* logs 0 */
//  上述通过使用闭包来定义公共函数，并令其可以访问私有函数和变量，这种方式也叫模块方式

//  例如计数器、延迟调用、回调等闭包的应用，其核心思想还是创建私有变量和延长变量的生命周期

//  如果不是某些特定任务需要使用闭包，在其它函数中创建函数是不明智的，因为闭包在处理速度和内存消耗方面对脚本性能具有负面影响

```

### 高阶函数
高阶函数 - 指的是能够接受函数作为参数或返回函数作为结果的函数
```js
// 定义接收函数的高阶函数
function times(n, f) {
  for (var i = 0; i < n; i++) {
    f(i);
  }
}

// 定义返回函数的高阶函数
function add(x) {
  return function(y) {
    return x + y;
  };
}
```

### 闭包链式调用
链式调用是一种编程风格，通过在对象上连续调用多个方法，使得代码看起来像是一条链条. 每个方法都会返回对象本身，使得可以在同一个表达式中连续调用多个方法. 如 Promise、lodash库都有体现这样的风格. 

```js
function Chainable(value) {
  // result 维护当前的运算值
  let result = value;

  this.square = function() {
    result = Math.pow(result, 2);
    return this;
  };

  this.cube = function() {
    result = Math.pow(result, 3);
    return this;
  };

  this.negate = function() {
    result = -result;
    return this;
  };

  this.random = function() {
    result = Math.random() * result;
    return this;
  };

  this.value = function() {
    return result;
  };
}

// 创建可链式调用对象
const chain = new Chainable(5);

const value = chain.square().cube().negate().random().value();
console.log(value); // 示例输出: -239.40167432539412

```

### 闭包 发布订阅模式
发布订阅模式是一种常见的设计模式，它用于在不同的对象之间建立松散耦合的联系. 该模式包含两个核心概念: 发布者(Publisher）和订阅者(Subscriber）. 发布者负责发布事件和通知订阅者，而订阅者则负责订阅事件并接收通知. 

```js
function eventEmitter() {
  // events 维护各事件以及对应的订阅者
  const events = {};

  // on 函数绑定订阅者(callback）至相应的事件(eventName）
  function on(eventName, callback) {
    // 如果 events 不存在该事件则创建该事件并赋值一个空数组存放订阅者
    events[eventName] = events[eventName] || [];
    // 存入订阅者
    events[eventName].push(callback);
  }

  // emit 函数发布事件(eventName），并传递相关参数(args）给订阅者
  function emit(eventName, ...args) {
    // 赋值订阅者数组
    const callbacks = events[eventName];
    if (callbacks) {
      callbacks.forEach(callback => callback(...args));
    }
  }

  // off 函数解除订阅关系
  function off(eventName, callback) {
    if (!callback) {
      // 订阅者为空，直接删除事件
      delete events[eventName];
    } else {
      // 订阅者不为空，筛选订阅者
      events[eventName] = events[eventName].filter(cb => cb !== callback);
    }
  }

  return { on, emit, off };
}

// 使用示例
const emitter = eventEmitter();

function handler1(name) {
  console.log(`${name} says hello from handler1`);
}

function handler2(name) {
  console.log(`${name} says hello from handler2`);
}

emitter.on('hello', handler1);
emitter.on('hello', handler2);

emitter.emit('hello', 'Alice'); // 输出 "Alice says hello from handler1" 和 "Alice says hello from handler2"

emitter.off('hello', handler1);
emitter.emit('hello', 'Bob'); // 只输出 "Bob says hello from handler2"

```

### 闭包缺点及解决方案
1 内存泄露
```js
function outer() {
  let count = 0;
  
  function inner() {
    count++;
    console.log(count);
  }
  
  return inner;
}

// 创建闭包
const fn = outer();

// 调用多次，但没有及时解绑闭包
fn();
fn();
fn();
fn();
// ...

// 结果: count 变量无法被释放，造成内存泄漏

```
解决方案: 
将闭包函数设置为 null: fn = null;
将闭包函数重新赋值: fn = outer();

2 闭包涉及作用域链查找，性能相较直接访问局部、全局变量要低一些，在一些频繁调用或要求高性能的场景不适用

### 柯里化函数 
```js
//  柯里化函数 - 目的在于避免频繁调用具有相同参数函数的同时，又能够轻松的重用
// 假设我们有一个求长方形面积的函数
function getArea(width, height) {
    return width * height
}
// 如果我们碰到的长方形的宽老是10
const area1 = getArea(10, 20)
const area2 = getArea(10, 30)
const area3 = getArea(10, 40)

// 改造 - 我们可以使用闭包柯里化这个计算面积的函数
function getArea(width) {
    return height => {
        return width * height
    }
}

const getTenWidthArea = getArea(10)
// 之后碰到宽度为10的长方形就可以这样计算面积
const area1 = getTenWidthArea(20)

// 而且如果遇到宽度偶尔变化也可以轻松复用
const getTwentyWidthArea = getArea(20)


// 经典题目
function add() {
  // 创建空数组来维护所有要 add 的值
  const args = []
  // curry 函数，存入每次调用传入的参数
  function curried(...nums) {
    if (nums.length === 0) {
      // 长度为0，说明调用结束，返回 args 的 sum
      return args.reduce((pre, cur) => pre + cur, 0);
    } else {
      // 长度不为0，将传入的参数存入 args，返回 curried函数给下一次调用
      args.push(...nums);
      return curried;
    }
  }

  // 一开始给 curried 传递 add 接收到的参数 arguments
  return curried(...Array.from(arguments));
}

console.log(add(1, 2)(1)()); // 输出: 4
console.log(add(1)(2)(3)(4)()); // 输出: 10
console.log(add(5)()); // 输出: 5

```

### 作用域链

> 词法作用域，又叫静态作用域，变量被创建时就确定好了，而非执行阶段确定的. 也就是说我们写好代码时它的作用域就确定了，JavaScript 遵循的就是词法作用域

```js
var a = 2;
function foo(){
    console.log(a)
}
function bar(){
    var a = 3;
    foo();
}
bar() //  均输出2
```

> 作用域链: 当在Javascript中使用一个变量的时候，首先Javascript引擎会尝试在当前作用域下去寻找该变量，如果没找到，再到它的上层作用域寻找，以此类推直到找到该变量或是已经到了全局作用域

```js
var sex = '男';
function person() {
    var name = '张三';
    function student() {
        var age = 18;
        console.log(name); // 张三
        console.log(sex); // 男 
    }
    student();
    console.log(age); // Uncaught ReferenceError: age is not defined
}
person();
```

上述代码主要主要做了以下工作: 

student函数内部属于最内层作用域，找不到name，向上一层作用域person函数内部找，找到了输出“张三”
student内部输出sex时找不到，向上一层作用域person函数找，还找不到继续向上一层找，即全局作用域，找到了输出“男”
在person函数内部输出age时找不到，向上一层作用域找，即全局作用域，还是找不到则报错

### this

> 在绝大多数情况下，函数的调用方式决定了 this 的值(运行时绑定）. this 不能在执行期间被赋值，并且在每次函数被调用时 this 的值也可能会不同. 
<br>ES5 引入了 bind 方法来设置函数的 this 值，而不用考虑函数如何被调用的. 
<br>ES2015 引入了箭头函数，箭头函数不提供自身的 this 绑定(this 的值将保持为闭合词法上下文的值）. [箭头函数内的this值继承自外围作用域. ]

> this永远指向的是最后调用它的对象;  this指的是函数运行时所在的环境. this就出现了，它的设计目的就是在函数体内部，指代函数当前的运行环境. 



reference: [谈谈THIS对象的理解](https://vue3js.cn/interview/JavaScript/this.html#%E4%B8%80%E3%80%81%E5%AE%9A%E4%B9%89)
