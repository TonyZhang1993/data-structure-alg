```js
Vue2 & Vue3 生命周期
  setup() {}  //  vue3
  beforeCreate() {},  //  实例尚未初始化完成,无法访问this
  created() {}, //  实例初始化完成,k可以访问this,还未挂载dom
  
  beforeMount() {}, //  虚拟dom 创建完成
  mounted() {}, //  渲染为真实 dom 并插入页面中
  
  beforeUpdate() {},  //  数据更新前调用
  updated() {}, //  页面重新渲染后调用
  
  // Vue2 里叫 beforeDestroy
  beforeUnmount() {}, //  dom 还未销毁,已从父组件移除
  // Vue2 里叫 destroyed
  unmounted() {}, //  销毁实例,清楚事件监听/定时器回收等

<script setup>
import {
  onBeforeMount,
  onMounted,

  onBeforeUpdate,
  onUpdated,

  onBeforeUnmount,
  onUnmounted,
} from 'vue'

Vue3 里,除了将两个 destroy 相关的钩子,改成了 unmount,剩下的需要注意的,就是在 <script setup> 中,不能使用 beforeCreate 和 created 两个钩子. 

2.vue响应式原理
Vue在初始化数据时,会使用Object.defineProperty重新定义data中的所有属性,当页面使用对应属性时,首先会进行依赖收集(收集当前组件的watcher)如果属性发生变化会通知相关依赖进行更新操作(发布订阅). 
Vue3.x改用Proxy替代Object.defineProperty. 因为Proxy可以直接监听对象和数组的变化,并且有多达13种拦截方法. 并且作为新标准将受到浏览器厂商重点持续的性能优化. 

同时Proxy 并不能监听到内部深层次的对象变化,而 Vue3 的处理方式是在getter 中去递归响应式,这样的好处是真正访问到的内部对象才会变成响应式,而不是无脑递归


Vue3.x 将使用 Proxy 取代 Vue2.x 版本的 Object.defineProperty
1. Object.defineProperty只能劫持对象的属性, 而 Proxy 是直接代理对象
由于Object.defineProperty只能劫持对象属性,需要遍历对象的每一个属性,如果属性值也是对象,就需要递归进行深度遍历. 但是 Proxy 直接代理对象, 不需要遍历操作
2. Object.defineProperty对新增属性需要手动进行Observe
因为Object.defineProperty劫持的是对象的属性,所以新增属性时,需要重新遍历对象, 对其新增属性再次使用Object.defineProperty进行劫持. 也就是 Vue2.x 中给数组和对象新增属性时,需要使用$set才能保证新增的属性也是响应式的, $set内部也是通过调用Object.defineProperty去处理的. 

3.vue3和vue2的区别

源码组织方式变化: 使用 TS 重写
支持 Composition API: 基于函数的API,更加灵活组织组件逻辑(vue2用的是options api)
响应式系统提升: Vue3中响应式数据原理改成proxy,可监听动态新增删除属性,以及数组变化
编译优化: vue2通过标记静态根节点优化diff,Vue3 标记和提升所有静态根节点,diff的时候只需要对比动态节点内容
打包体积优化: 移除了一些不常用的api(inline-template, filter)
生命周期的变化: 使用setup代替了之前的beforeCreate和created
Vue3 的 template 模板支持多个根标签
Vuex状态管理: 创建实例的方式改变,Vue2为new Store , Vue3为createStore
Route 获取页面实例与路由信息: vue2通过this获取router实例,vue3通过使用 getCurrentInstance/ useRoute和useRouter方法获取当前组件实例
Props 的使用变化: vue2 通过 this 获取 props 里面的内容,vue3 直接通过 props
父子组件传值: vue3 在向父组件传回数据时,如使用的自定义名称,如 backData,则需要在 emits 中定义一下

MVVM是Model-View-ViewModel缩写,也就是把MVC中的Controller演变成ViewModel. Model层代表数据模型,View代表UI组件,ViewModel是View和Model层的桥梁,数据会绑定到viewModel层并自动将数据渲染到页面中,视图变化的时候会通知viewModel层更新数据. 

v-model本质就是一个语法糖,可以看成是value + input方法的语法糖.  可以通过model属性的prop和event属性来进行自定义. 

7.vue2.x 和 vue3.x 渲染器的 diff 算法分别说一下?
简单来说,diff算法有以下过程

同级比较,再比较子节点
先判断一方有子节点一方没有子节点的情况(如果新的children没有子节点,将旧的子节点移除)
比较都有子节点的情况(核心diff)
递归比较子节点

Vue 2.x 使用的是 Virtual DOM 和基于双向比较的 diff 算法来更新 DOM. 当数据发生变化时,Vue 2.x 会生成新的 Virtual DOM 树,并与旧的 Virtual DOM 树进行比较,找出差异并更新到实际的 DOM 上. Vue 2.x 的 diff 算法是一种逐级比较的算法,它会对比两棵树的节点,找出差异并进行更新,但在某些情况下可能会比较耗费性能. 

Vue 3.x 引入了全新的响应式系统和编译器优化,以及基于 Proxy 的响应式数据追踪机制. Vue 3.x 的渲染器使用了一种基于静态分析的优化 diff 算法,称为静态树提升(Static Tree Hoisting),这种算法利用编译时的静态分析结果,将更新过程中的操作精确化,只对需要更新的部分进行 diff 操作,从而提高了性能. 

## keep-alive可以实现组件缓存,当组件切换时不会对当前组件进行卸载. 
keep-alive 内部的组件切回时,不用重新创建组件实例,而直接使用缓存中的实例,一方面能够避免创建组件带来的开销,另一方面可以保留组件的状态. 

* 一般结合路由和动态组件一起使用,用于缓存组件; 
* 提供 include 和 exclude 属性,两者都支持字符串或正则表达式, include 表示只有名称匹配的组件会被缓存,exclude 表示任何名称匹配的组件都不会被缓存 ,其中 exclude 的优先级比 include 高; 
* 受 keep-alive 的影响,其内部所有嵌套的组件都具有两个生命周期钩子函数,分别是 activated 和 deactivated,它们分别在组件激活和失活时触发. 第一次 activated 触发是在 mounted 之后

还提供了 max 属性,通过它可以设置最大缓存数,当缓存的实例超过该数时,vue 会移除最久没有使用的组件缓存. 
keep-alive的中还运用了LRU(Least Recently Used)算法. 

keep-alive 在内部维护了一个 key 数组和一个缓存对象

key 数组记录目前缓存的组件 key 值,如果组件没有指定 key 值,则会为其自动生成一个唯一的 key 值
cache 对象以 key 值为键,vnode 为值,用于缓存组件对应的虚拟 DOM
- 当缓存数量超过 max 数值时,keep-alive 会移除掉 key 数组的第一个元素. 

如果为一个组件包裹了 keep-alive,那么它会多出两个生命周期: deactivated, activated. 同时,beforeDestroy 和 destroyed 就不会再被触发了,因为组件不会被真正销毁. 
[在set up 中 则对应  onActivated() 和 onDeactivated()]

当组件被换掉时,会被缓存到内存中, 触发 deactivated 生命周期; 当组件被切回来时,再去缓存里找这个组件, 触发 activated 钩子函数. 

https://cn.vuejs.org/guide/built-ins/keep-alive.html#keepalive

浏览器缓存淘汰策略,最常见的淘汰策略有 FIFO(先进先出), LFU(最少使用), LRU(最近最少使用). 

LRU ( Least Recently Used : 最近最少使用 ),其核心思想是 如果数据最近被访问过,那么将来被访问的几率也更高 ,优先淘汰最近没有被访问到的数据. 

LRU 算法示例
let LRU = function (capacity) {
  this.keys = []  //  前缓存的组件 key 值
  this.cache = Object.create(null)  //  以 key 值为键,vnode 为值,用于缓存组件对应的虚拟 DOM
  this.capacity = capacity  //  最大容量
}

LRU.prototype.get = function (key) {
  if (this.cache[key]) {
    //  将该key 移至末尾
    remove(this.keys, key)
    this.keys.push(key)

    return this.cache[key]
  }

  return -1
}

LRU.prototype.set = function (key, value) {
  if (this.cache[key]) {
    //  存在即更新
    //  将该key 移至末尾
    remove(this.keys, key)
    this.keys.push(key)

    this.cache[key] = value
  } else {
    //  不存在即设置
    this.keys.push(key)
    this.cache[key] = value
    // 判断缓存是否已超过最大值
    if (this.keys.length > this.capacity) {
      removeCache(this.cache, this.keys, this.keys[0])
    }
  }
}
//  移除数组中的key
function remove(keys, key) {
  if (keys.length > 0) {
    let index = keys.indexOf(key)
    if (index > -1) {
      keys.splice(index, 1)
    }
  }

}
//  移除cache 目标key 以及 keys 中key
function removeCache(cache, keys, key) {
  cache[key] = 0
  remove(keys, key)
}


12. nextTick 的作用是什么?他的实现原理是什么?
nextTick 是 Vue.js 提供的一个异步更新 DOM 的方法,它的作用是将回调函数延迟到下次 DOM 更新循环之后执行. 在 Vue.js 中,数据变化后 DOM 的更新是异步执行的,而 nextTick 可以让我们在 DOM 更新之后执行一些操作,确保我们在获取更新后的 DOM 状态. 

Vue.js 中使用了一下方法来实现 nextTick
Promise
MutationObserver
setImmediate
如果以上都不行则采用setTimeout

nextTick 的作用是确保在 DOM 更新之后执行回调函数,其实现原理是利用 JavaScript 的事件循环机制,将回调函数异步放入微任务队列或者宏任务队列中,以确保在下次 DOM 更新循环中执行. 

## render 是字符串模板的一种替代, 编程式地创建组件虚拟 DOM 树的函数

## vue compiler
在使用 vue 的时候,我们有两种方式来创建我们的 HTML 页面,第一种情况,也是大多情况下,我们会使用模板 template 的方式,因为这更易读易懂也是官方推荐的方法; 第二种情况是使用 render 函数来生成 HTML,它比 template 更接近最终结果. 

complier 的主要作用是*解析模板,生成渲染模板的 render*, 而 render 的作用主要是为了生成 *VNode*
complier 主要分为 3 大块: 
parse: 接受 template 原始模板,按着模板的节点和数据生成对应的 AST语法树
optimize: 遍历 AST 的每一个节点,标记静态节点,这样就知道哪部分不会变化,于是在页面需要更新时,通过 diff 减少去对比这部分DOM,提升性能
generate 把前两步生成完善的 AST render 字符串,然后将 render 字符串通过 new Function 的方式转换成渲染函数

## vue 模版编译的原理是什么

Vue 的编译过程就是将 template 转化为 render 函数的过程. 会经历以下阶段: 
- 首先解析模版,生成 AST 语法树(一种用 JavaScript 对象的形式来描述整个模板).  使用大量的正则表达式对模板进行解析,遇到标签, 文本的时候都会执行对应的钩子进行相关处理. 

- Vue 的数据是响应式的,但其实模板中并不是所有的数据都是响应式的. 有一些数据首次渲染后就不会再变化,对应的 DOM 也不会变化. 那么优化过程就是深度遍历 AST 树,按照相关条件对树节点进行标记. 这些被标记的节点(静态节点)我们就可以跳过对它们的比对,对运行时的模板起到很大的优化作用. 

- 编译的最后一步是将优化后的 AST 树转换为可执行的代码. 

vue 中的模板 template 无法被浏览器解析并渲染,因为这不属于浏览器的标准,不是正确的 HTML 语法,所有需要将 template 转化成一个 JavaScript 函数,这样浏览器就可以执行这一个函数并渲染出对应的 HTML 元素,就可以让视图跑起来了,这一个转化的过程,就成为模板编译. 模板编译又分三个阶段,解析 parse,优化 optimize,生成 generate,最终生成可执行函数 render. 

解析阶段: 使用大量的正则表达式对 template 字符串进行解析,将标签, 指令, 属性等转化为抽象语法树 AST. 
优化阶段: 遍历 AST,找到其中的一些静态节点并进行标记,方便在页面重渲染的时候进行 diff 比较时,直接跳过这一些静态节点,优化 runtime 的性能. 
生成阶段: 将最终的 AST 转化为 渲染函数代码,最终输出为JS函数代码


## Vue 中的 VNodes 是虚拟 DOM 的概念. VNode 是 Virtual Node 的缩写,表示虚拟 DOM 树中的一个节点. 在 Vue 中,VNode 是用来描述真实 DOM 结构的 JavaScript 对象,它包含了节点的标签名, 数据, 子节点等信息,并且可以通过 VNodes 构建出整个虚拟 DOM 树. 


## watch 适用于监听数据变化并执行特定操作的场景,而 computed 适用于根据其他数据派生出新数据的场景. 
watch 是一对多(监听某一个值变化,执行对应操作); computed 是多对一(监听属性依赖于其他属性)
watch 监听函数接收两个参数,第一个是最新值,第二个是输入之前的值; 

watch(() => state.count, (newVal, oldVal) => {
  console.log(`count 从 ${oldVal} 变为 ${newVal}`);
},
  {
    immediate: true,
    deep: true
  });

const doubleCount = computed(() => state.count * 2);


## 在 vue 中修饰符可以分为 3 类: 

事件修饰符
按键修饰符
表单修饰符

事件修饰符
在事件处理程序中调用 event.preventDefault 或 event.stopPropagation 方法是非常常见的需求. 尽管可以在 methods 中轻松实现这点,但更好的方式是: methods 只有纯粹的数据逻辑,而不是去处理 DOM 事件细节. 

vue 为 v-on 提供了事件修饰符. 通过由点 . 表示的指令后缀来调用修饰符. 
常见的事件修饰符如下: 

.stop: 阻止冒泡. 
.prevent: 阻止默认事件. 
.capture: 使用事件捕获模式. 
.self: 只在当前元素本身触发. 
.once: 只触发一次. 
.passive: 默认行为将会立即触发. 

按键修饰符
除了事件修饰符以外,在 vue 中还提供了有鼠标修饰符,键值修饰符,系统修饰符等功能. 

.left: 左键
.right: 右键
.middle: 滚轮
.enter: 回车
.tab: 制表键
.delete: 捕获 "删除" 和 "退格" 键
.esc: 返回
.space: 空格
.up: 上
.down: 下
.left: 左
.right: 右
.ctrl: ctrl 键
.alt: alt 键
.shift: shift 键
.meta: meta 键

表单修饰符

vue 同样也为表单控件也提供了修饰符,常见的有 .lazy,  .number 和 .trim. 

.lazy: 在文本框失去焦点时才会渲染
.number: 将文本框中所输入的内容转换为number类型
.trim: 可以自动过滤输入首尾的空格


## vue 性能优化
- 编码阶段
如果需要使用 v-for 给每项元素绑定事件时使用事件代理
SPA 页面采用 keep-alive 缓存组件
在更多的情况下,使用 v-if 替代 v-show
key 保证唯一
使用路由懒加载, 异步组件
防抖, 节流
第三方模块按需导入
长列表滚动到可视区域动态加载
图片懒加载

- SEO 优化
预渲染
服务端渲染 SSR

- 打包优化
压缩代码
Tree Shaking/Scope Hoisting
使用 cdn 加载第三方模块
多线程打包 happypack
splitChunks 抽离公共文件
sourceMap 优化

-用户体验
骨架屏
PWA
还可以使用缓存(客户端缓存, 服务端缓存)优化, 服务端开启 gzip 压缩等. 

## Tree-shaking 是一种用于剔除 JavaScript 应用中未使用代码的优化技术,通过静态分析代码的依赖关系,去除生产环境中不会被使用的代码,从而减小最终打包文件的体积. Tree-shaking 主要应用于模块化开发环境,通常与 ES6 模块系统(如 import 和 export)结合使用. 

Tree-shaking 的原理是通过静态代码分析,识别和标记那些在运行时不会被引用的代码,然后在打包阶段将这些未引用的代码从最终的输出中去除. 这个过程需要构建工具(如 Webpack 或 Rollup)的支持,以便能够正确识别哪些代码是未使用的. 

以下是一个简单的例子,展示了如何使用 Tree-shaking 去除未使用的代码: 

// math.js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

// index.js
import { add } from './math.js';

console.log(add(2, 3));
在上面的例子中,虽然 math.js 中定义了 subtract 函数,但是在 index.js 中并没有使用它. 通过 Tree-shaking 技术,在打包过程中构建工具会识别到 subtract 函数是未被使用的代码,从而将其剔除,最终输出的打包文件中只包含使用到的 add 函数,减小了打包文件的体积. 

需要注意的是,要确保 Tree-shaking 能够正常工作,代码需要遵循一些规则,比如导出的模块必须是纯函数,不能有副作用,以便构建工具可以准确地判断哪些代码是可以被安全地删除的. 

## 优化首屏 

优化首屏加载可以从这几个方面开始: 

- 请求优化: CDN 将第三方的类库放到 CDN 上,能够大幅度减少生产环境中的项目体积,另外 CDN 能够实时地根据网络流量和各节点的连接, 负载状况以及到用户的距离和响应时间等综合信息将用户的请求重新导向离用户最近的服务节点上. 
- 缓存: 将长时间不会改变的第三方类库或者静态资源设置为强缓存,将 max-age 设置为一个非常长的时间,再将访问路径加上哈希达到哈希值变了以后保证获取到最新资源,好的缓存策略有助于减轻服务器的压力,并且显著的提升用户的体验
- gzip: 开启 gzip 压缩,通常开启 gzip 压缩能够有效的缩小传输资源的大小. 
- http2: 如果系统首屏同一时间需要加载的静态资源非常多,但是浏览器对同域名的 tcp 连接数量是有限制的(chrome 为 6 个)超过规定数量的 tcp 连接,则必须要等到之前的请求收到响应后才能继续发送,而 http2 则可以在多个 tcp 连接中并发多个请求没有限制,在一些网络较差的环境开启 http2 性能提升尤为明显. 
- 懒加载: 当 url 匹配到相应的路径时,通过 import 动态加载页面组件,这样首屏的代码量会大幅减少,webpack 会把动态加载的页面组件分离成单独的一个 chunk.js 文件
- 预渲染: 由于浏览器在渲染出页面之前,需要先加载和解析相应的 html, css 和 js 文件,为此会有一段白屏的时间,可以添加loading,或者骨架屏幕尽可能的减少白屏对用户的影响体积优化
- 合理使用第三方库: 对于一些第三方 ui 框架, 类库,尽量使用按需加载,减少打包体积
使用可视化工具分析打包后的模块体积: webpack-bundle- analyzer 这个插件在每次打包后能够更加直观的分析打包后模块的体积,再对其中比较大的模块进行优化
- 提高代码使用率: 利用代码分割,将脚本中无需立即调用的代码在代码构建时转变为异步加载的过程
- 封装: 构建良好的项目架构,按照项目需求就行全局组件,插件,过滤器,指令,utils 等做一 些公共封装,可以有效减少我们的代码量,而且更容易维护资源优化
- 图片懒加载: 使用图片懒加载可以优化同一时间减少 http 请求开销,避免显示图片导致的画面抖动,提高用户体验
- 使用 svg 图标: 相对于用一张图片来表示图标,svg 拥有更好的图片质量,体积更小,并且不需要开启额外的 http 请求
- 压缩图片: 可以使用 image-webpack-loader,在用户肉眼分辨不清的情况下一定程度上压缩图片

## 组件中写 name 选项有哪些好处

在组件自己的模板中递归引用自己时
在 Vue 开发者工具中的组件树显示时
在组件抛出的警告追踪栈信息中显示时
[在 3.2.34 或以上的版本中,使用 <script setup> 的单文件组件会自动根据文件名生成对应的 name 选项,即使是在配合 <KeepAlive> 使用时也无需再手动声明. ]


## ref 的作用是被用来给元素或子组件注册引用信息. 引用信息将会注册在父组件的 $refs 对象上. 其特点是: 

如果在普通的 DOM 元素上使用,引用指向的就是 DOM 元素
如果用在子组件上,引用就指向组件实例

所以常见的使用场景有: 

基本用法,本页面获取 DOM 元素
获取子组件中的 data
调用子组件中的方法

在 Vue 3 中,可以使用 ref 函数为元素或子组件注册引用信息. ref 函数可以创建一个响应式的引用对象,可以用来引用 DOM 元素, 组件实例或其他数据. 
<template>
  <div ref="myElement">这是一个 DOM 元素</div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const myElement = ref(null);

    onMounted(() => {
      console.log('DOM 元素: ', myElement.value);
    });

    return {
      myElement
    };
  }
}
</script>

在 Vue 3 中,接口请求一般可以放在组件的生命周期钩子函数 onMounted 

在 Vue 2 中,接口请求一般会在 created 或者 mounted 这两个生命周期钩子中. 
```