## 经验之谈

候选人的挑战在于面对问题时，如何在一两分钟内作出有效回答。有效回答是指：用两三句话对问题作出概括性回答，并引导面试官对回答中提到的关键词进一步深入提问。

复习阶段应该有意识的复习高频内容。

面试时考察候选人对知识点的理解和运用，需要候选人在有限时间内言简意赅的回答问题。

在有限时间内，无法对高频知识点进行吸收和总结，只能不断地在面试环节中试错，机会成本和时间成本极高。

![alt text](image-2.png)

本书主要整理了高频面试题和对应篇幅可控的答案。高频题是为了提高候选人复习效率，篇幅可控的答案则节省候选人的阅读时间

「回答关键点」作为高度概括的总结性语言，可用于第一时间回答面试官的问题；「知识点深入」以递进方式深入解析，可作为引导面试官进一步提问的方向。

## 模拟题1

### 浏览器跨域

什么是跨域： *跨域问题的来源是浏览器为了请求安全而引入的基于同源策略的安全特性。当页面和请求的协议、主机名或端口不同时，浏览器判定两者不同源，即为跨域请求。需要注意的是跨域是浏览器的限制，服务端并不受此影响。当产生跨域时，我们可以通过 JSONP、CORS、postMessage 等方式解决。*

![alt text](image-3.png)

一个 origin 由协议（Protocol）、主机名（Host）和端口（Port）组成，这三块也是同源策略的判定条件，只有当协议、主机名和端口都相同时，浏览器才判定两者是同源关系，否则即为跨域。

⚠️注意：二级域名不同也被视为跨域

- 方法1：
CORS (Cross-Origin Resource Sharing) 【服务端/后端】
CORS 是目前最为广泛的解决跨域问题的方案。方案依赖服务端/后端在响应头中添加 Access-Control-Allow-* 头，告知浏览器端通过此请求

CORS 引入了以下几个以 Access-Control-Allow-* 开头：

Access-Control-Allow-Origin 表示允许的来源
Access-Control-Allow-Methods 表示允许的请求方法
Access-Control-Allow-Headers 表示允许的请求头
Access-Control-Allow-Credentials 表示允许携带认证信息

- 方法2
反向代理 【服务端/后端】
在页面同域下配置一套反向代理服务，页面请求同域的服务端，服务端请求上游的实际的服务端，之后将结果返回给前端。

- 方法3 【前端/后端】
JSONP 是一个相对古老的跨域解决方案，只支持 GET 请求。*主要是利用了浏览器加载 JavaScript 资源文件时不受同源策略的限制而实现跨域获取数据。*

```js
1 全局注册一个函数，例如：window.getHZFEMember = (num) => console.log('HZFE Member: ' + num);。
2 构造一个请求 URL，例如：https://hzfe.org/api/hzfeMember?callback=getHZFEMember。
3 生成一个 <script> 并把 src 设为上一步的请求 URL 并插入到文档中，如 <script src="https://hzfe.org/api/hzfeMember?callback=getHZFEMember" />。
4 服务端构造一个 JavaScript 函数调用表达式并返回，例如：getHZFEMember(17)。
5 浏览器加载并执行以上代码，输出 HZFE Member: 17。
```

- 方法4 
postMessage

```js
1. 发送消息
在发送端（例如，父窗口）使用 postMessage 发送消息：

// 发送消息到目标窗口
const targetWindow = document.getElementById('iframe').contentWindow;
const message = { text: 'Hello from parent' };
targetWindow.postMessage(message, 'https://target-origin.com');
targetWindow 是目标窗口对象，可以是 iframe、window.open 打开的窗口等。
message 是要发送的消息，可以是字符串或对象。
https://target-origin.com 是目标窗口的来源（origin）。你应该指定准确的来源以确保安全。

2. 接收消息
在接收端（例如，iframe）使用 message 事件监听器接收消息：

window.addEventListener('message', (event) => {
  // 验证消息来源
  if (event.origin !== 'https://expected-origin.com') {
    return;
  }

  // 处理消息
  console.log('Received message:', event.data);
});
event.origin 用于检查消息的来源，以确保消息来自预期的域。
event.data 包含发送的消息内容。

```

