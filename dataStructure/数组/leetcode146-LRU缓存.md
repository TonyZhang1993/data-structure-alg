
## 题目

[leetcode146](https://leetcode.cn/problems/lru-cache/description/)

```
背景: 
其实,浏览器中的缓存是一种在本地保存资源副本,它的大小是有限的,当我们请求数过多时,缓存空间会被用满,此时,继续进行网络请求就需要确定缓存中哪些数据被保留,哪些数据被移除,这就是浏览器缓存淘汰策略,最常见的淘汰策略有 FIFO(先进先出), LFU(最少使用), LRU(最近最少使用). 

LRU ( Least Recently Used : 最近最少使用 )缓存淘汰策略,故名思义,就是根据数据的历史访问记录来进行淘汰数据,其核心思想是 如果数据最近被访问过,那么将来被访问的几率也更高 ,优先淘汰最近没有被访问到的数据. 
```

题目: 
```js
运用你所掌握的数据结构,设计和实现一个 LRU (最近最少使用) 缓存机制. 它应该支持以下操作:  获取数据 get 和写入数据 put . 

获取数据 get(key) - 如果密钥 ( key ) 存在于缓存中,则获取密钥的值(总是正数),否则返回 -1 . 
写入数据 put(key, value) - 如果密钥不存在,则写入数据. 当缓存容量达到上限时,它应该在写入新数据之前删除最久未使用的数据,从而为新数据留出空间. 

进阶:

你是否可以在 O(1) 时间复杂度内完成这两种操作?
```

## 思路

利用 Map 既能保存键值对,并且能够记住键的原始插入顺序

```js
var LRUcache = function (capacity) {
  this.cache = new Map()
  this.capacity = capacity
}

/** 
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function(key) {
  if (this.cache.has(key)) {
    //  存在即更新
    let tmp = this.cache.get(key)
    this.cache.delete(key)

    this.cache.set(key, tmp)

    return tmp
  }

  return -1
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function(key, value) {
  if (this.cache.has(key)) {
    //  存在即更新(删除后加入)
    this.cache.delete(key)
  } else if (this.cache.size >= this.capacity) {
    //  超出缓存,则移除最近没有使用的
    this.cache.delete(this.cache.keys().next().value)
  }

  this.cache.set(key, value)


};

```

```js
tip:
Map.prototype.size
返回 Map 对象中的键值对数量. 

Map 实例的 entries() 方法返回一个新的 map 迭代器 (en-US)对象,该对象包含了此 map 中的每个元素的 [key, value] 对,按插入顺序排列. 

const map1 = new Map();

map1.set('0', 'foo');
map1.set(1, 'bar');
//  重复添加相同的key 后者会覆盖,不会有重复的key-value

const iterator1 = map1.entries(); //  .keys() or values

for (const entry of iterator1) {
  console.log(entry);
  // Expected output: Array ['0', 'foo']
  // Expected output: Array ["1", "bar"]
}
//  或者
console.log(iterator1.next().value);
// Expected output: Array ["0", "foo"]

console.log(iterator1.next().value);
// Expected output: Array [1, "bar"]


Map.prototype.keys() / values() 类似
返回一个新的迭代器[generator]对象,其包含 Map 对象中所有元素的键,以*插入顺序排列*. 

Generator.prototype.next()
next() 方法返回一个包含属性 done 和 value 的对象. 该方法也可以通过接受一个参数用以向生成器传值. 

function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen(); // "Generator { }"
g.next(); // "Object { value: 1, done: false }"
g.next(); // "Object { value: 2, done: false }"
g.next(); // "Object { value: 3, done: false }"
g.next(); // "Object { value: undefined, done: true }"

```