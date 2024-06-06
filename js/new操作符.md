## 知识回顾

- new 通过构造函数 Person 创建出来的实例可以访问到构造函数中的属性
- new 通过构造函数 Person 创建出来的实例可以访问到构造函数原型链中的属性(即实例与构造函数通过原型链连接了起来)

构造函数如果返回值为一个对象,那么这个返回值会被正常使用
```js
function Test(name) {
  this.name = name
  console.log(this) // Test { name: 'xxx' }
  return { age: 26 }  //  return 1 则不起作用; return null 此时new仍然指向实例对象

}
const t = new Test('xxx')
console.log(t) // { age: 26 }
console.log(t.name) // 'undefined'
```

```js
var obj = new fn();
//  new 操作符到底做了什么
//  首先,new操作符为我们创建一个新的空对象,然后this变量指向该对象,
//  其次,空对象的原型指向函数的原型对象,
//  最后,改变构造函数内部的this的指向

var obj = {};
obj.__proto__ = fn.prototype;
fn.call(obj);
```
！！！！！
- instanceof: 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上[instanceof只能正确判断引用数据类型]<br>
`object instanceof constructor`
- hasOwnProperty: 检测一个属性是存在于实例中, 不计原型链中属性
- in: 判断实例中或者原型中是否有该属性; 

```js
function Person() {}
const person1 = new Person()

person1 instanceof Person //  true
person1.__proto__ === Person.prototype
Person.prototype.constructor = Person

person1 instanceof Object  //true  判断Object的prototype是否出现在person1的原型链上
person1.hasOwnProperty('name')  //true  检测一个属性是存在于实例中
'name' in person1   //true   判断实例中或者原型中是否有该属性; 
```

手写new
```js
function mynew(func, ...args) {
  // 1.创建一个新对象
  var obj = {}
  // 2.新对象原型指向构造函数原型对象
  obj.__proto__ = func.prototype
  // 3. 将这个新对象作为 this 上下文, 并调用构造函数  
  const result = func.apply(obj, args)  //  args 为数组
  // 4. 如果构造函数返回的是一个对象, 则返回这个对象；否则返回新创建的对象  
  return result instanceof Object ? result : obj
}

//  注意 {} instanceof Object 报错
//  因为{}不是通过new Object 创建的,是普通对象,无prototype属性
```

reference: [new操作符](https://vue3js.cn/interview/JavaScript/new.html#%E4%B8%80%E3%80%81%E6%98%AF%E4%BB%80%E4%B9%88)