- 方法4
document.domain
用途：document.domain 允许设置共享相同主域的不同子域之间的跨域访问。

在子域中设置 document.domain 为主域：

document.domain = 'example.com';
设置后，同一主域下的其他子域（如 sub.example.com）可以访问该文档的内容。

Tip：LocalStorage 和 SessionStorage 同样受到同源策略的限制。

### 浏览器重排 & 重绘

浏览器渲染大致分为四个阶段，其中在解析 HTML 后，会依次进入 Layout 和 Paint 阶段。样式或节点的更改，以及对布局信息的访问等，都有可能导致重排和重绘。而重排和重绘的过程在主线程中进行，这意味着不合理的重排重绘会导致渲染卡顿，用户交互滞后等性能问题。

![alt text](NSFileHandle.png)

从上面这个图上，我们可以看到，浏览器渲染过程如下：
1. 解析HTML，生成DOM树，解析CSS，生成CSSOM树
2. 将DOM树和CSSOM树结合，生成渲染树(Render Tree)
3. Layout(回流):根据生成的渲染树，进行回流(Layout)，得到节点的几何信息（位置，大小）
4. Painting(重绘):根据渲染树以及回流得到的几何信息，得到节点在屏幕上绘制的实际像素大小
5. Display:将像素发送给GPU，展示在页面上。

注意：渲染树只包含可见的节点

!!引起重排/重绘的常见操作!!
外观有变化时，会导致*重绘*。相关的样式属性如 color opacity 等。
布局结构或节点内容变化时，会导致*重排*。相关的样式属性如 height float position 等。
盒子尺寸和类型。
定位方案（正常流、浮动和绝对定位）。
文档树中元素之间的关系。
外部信息（如视口大小等）。
获取布局信息时，会导致重排。相关的方法属性如 offsetTop getComputedStyle 等。

回流这一阶段主要是计算节点的位置和几何信息，那么当页面布局和几何信息发生变化的时候，就需要回流。比如以下情况：
* 添加或删除可见的DOM元素
* 元素的位置发生变化
* 元素的尺寸发生变化（包括外边距、内边框、边框大小、高度和宽度等）
* 内容发生变化，比如文本变化或图片被另一个不同尺寸的图片所替代。
* 页面一开始渲染的时候（这肯定避免不了）
* 浏览器的窗口尺寸变化（因为回流是根据视口的大小来计算元素的位置和大小的）
注意：回流一定会触发重绘，而重绘不一定会回流

!!如何减少！！！
解决方案
* 对 DOM 进行批量写入和读取（通过虚拟 DOM 或者 DocumentFragment 实现）。【创建一个documentFragment，在它上面应用所有DOM操作，最后再把它添加到文档中。】
* 避免对样式频繁操作，了解常用样式属性触发 Layout / Paint / Composite 的机制，合理使用样式。【最好一次性重写style属性，或者将样式列表定义为class并一次性更改class属性。】
* 合理利用特殊样式属性（如 transform: translateZ(0) 或者 will-change），将渲染层提升为合成层，开启 GPU 加速，提高页面性能。
* 使用变量对布局信息（如 clientTop）进行缓存，避免因频繁读取布局信息而触发重排。
* 对具有复杂动画的元素使用绝对定位，使它脱离文档流，否则会引起父元素及后续元素频繁回流。

### webpack 工作流程

webpack 是一种模块打包工具，可以将各类型的资源，例如图片、CSS、JS 等，转译组合为 JS 格式的 bundle 文件。

webpack 构建的核心任务是完成内容转化和资源合并。主要包含以下 3 个阶段：

- 初始化阶段
初始化参数：从配置文件、配置对象和 Shell 参数中读取并与默认参数进行合并，组合成最终使用的参数。
创建编译对象：用上一步得到的参数创建 Compiler 对象。
初始化编译环境：包括注入内置插件、注册各种模块工厂、初始化 RuleSet 集合、加载配置的插件等。

- 构建阶段
开始编译：执行 Compiler 对象的 run 方法，创建 Compilation 对象。
确认编译入口：进入 entryOption 阶段，读取配置的 Entries，递归遍历所有的入口文件，调用 Compilation.addEntry 将入口文件转换为 Dependency 对象。
编译模块（make）： 调用 normalModule 中的 build 开启构建，从 entry 文件开始，调用 loader 对模块进行转译处理，然后调用 JS 解释器（acorn）将内容转化为 AST 对象，然后递归分析依赖，依次处理全部文件。
完成模块编译：在上一步处理好所有模块后，得到模块编译产物和依赖关系图。

