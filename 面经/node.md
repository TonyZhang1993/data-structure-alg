## Node

在浏览器外运行 V8 JavaScript 引擎（Google Chrome 的内核），利用事件驱动、非阻塞和异步输入输出模型等技术提高性能

可以理解为 Node.js 就是一个服务器端的、非阻塞式I/O的、事件驱动的JavaScript运行环境

Nodejs采用了非阻塞型I/O机制，在做I/O操作的时候不会造成任何的阻塞，当完成之后，以时间的形式通知执行操作

事件驱动就是当进来一个新的请求的时，请求将会被压入一个事件队列中，然后通过一个循环来检测队列中的事件状态变化，如果检测到有状态变化的事件，那么就执行该事件对应的处理代码，一般都是回调函数

因为Nodejs是单线程，带来的缺点有：
不适合CPU密集型应用
只支持单核CPU，不能充分利用CPU
可靠性低，一旦代码某个环节崩溃，整个系统都崩溃

优点：
处理高并发场景性能更佳
适合I/O密集型应用

其应用场景分类如下：
善于I/O，不善于计算。因为Nodejs是一个单线程，如果计算（同步）太多，则会阻塞这个线程
大量并发的I/O，应用程序内部并不需要进行非常复杂的处理
与 websocket 配合，开发长连接的实时交互应用程序


## 全局对象
Nodejs中的全局对象是 global
在NodeJS中，用var声明的变量并不属于全局的变量，只在当前模块生效


真正的全局对象
下面给出一些常见的全局对象：

Class:Buffer [可以处理二进制以及非Unicode编码的数据]

process [进程对象，提供有关当前进程的信息和控制]

console

clearInterval、setInterval

clearTimeout、setTimeout

global [process、console、setTimeout等都有放到global中]

模块级别的全局对象
__dirname [获取当前文件所在的路径，不包括后面的文件名]
__filename [获取当前文件所在的路径和文件名称，包括后面的文件名称]
exports
```js
module.exports 用于指定一个模块所导出的内容，即可以通过 require() 访问的内容

exports.name = name;
exports.age = age;
exports.sayHello = sayHello;
```
module [module.exports 用于指定一个模块所导出的内容]
require [用于引入模块、 JSON、或本地文件。 可以从 node_modules 引入模块]

## process
process 对象是一个全局变量，提供了有关当前 Node.js `进程`信息并对其进行控制，作为一个全局变量

我们都知道，`进程` 是 计算机系统进行资源分配和调度的基本单位，是操作系统结构的基础，是`线程`的容器

当我们启动一个js文件，实际就是开启了一个服务进程，每个进程都拥有自己的独立空间地址、数据栈，像另一个进程无法访问当前进程的变量、数据结构，只有数据通信后，进程之间才可以数据共享

由于JavaScript是一个单线程语言，所以通过node xxx启动一个文件后，只有一条主线程

关于process常见的属性有如下：

process.env：环境变量，例如通过 `process.env.NODE_ENV 获取不同环境项目配置信息
process.nextTick：这个在谈及 EventLoop 时经常为会提到
process.pid：获取当前进程id
process.ppid：当前进程对应的父进程
process.cwd()：获取当前进程工作目录，
process.platform：获取当前进程运行的操作系统平台
process.uptime()：当前进程已运行时间，例如：pm2 守护进程的 uptime 值
进程事件： process.on(‘uncaughtException’,cb) 捕获异常信息、 process.on(‘exit’,cb）进程推出监听
三个标准流： process.stdout 标准输出、 process.stdin 标准输入、 process.stderr 标准错误输出
process.title 指定进程名称，有的时候需要给进程指定一个名称

我们知道NodeJs是基于事件轮询，在这个过程中，同一时间只会处理一件事情

在这种处理模式下，process.nextTick()就是定义出一个动作，并且让这个动作在下一个事件轮询的时间点上执行

两者区别在于：

process.nextTick()会在这一次event loop的call stack清空后（下一次event loop开始前）再调用callback
setTimeout()是并不知道什么时候call stack清空的，所以何时调用callback函数是不确定的

## fs 模块

fs（filesystem），该模块提供本地文件的读写能力，基本上是POSIX文件操作命令的简单包装

const fs = require('fs');
这个模块对所有文件系统操作提供异步（不具有sync 后缀）和同步（具有 sync 后缀）两种操作方式，而供开发者选择

如在linux查看文件权限位：

drwxr-xr-x 1 PandaShen 197121 0 Jun 28 14:41 core
-rw-r--r-- 1 PandaShen 197121 293 Jun 23 17:44 index.md
在开头前十位中，d为文件夹，-为文件，后九位就代表当前用户、用户所属组和其他用户的权限位，按每三位划分，分别代表读（r）、写（w）和执行（x），- 代表没有当前位对应的权限


文件读取
fs.readFileSync同步读取
fs.readFile异步读取方法

writeFileSync同步写入
writeFile异步写入

文件追加写入
appendFileSync
appendFile 异步追加写入

copyFileSync 同步拷贝
copyFile 异步拷贝


mkdirSync 同步创建目录
mkdir 异步创建

## Buffer & Stream
TBD

## EventEmitter
Node采用了事件驱动机制，而EventEmitter就是Node实现事件驱动的基础
在EventEmitter的基础上，Node几乎所有的模块都继承了这个类，这些模块拥有了自己的事件，可以绑定／触发监听器，实现了异步操作

Node的events模块只提供了一个EventEmitter类，这个类实现了Node异步事件驱动架构的基本模式——观察者模式

emitter.addListener/on(eventName, listener) ：添加类型为 eventName 的监听事件到事件数组尾部
emitter.prependListener(eventName, listener)：添加类型为 eventName 的监听事件到事件数组头部
emitter.emit(eventName[, ...args])：触发类型为 eventName 的监听事件
emitter.removeListener/off(eventName, listener)：移除类型为 eventName 的监听事件
emitter.once(eventName, listener)：添加类型为 eventName 的监听事件，以后只能执行一次并删除
emitter.removeAllListeners([eventName])： 移除全部类型为 eventName 的监听事件


## 事件循环
在Node中，同样存在宏任务和微任务，与浏览器中的事件循环相似

微任务对应有：

next tick queue：process.nextTick
other queue：Promise的then回调、queueMicrotask
宏任务对应有：

timer queue：setTimeout、setInterval
poll queue：IO事件
check queue：setImmediate
close queue：close事件
其执行顺序为：

next tick microtask queue
other microtask queue
timer queue
poll queue
check queue
close queu

## node 中间件
在express、koa等web框架中，中间件的本质为一个回调函数，参数包含请求对象、响应对象和执行下一个中间件的函数

koa是基于NodeJS当前比较流行的web框架

Koa 中间件采用的是洋葱圈模型，每次执行下一个中间件传入两个参数：

ctx ：封装了request 和 response 的变量
next ：进入下一个要执行的中间件的函数


## 分页功能
在我们做数据查询的时候，如果数据量很大，比如几万条数据，放在一个页面显示的话显然不友好，这时候就需要采用分页显示的形式，如每次只显示10条数据

要实现分页功能，实际上就是从结果集中显示第1~10条记录作为第1页，显示第11~20条记录作为第2页，

前端发送
```js
{
  order: "desc" ,
  orderBy: "",
  pageNum: 2,
  pageSize: 10,
}
```

后端返回必要数据
```js
{
 "totalCount": 1836,   // 总的条数
 "totalPages": 92,  // 总页数
 "pageNum": 2   // 当前页数
  "pageSize": 10,
 "data": [     // 当前页的数据
   {
 ...
   }
  ]
}
```