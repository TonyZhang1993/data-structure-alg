当遇到大量数据时, 如何才能在不卡主页面的情况下渲染数据

## 高性能渲染十万条数据(时间分片)

```js
//  一次性将大量数据插入到页面中

// 记录任务开始时间
let now = Date.now();
// 插入十万条数据
const total = 100000;
// 获取容器
let ul = document.getElementById('container');
// 将数据插入容器中
for (let i = 0; i < total; i++) {
    let li = document.createElement('li');
    //  ~~理解成 Math.floor()
    li.innerText = ~~(Math.random() * total)
    ul.appendChild(li);
}

console.log('JS运行时间: ',Date.now() - now);
setTimeout(()=>{
  console.log('总运行时间: ',Date.now() - now);
},0)
// print: JS运行时间:  187
// print: 总运行时间:  2844


如何简单来统计JS运行时间和总渲染时间: 

- 在 JS 的Event Loop中, 当JS引擎所管理的执行栈中的事件以及所有微任务事件全部执行完后, 才会触发 *渲染线程* 对页面进行渲染
- 第一个console.log的触发时间是在页面进行渲染之前, 此时得到的间隔时间为JS运行所需要的时间
- 第二个console.log是放到 setTimeout 中的, 它的触发时间是在渲染完成, 在下一次Event Loop中执行的

//  对于大量数据渲染的时候, JS运算并不是性能的瓶颈, 性能的瓶颈主要在于渲染阶段

//  使用定时器 setTimeout

//需要插入的容器
let ul = document.getElementById('container');
// 插入十万条数据
let total = 100000;
// 一次插入 20 条
let once = 20;
//总页数
let page = total/once
//每条记录的索引
let index = 0;
//循环加载数据
function loop(curTotal,curIndex){
    if(curTotal <= 0){
        return false;
    }
    //每页多少条
    let pageCount = Math.min(curTotal , once);
    setTimeout(()=>{
        for(let i = 0; i < pageCount; i++){
            let li = document.createElement('li');
            li.innerText = curIndex + i + ' : ' + ~~(Math.random() * total)
            ul.appendChild(li)
        }
        loop(curTotal - pageCount,curIndex + pageCount)
    },0)
}
loop(total,index);

FPS表示的是每秒钟画面更新次数, 也就是每秒钟画面更新的帧数. 
大多数电脑显示器的刷新频率是60Hz, 大概相当于每秒钟重绘60次, FPS为60frame/s
最平滑动画的最佳循环间隔是1000ms/60, 约等于16.6ms. 

setTimeout的执行时间并不是确定的. 在JS中, setTimeout任务被放进事件队列中, 只有主线程执行完才会去检查事件队列中的任务是否需要执行, 因此setTimeout的实际执行时间可能会比其设定的时间晚一些. 

在setTimeout中对dom进行操作, 必须要等到屏幕下次绘制时才能更新到屏幕上, 如果两者步调不一致, 就可能导致中间某一帧的操作被跨越过去, 而直接更新下一帧的元素, 从而导致丢帧现象. 


//  使用 requestAnimationFrame
requestAnimationFrame最大的优势是由系统来决定回调函数的执行时机. 
如果屏幕刷新率是60Hz,那么回调函数就每16.7ms被执行一次

window.requestAnimationFrame(function(){
    for(let i = 0; i < pageCount; i++){
        let li = document.createElement('li');
        li.innerText = curIndex + i + ' : ' + ~~(Math.random() * total)
        ul.appendChild(li)
    }
    loop(curTotal - pageCount,curIndex + pageCount)
})

//  使用 DocumentFragment
DocumentFragment, 文档片段接口, 表示一个没有父级文件的最小文档对象. 它被作为一个轻量版的Document使用, 用于存储已排好版的或尚未打理好格式的XML片段. 最大的区别是因为DocumentFragment不是真实DOM树的一部分, 它的变化不会触发DOM树的(重新渲染) , 且不会导致性能等问题. 

DocumentFragments是DOM节点, 但并不是DOM树的一部分, 可以认为是存在内存中的, 所以将子元素插入到文档片段时不会引起页面回流. 

当append元素到document中时, 被append进去的元素的样式表的计算是同步发生的, 此时调用 getComputedStyle 可以得到样式的计算值. 
而append元素到documentFragment 中时, 是不会计算元素的样式表, 所以documentFragment 性能更优

当然现在浏览器的优化已经做的很好了,  当append元素到document中后, 没有访问 getComputedStyle 之类的方法时, 现代浏览器也可以把样式表的计算推迟到脚本执行之后. 



window.requestAnimationFrame(function(){
    let fragment = document.createDocumentFragment();
    for(let i = 0; i < pageCount; i++){
        let li = document.createElement('li');
        li.innerText = curIndex + i + ' : ' + ~~(Math.random() * total)
        fragment.appendChild(li)
    }
    ul.appendChild(fragment)
    loop(curTotal - pageCount,curIndex + pageCount)
})

```



## 高性能渲染十万条数据(虚拟列表)

虚拟列表解决的长列表渲染大量节点导致的性能问题: 
- 一次性渲染大量节点, 会占用大量 GPU 资源, 导致卡顿；
- 即使渲染好了, 大量的节点也持续占用内存. 列表项下的节点越多, 就越耗费性能. 

虚拟列表的实现分两种, 一种是列表项高度固定的情况, 另一种是列表项高度动态的情况. 

虚拟列表其实是按需显示的一种实现, 即只对可见区域进行渲染, 对非可见区域中的数据不渲染或部分渲染的技术, 从而达到极高的渲染性能. 

虚拟列表的实现, 实际上就是在首屏加载的时候, 只加载可视区域内需要的列表项, 当滚动发生时, 动态通过计算获得可视区域内的列表项, 并将非可视区域内存在的列表项删除. 

- 计算当前可视区域起始数据索引(startIndex)
- 计算当前可视区域结束数据索引(endIndex)
- 计算当前可视区域的数据, 并渲染到页面中
- 计算startIndex对应的数据在整个列表中的偏移位置startOffset并设置到列表上

[参考](https://juejin.cn/post/6844903982742110216)瀑布流

## 瀑布流

优点如下: 
节省空间, 外表美观, 更有艺术性. 
对于触屏设备非常友好, 通过向上滑动浏览
用户浏览时的观赏和思维不容易被打断, 留存更容易. 

缺点如下: 
用户无法了解内容总长度, 对内容没有宏观掌控. 
用户无法了解现在所处的具体位置, 不知道离终点还有多远. 
回溯时不容易定位到之前看到的内容. 
容易造成页面加载的负荷. 

适用场景: 
内容以图片为主的时候 / 信息与信息之间相对独立时 / 信息与搜索匹配比较模糊时 / 用户目的性不强的时候

```js
//  grid 布局实现瀑布流
display:设置为grid指明当前容器为Grid布局
grid-template-columns: 定义每一列的列宽
grid-template-rows: 定义每一行的行高
column-gap: 用于设置列间距

grid-row-start: 上边框所在的水平网格线
grid-row-end: 下边框所在的水平网格线
grid-column-start: 左边框所在的垂直网格线
grid-column-end: 右边框所在的垂直网格线

//  Flexbox 实现瀑布流
.masonry {
    display: flex; // 设置为Flex容器
    flex-direction: row; // 主轴方向设置为水平方向
}

.column {
    display: flex; // 设置为Flex容器
    flex-direction: column; // 主轴方向设置为垂直方向
}

```