- 生成阶段
输出资源（seal）：根据入口和模块之间的依赖关系，组装成多个包含多个模块的 Chunk，再把每个 Chunk 转换成一个 Asset 加入到输出列表，这步是可以修改输出内容的最后机会。
写入文件系统（emitAssets）：确定好输出内容后，根据配置的 output 将内容写入文件系统。

AI 版本
1. 初始化 (Initialization)
在这个阶段，Webpack 初始化其配置并设置一些内部数据结构。主要包括：

读取配置文件（通常是 webpack.config.js）。
解析配置中的入口点（entry）、输出配置（output）、加载器（loaders）、插件（plugins）等。

2. 编译 (Compilation)
编译阶段是 Webpack 的核心部分，主要包括以下步骤：

入口点 (Entry):
Webpack 从配置文件中指定的入口点开始（通常是一个或多个 JavaScript 文件）。
入口点是 Webpack 构建依赖图的起点。

构建依赖图 (Build Dependency Graph):
Webpack 解析入口点，并递归地分析这些文件所依赖的其他模块。
Webpack 使用loader（module.rules 配置的加载器）来处理不同类型的文件（例如 JavaScript、CSS、图片等）。

模块处理 (Module Processing):
使用加载器（Loaders）将不同类型的文件转换为 Webpack 能理解的格式（例如，将 SCSS 转换为 CSS，将 ES6 代码转译为 ES5）。
对每个模块应用配置的加载器，并生成中间结果。

3. 打包 (Bundling)
在打包阶段，Webpack 会将所有处理后的模块打包成一个或多个 bundle 文件：

生成模块代码 (Generate Module Code):
Webpack 将所有模块转换成一个特定格式的 JavaScript 代码，这些代码将用于加载和执行模块。
生成的代码通常包括了一个依赖图的实现代码，允许 Webpack 动态地加载模块。

输出文件 (Output Files):
根据配置的输出目录和文件名，将生成的 bundle 文件写入到磁盘。

4. 优化 (Optimization)
Webpack 在打包过程中还会进行各种优化，以提高性能：

代码分割 (Code Splitting):
将代码拆分成多个 bundle，减少初次加载的资源量，提高加载速度。
支持动态导入和按需加载（import()）。

压缩和混淆 (Minification and Obfuscation):
压缩 JavaScript 和 CSS 文件，去掉多余的空白和注释。
使用工具（如 Terser 插件）来混淆代码，减小文件大小。

缓存优化 (Caching Optimization):
为生成的文件添加 hash 值，以便浏览器可以缓存这些文件，从而避免重复下载。

5. 输出 (Output)
最后，Webpack 将处理后的文件输出到指定的目录：

写入文件系统 (Write to File System):
将最终生成的 bundle 文件、source map 等写入到磁盘上。
根据配置的输出目录和文件名进行存储。


### Vue 数据绑定机制

1. Vue 如何实现数据劫持
Vue 实现数据劫持主要通过 Object.defineProperty 方法。Vue 会遍历数据对象的所有属性，为每个属性定义 getter 和 setter。当属性被访问或修改时，Vue 可以检测到这些变化并触发视图更新。具体步骤包括：

数据劫持：在 Vue 的实例化过程中，observe 方法会被调用，它会递归地遍历对象的每个属性，并用 Object.defineProperty 将这些属性的 getter 和 setter 方法重写，以便能够监控属性的变化。
依赖收集：每当属性被访问时，getter 方法会被调用，Vue 会将当前的订阅者（组件或视图）记录为该属性的依赖。
通知更新：当属性的 setter 方法被调用时，Vue 会通知所有相关的订阅者（视图或组件），触发视图的重新渲染。

2. Vue 如何实现双向绑定
Vue 的双向绑定主要依赖于数据劫持和发布-订阅模式的结合：

