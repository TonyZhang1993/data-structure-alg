## 原理
```js
person instanceof Object
```
判断`Object`的`prototype`是否在`a`的原型链上

## 实现
```js
function myInstanceof(obj, target) {
  const tmp = obj.__proto__

  if (tmp) {
    if (tmp === target.prototype) {
      return true
    } else {
      return myInstanceof(tmp, target)
    }
  } else {
    return false  //  到顶了 Object.prototype.__proto__
  }
}
```