数据劫持：如上所述，通过 Object.defineProperty 对数据进行劫持，能够检测数据变化。
指令系统：Vue 通过 v-model 实现双向绑定。v-model 实际是 v-bind:xxx 和 v-on:xxx 的语法糖。当触发元素对应的事件（如 input、change 等）时更新数据（ViewModel），当数据（ViewModel）更新时同步更新到元素的对应属性（View）上。

3. MVVM 是什么
MVVM（Model-View-ViewModel）是一种设计模式，用于分离用户界面（View）和业务逻辑（Model）：

Model（模型）：表示应用的数据和业务逻辑。它负责数据的存取和处理。
View（视图）：用户界面的表现层。它展示数据并响应用户交互。
ViewModel（视图模型）：充当 Model 和 View 之间的中介。它处理用户输入，将其转化为 Model 的操作，并将 Model 的数据更新到 View。

4. Vue2 与 Vue3 的差异
Vue2 与 Vue3 数据绑定机制的主要差异是劫持方式。Vue2 使用的是 Object.defineProperty 而 Vue3 使用的是 Proxy。Proxy 可以创建一个对象的代理，从而实现对这个对象基本操作的拦截和自定义。

-------无法监听到的变化
由于受到 JavaScript 设计的限制，Vue2 使用的 Object.defineProperty 并不能完全劫持所有数据的变化，以下是几种无法正常劫持的变化：

1 无法劫持新创建的属性，为了解决这个问题，Vue 提供了 Vue.set 以创建新属性。
```js
// 创建 Vue 实例
const vm = new Vue({
  data: {
    person: {
      name: 'Alice'
    }
  }
});

// 新增属性 'age'
vm.person.age = 30; // Vue 不会检测到这个变化

// 解决方案：使用 Vue.set
Vue.set(vm.person, 'age', 30); // Vue 能够正确地劫持这个新属性

```
2 无法劫持数组的变化，为了解决这个问题，Vue 对数组原生方法进行了劫持。
```js
// 创建 Vue 实例
const vm = new Vue({
  data: {
    numbers: [1, 2, 3]
  }
});

// 使用数组的原生方法（如 push）修改数组
vm.numbers.push(4); // Vue 不会检测到这个变化

// 解决方案：Vue 对数组原生方法进行了劫持
// 所以上述 push 操作实际上会触发视图更新

```
3 无法劫持利用索引修改数组元素，这个问题同样可以用 Vue.set 解决。
```js
// 创建 Vue 实例
const vm = new Vue({
  data: {
    numbers: [1, 2, 3]
  }
});

// 直接使用索引修改数组元素
vm.numbers[1] = 4; // Vue 不会检测到这个变化

// 解决方案：使用 Vue.set 修改数组元素
Vue.set(vm.numbers, 1, 4); // Vue 能够正确地劫持这个修改

```

computed 等同于为属性设置 getter 函数（也可设置 setter），而 watch 等同于为属性的 setter 设置回调函数、监听深度 deep 及响应速度 immediate。

computed：需要处理复杂逻辑的模板表达式。
```js
const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})

// 一个计算属性 ref
const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})
```
watch：需要执行异步或开销较大的操作。
```js
const x = ref(0)
const y = ref(0)

// 单个 ref
watch(x, (newX) => {
  console.log(`x is ${newX}`)
})

// getter 函数
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)

// 多个来源组成的数组
watch([x, () => y.value], ([newX, newY]) => {
  console.log(`x is ${newX} and y is ${newY}`)
})
```
watch 只追踪明确侦听的数据源。它不会追踪任何在回调中访问到的东西。

watchEffect，则会在副作用发生期间追踪依赖。它会在同步执行过程中，自动追踪所有能访问到的响应式属性。

```js
const todoId = ref(1)
const data = ref(null)

watch(
  todoId,
  async () => {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
    )
    data.value = await response.json()
  },
  { immediate: true }
)

watchEffect(async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
})
```


### 闭包的作用和原理
作用：能够在函数定义的作用域外，使用函数定义作用域内的局部变量，并且不会污染全局。

在 JavaScript 中，每当创建一个函数，闭包就会在函数创建的同时被创建出来。

```js
function foo() {
  var a = "hzfe";
  function bar() {
    console.log(a);
  }
  return bar;
}

var baz = foo();
baz(); // hzfe
```