```js
Target - 1 html


<!DOCTYPE>  声明一般位于文档的第一行，它的作用主要是告诉浏览器以什么样的模式来解析文档。一般指定了之后会以标准模式来
进行文档解析，否则就以兼容模式进行解析。在标准模式下，浏览器的解析规则都是按照最新的标准进行解析的。而在兼容模式下，浏
览器会以向后兼容的方式来模拟老式浏览器的行为，以保证一些老的网站的正确访问。

标准模式的渲染方式和 JS 引擎的解析方式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示
，模拟老式浏览器的行为以防止站点无法工作。

HTML（HyperText Markup Language）是超文本标记语言，主要是用于规定怎么显示网页。

XML（Extensible Markup Language）是可扩展标记语言是未来网页语言的发展方向，XML 和 HTML 的最大区别就在于 XML 的标签是可以自己创建的，数量无限多，
而 HTML 的标签都是固定的而且数量有限。

XHTML（Extensible Hypertext Markup Language）和 HTML 没什么本质的区别，标签都一样，用法也都一样，就是比 HTML ;更严格，比如标签必须都用小写，标签都必须有闭合标签等。

HTML4 中，元素被分成两大类: inline （内联元素）与 block（块级元素）。一个行内元素只占据它对应标签的边框所包含的空
间。
常见的行内元素有 a b span img strong sub sup button input label select textarea
行内元素只能包含文本和其他行内元素。而块级元素可以包含行内元素和其他块级元素。

块级元素占据其父元素（容器）的整个宽度，因此创建了一个“块”。
常见的块级元素有  div ul ol li dl dt dd h1 h2 h3 h4 h5 h6 p 

区别 - 
（1） 格式上，默认情况下，行内元素不会以新行开始，而块级元素会新起一行。
（2） 内容上，默认情况下，行内元素只能包含文本和其他行内元素。而块级元素可以包含行内元素和其他块级元素。
（3） 行内元素与块级元素属性的不同，主要是盒模型属性上：行内元素设置 width 无效，height 无效（可以设置 line-hei
     ght），设置 margin 和 padding 的上下不会对其他元素产生影响。

HTML4中，元素被分成两大类: inline（内联元素）与 block（块级元素）
HTML5中，元素主要分为7类：Metadata Flow Sectioning Heading Phrasing Embedded Interactive
 inline-block 这一对外呈现 inline 对内呈现 block 的属性。因此，简单地把 HTML 元素划分为
inline 与 block 已经不再符合实际需求。

display:inline-block 
a、简单来说就是将对象呈现为inline对象，但是对象的内容作为block对象呈现。
比如我们可以给一个link（a元素）inline-block属性值，使其既具有block的宽度高度及padding margin 垂直方向特性又具有inline的同行特性。

标签内没有内容的 HTML 标签被称为空元素。空元素是在开始标签中关闭的。
常见的空元素有：br hr img input link meta

link 标签定义文档与外部资源的关系。
link 元素是空元素，它仅包含属性。 此元素只能存在于 head 部分，不过它可出现任何次数。
link 标签中的 rel 属性定义了当前文档与被链接文档之间的关系。常见的 stylesheet 指的是定义一个外部加载的样式表。

使用 link 和 @import 有什么区别？
（1）从属关系区别。 @import 是 CSS 提供的语法规则，只有导入样式表的作用；link 是 HTML 提供的标签，不仅可以加
     载 CSS 文件，还可以定义 RSS、rel 连接属性、引入网站图标等。
（2）加载顺序区别。加载页面时，link 标签引入的 CSS 被同时加载；@import 引入的 CSS 将在页面加载完毕后被加载。
（3）兼容性区别。@import 是 CSS2.1 才有的语法，故只可在 IE5+ 才能识别；link 标签作为 HTML 元素，不存在兼容
     性问题。
（4）DOM 可控性区别。可以通过 JS 操作 DOM ，插入 link 标签来改变样式；由于 DOM 方法是基于文档的，无法使用 @i
    mport 的方式插入样式。


对浏览器内核的理解？
主要分成两部分：渲染引擎和 JS 引擎。

渲染引擎: 是渲染，即在浏览器窗口中显示所请求的内容。默认情况下，渲染引擎可以显示 html、xml 文档及图片

JS 引擎：解析和执行 js 来实现网页的动态效果。

最开始渲染引擎和 JS 引擎并没有区分的很明确，后来 JS 引擎越来越独立，内核就倾向于只指渲染引擎。

浏览器内核比较
 （1） IE 浏览器内核：Trident 内核，也是俗称的 IE 内核；
 （2） Chrome 浏览器内核：统称为 Chromium 内核或 Chrome 内核，以前是 Webkit 内核，现在是 Blink内核；
 （3） Firefox 浏览器内核：Gecko 内核，俗称 Firefox 内核；
 （4） Safari 浏览器内核：Webkit 内核；
 （5） Opera 浏览器内核：最初是自己的 Presto 内核，后来加入谷歌大军，从 Webkit 又到了 Blink 内核；


渲染主流程：渲染引擎获得所请求的文档后 => 解析html以构建DOM树 => 加载并解析CSS，构建CSSOM树 => 构建render树 => 布局render树 => 绘制render树

详细过程：渲染引擎开始解析html，并将标签转化为内容树中的DOM节点，接着解析CSS文件或者style中的样式，构建CSSOM树，CSSOM树以及DOM中的可见性指令将被用来构建另一棵树，即render树，Render树由一些包含有颜色和大小等属性的矩形组成，它们将被按照正确的顺序显示到屏幕上。Render树构建好了之后，将会执行布局过程，它将确定每个节点在屏幕上的确切坐标。再下一步就是绘制，即遍历render树，并使用UI后端层绘制每个节点。

页面在首次加载时必然会经历reflow和repain

当文档加载过程中遇到js文件，html文档会挂起渲染（加载解析渲染同步）的线程，不仅要等待文档中js文件加载完毕，还要等待解析执行完毕，才可以恢复html文档的渲染线程。

「CSS 加载不会阻塞 DOM 的解析」
「CSS加载会阻塞DOM渲染」 - Render Tree 是依赖DOM Tree和 CSSOM Tree
JS文件下载和CSS文件下载是并行的
css 会阻塞后面 js 的执行
 JavaScript 的加载、解析与执行会阻塞文档的解析 【JS 下载和执行 会阻塞； 给 script 标签添加 defer 或者 async 属性】
!!!!! 并行化加载，但 dom 树解析、js执行和首屏渲染却是串行的

！！！！！！！！！！！！JS  执行机制 / 事件循环 - 
javaScript 引擎说起来最流行的当然是谷歌的 V8 引擎了， V8 引擎使用在 Chrome 以及 Node 
这个引擎主要由两部分组成:
* 内存堆：这是内存分配发生的地方
* 调用栈：这是你的代码执行时的地方

JavaScript 是一门单线程的语言，这意味着它只有一个调用栈，因此，它同一时间只能做一件事。

* 执行一个宏任务（栈中没有就从事件队列中获取）
* 执行过程中如果遇到微任务，就将它添加到微任务的任务队列中
* 宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）
* 当前宏任务执行完毕，开始检查渲染，然后GUI线程接管渲染
* 渲染完毕后，JS线程继续接管，开始下一个宏任务（从事件队列中获取）

* macro-task(宏任务)：包括整体代码script，setTimeout，setInterval，setImediate、 I/O 、UI rendering。
* micro-task(微任务)：MutationObserver、Promise.then()或catch()、Promise为基础开发的其它技术，比如fetch API、V8的垃圾回收过程、Node独有的process.nextTick。

event loop首先检查macrotask队列，如果有一个macrotask等待执行，那么执行该任务。当该任务执行完毕后（或者macrotask队列为空），event loop继续执行microtask队列。如果microtask队列有等待执行的任务，那么event loop就一直取出任务执行直到microtask为空。这里我们注意到处理microtask和macrotask的不同之处：在单次循环中，一次最多处理一个macrotask（其他的仍然驻留在队列中），然而却可以处理完所有的microtask。

event loop它的执行顺序：
* 一开始整个脚本作为一个宏任务执行
* 执行过程中同步代码直接执行，宏任务进入宏任务队列，微任务进入微任务队列
* 当前宏任务执行完出队，检查微任务列表，有则依次执行，直到全部执行完
* 执行浏览器UI线程的渲染工作
* 检查是否有Web Worker任务，有则执行
* 执行完本轮的宏任务，回到2，依此循环，直到宏任务和微任务队列都为空

白屏：把 JS 文件放在头部，脚本的加载会阻塞后面
      文档内容的解析，从而页面迟迟未渲染出来，出现白屏问题。
样式移动： 骨架屏

重绘: 当渲染树中的一些元素需要更新属性，而这些属性只是影响元素的外观、风格，而不会影响布局的操作，比如 background
       -color，我们将这样的操作称为重绘。
 回流：当渲染树中的一部分（或全部）因为元素的规模尺寸、布局、隐藏等改变而需要重新构建的操作，会影响到布局的操作，这样
      的操作我们称为回流。

       任何会改变元素几何信息（元素的位置和尺寸大小）的操作，都会触发回流。

减少回流：
1. CSS
    * 使用 transform 替代 top
    * 使用 visibility 替换 display: none ，因为前者只会引起重绘，后者会引发回流（改变了布局
    * 避免使用table布局，可能很小的一个小改动会造成整个 table 的重新布局。
    * 尽可能在DOM树的最末端改变class，回流是不可避免的，但可以减少其影响。尽可能在DOM树的最末端改变class，可以限制了回流的范围，使其影响尽可能少的节点。
    * 避免设置多层内联样式，CSS 选择符从右往左匹配查找，避免节点层级过多。应该尽可能的避免写过于具体的 CSS 选择器，然后对于 HTML 来说也尽量少的添加无意义标签，保证层级扁平。
    * 将动画效果应用到position属性为absolute或fixed的元素上，避免影响其他元素的布局，这样只是一个重绘，而不是回流，同时，控制动画速度可以选择 requestAnimationFrame，详见探讨 requestAnimationFrame。
    * 避免使用CSS表达式，可能会引发回流。
    * CSS3 硬件加速（GPU加速），使用css3硬件加速，可以让transform、opacity、filters这些动画不会引起回流重绘 
2. JavaScript
    * 避免频繁操作样式，最好一次性重写style属性，或者将样式列表定义为class并一次性更改class属性。
    * 避免频繁操作DOM，创建一个documentFragment，在它上面应用所有DOM操作，最后再把它添加到文档中。
    * 避免频繁读取会引发回流/重绘的属性，如果确实需要多次使用，就用一个变量缓存起来。
    * 对具有复杂动画的元素使用绝对定位，使它脱离文档流，否则会引起父元素及后续元素频繁回流。

  为什么操作 DOM 慢： 一些 DOM 的操作或者属性访问可能会引起页面的回流和重绘，从而引起性能上的消耗。


 当初始的 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发，而无需等待样式表、图像和
 子框架的加载完成。仅当DOM解析完成后
 Load 事件是当所有资源加载完成后触发的。

 HTML5 有哪些新特性？

 新增的有：
 绘画 canvas;
 用于媒介回放的 video 和 audio 元素;
 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失;
 sessionStorage 的数据在浏览器关闭后自动删除;
 语意化更好的内容元素，比如 article、footer、header、nav、section;
 表单控件，calendar、date、time、email、url、search;
 新的技术 webworker, websocket;
 新的文档属性 document.visibilityState


对 HTML 语义化的理解：  我认为 html 语义化主要指的是我们应该使用合适的标签来划分网页内容的结构。html 的本质作用其实就是定义网页文档的结构，
 一个语义化的文档，能够使页面的结构更加清晰，易于理解。这样不仅有利于开发者的维护和理解，同时也能够使机器对文档内容进
 行正确的解读。如果是搜索引擎的爬虫对我们网页进行分析的话，那么它会
 依赖于 html 标签来确定上下文和各个关键字的权重，一个语义化的文档对爬虫来说是友好的，是有利于爬虫对文档内容解读的，
 从而有利于我们网站的 SEO 。从 html5 我们可以看出，标准是倾向于以语义化的方式来构建网页的，比如新增了 header 、fo
 oter 这些语义标签，删除了 big 、font 这些没有语义的标签。


 从页面显示效果来看，被 <b> 和 <strong> 包围的文字将会被加粗，而被 <i> 和 <em> 包围的文字将以斜体的形式呈现。
 而 <em> 和 <strong> 是语义样式标签。 <em> 表示一般的强调文本，而 <strong> 表示比 <em> 语义更强的强调文本。

前端需要注意哪些 SEO ？
 （1）合理的 title、description、keywords：搜索对着三项的权重逐个减小，title 值强调重点即可，重要关键词出现不要超
     过2次，而且要靠前，不同页面 title 要有所不同；description 把页面内容高度概括，长度合适，不可过分堆砌关键词，不
     同页面 description 有所不同；keywords 列举出重要关键词即可。
     <title>Vue.js - 渐进式 JavaScript 框架 | Vue.js</title>
     <meta name="description" content="Vue.js - 渐进式的 JavaScript 框架">

 （2）语义化的 HTML 代码，符合 W3C 规范：语义化代码让搜索引擎容易理解网页。
 （3）重要内容 HTML 代码放在最前：搜索引擎抓取 HTML 顺序是从上到下，有的搜索引擎对抓取长度有限制，保证重要内容肯定被
     抓取。
 （4）重要内容不要用 js 输出：爬虫不会执行 js 获取内容
 （5）少用 iframe：搜索引擎不会抓取 iframe 中的内容
 （6）非装饰性图片必须加 alt
 （7）提高网站速度：网站速度是搜索引擎排序的一个重要指标


  HTML5 的离线储存怎么使用，工作原理能不能解释一下？
 在用户没有与因特网连接时，可以正常访问站点或应用，在用户与因特网连接时，更新用户机器上的缓存文件。

  原理：HTML5 的离线存储是基于一个新建的 .appcache 文件的缓存机制（不是存储技术），通过这个文件上的解析清单离线存储资源，这些资源就会像 cookie 一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。
[和 manifest 相关]在线的情况下，浏览器发现 html 头部有 manifest 属性，它会请求 manifest 文件，如果是第一次访问 app ，那么浏览器就会根据 manifest 文件的内容下载相应的资源并且进行离线存储。
 离线的情况下，浏览器就直接使用离线存储的资源。

  cookie 其实最开始是服务器端用于记录用户状态的一种方式，由服务器设置，在客户端存储，然后每次发起同源请求时，发送给服
 务器端。cookie 最多能存储 4 k 数据，它的生存时间由 expires 属性指定，并且 cookie 只能被同源的页面访问共享。

 sessionStorage 是 html5 提供的一种浏览器本地存储的方法，它借鉴了服务器端 session 的概念，代表的是一次会话中所保
 存的数据。它一般能够存储 5M 或者更大的数据，它在当前窗口关闭后就失效了，并且 sessionStorage 只能被同一个窗口的同源
 页面所访问共享。

 localStorage 也是 html5 提供的一种浏览器本地存储的方法，它一般也能够存储 5M 或者更大的数据。它和 sessionStorage 
 不同的是，除非手动删除它，否则它不会失效，并且 localStorage 也只能被同源页面所访问共享。


 iframe 缺点
 （1） iframe 会阻塞主页面的 onload 事件。window 的 onload 事件需要在所有 iframe 加载完毕后（包含里面的元素）才会触发。在 Safari 和 Chrome 里，通过 JavaScript 动态设置 iframe 的 src 可以避免这种阻塞情况。
 （2） 搜索引擎的检索程序无法解读这种页面，不利于网页的 SEO 。
 （3） iframe 和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。
 （4） 浏览器的后退按钮失效。
 （5） 小型的移动设备无法完全显示框架。


 label 标签来定义表单控制间的关系，当用户选择该标签时，浏览器会自动将焦点转到和标签相关的表单控件上。
 <label for="Name">Number:</label>
 <input type=“text“ name="Name" id="Name"/>

  autocomplete 属性规定输入字段是否应该启用自动完成功能。默认为启用，设置为 autocomplete=off 可以关闭该功能。
 自动完成允许浏览器预测对字段的输入。当用户在字段开始键入时，浏览器基于之前键入过的值，应该显示出在字段中填写的选项。
form / input

如何实现浏览器内多个标签页之间的通信?
相关资料：

 （1）使用 WebSocket，通信的标签页连接同一个服务器，发送消息到服务器后，服务器推送消息给所有连接的客户端。
 （2）使用 SharedWorker （只在 chrome 浏览器实现了），两个页面共享同一个线程，通过向线程发送数据和接收数据来实现标
     签页之间的双向通行。
 （3）可以调用 localStorage、cookies 等本地存储方式，localStorge 另一个浏览上下文里被添加、修改或删除时，它都会触
     发一个 storage 事件，我们通过监听 storage 事件，控制它的值来进行页面信息通信；
 （4）如果我们能够获得对应标签页的引用，通过 postMessage 方法也是可以实现多个标签页通信的。


 如何在页面上实现一个圆形的可点击区域？
 （1）纯 html 实现，使用 <area> 来给 <img> 图像标记热点区域的方式，<map> 标签用来定义一个客户端图像映射，<area> 
     标签用来定义图像映射中的区域，area 元素永远嵌套在 map 元素内部，我们可以将 area 区域设置为圆形，从而实现可点击
     的圆形区域。
		<img src="1.png" width="299" height="299" border="0" usemap="#Map" />
		<map name="Map" id="Map">
			<area shape="circle" coords="150,150,150" href="canvas.html" target="_blank"/>
		</map>

 （2）纯 css 实现，使用 border-radius ，当 border-radius 的长度等于宽高相等的元素值的一半时，即可实现一个圆形的
     点击区域。
     #circle{
				width: 100px;
				height: 100px;
				line-height: 100px;
				margin: 120px auto;
				border-radius: 50%;
				background: red;
				cursor: pointer;
				text-align: center;
				color: white;
			}

 （3）纯 js 实现，判断一个点在不在圆上的简单算法，通过监听文档的点击事件，获取每次点击时鼠标的位置，判断该位置是否在我
     们规定的圆形区域内。

// 假设圆心为（100，100），半径r = 50
				document.onclick = function(event){
					var event = event || window.event;
					var x0 = 100;
					var y0 = 100;
					var r = 50;
					var x1 = event.clientX;
					var y1 = event.clientY;
					var dis = Math.sqrt((x1-x0)*(x1-x0) + (y1-y0)*(y1-y0));
					if(dis <= r){
						alert("Yeap~");
					}else{
						alert("Oh,no!");
					}
				}
			}


实现不使用 border 画出 1 px 高的线，在不同浏览器的标准模式与怪异模式下都能保持一致的效果。
  <div style="height:1px;overflow:hidden;background:red"></div>


  <img> 的 title 和 alt 有什么区别？
 title 通常当鼠标滑动到元素上的时候显示

 alt 是 <img> 的特有属性，是图片内容的等价描述，用于图片无法加载时显示

  Canvas 是一种通过 JavaScript 来绘制 2D 图形的方法。Canvas 是逐像素来进行渲染的，因此当我们对 Canvas 进行缩放时，会出现锯齿或者失真的情况。
 
 SVG 是一种使用 XML 描述 2D 图形的语言。SVG 保存的是图形的绘制方法，因此当 SVG 图形缩放时并不会失真。

 网页验证码 - 区分用户是计算机还是人的公共全自动程序。可以防止恶意破解密码、刷票、论坛灌水


  渐进增强：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的
         用户体验。
 优雅降级：一开始就根据高版本浏览器构建完整的功能，然后再针对低版本浏览器进行兼容。


  可用性（Usability）：产品是否容易上手，效率如何，以及用户的主观感受可好，是从用户的角度来看产品的质量。可用性好意味着产品质量高，是企业的核心竞争力

 可访问性（Accessibility）：Web 内容对于残障用户的可阅读和可理解性
 
 可维护性（Maintainability）：一般包含两个层次，一是当系统出现问题时，快速定位并解决问题的成本，成本低则可维护性好。二是代码是否容易被人理解，是否容易修改和增强功能。

 IE 各版本和 Chrome 可以并行下载多少个资源 - 6

 怎么重构页面？
 （1） 编写 CSS
 （2） 让页面结构更合理化，提升用户体验
 （3） 实现良好的页面效果和提升性能

  * 用户界面
   * 主进程
   * 内核
       * 渲染引擎
       * JS 引擎
           * 执行栈
       * 事件触发线程
           * 消息队列
               * 微任务
               * 宏任务
       * 网络异步线程
       * 定时器线程


 <meta> 元素可提供有关页面的元信息（meta-information），比如针对搜索引擎和更新频度的描述和关键词。
 <meta> 标签位于文档的头部，不包含任何内容。<meta> 标签的属性定义了与文档相关联的名称/值对。

 <!DOCTYPE html>  H5标准声明，使用 HTML5 doctype，不区分大小写
 <head lang="en"> 标准的 lang 属性写法
 <meta charset="utf-8">    声明文档使用的字符编码
 <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>   优先使用 IE 最新版本和 Chrome
 <meta name="description" content="不超过150个字符"/>       页面描述
 <meta name="keywords" content=""/>      页面关键词者
  设置页面不缓存
 <meta http-equiv="pragma" content="no-cache">
 <meta http-equiv="cache-control" content="no-cache">
 <meta http-equiv="expires" content="0">


  css reset 是最早的一种解决浏览器间样式不兼容问题的方案，它的基本思想是将浏览器的所有样式都重置掉，从而达到所有浏览器
 样式保持一致的效果。但是使用这种方法，可能会带来一些性能上的问题，并且对于一些元素的不必要的样式的重置，其实反而会造成画蛇添足的效果。

 后面出现一种更好的解决浏览器间样式不兼容的方法，就是 normalize.css ，它的思想是尽量的保留浏览器自带的样式，通过在原有的样式的基础上进行调整，来保持各个浏览器间的样式表现一致。

  预格式化就是保留文字在源码中的格式 最后显示出来样式与源码中的样式一致 所见即所得。
 <pre> 定义预格式文本，保持文本原有的格式

  下面这些标签可用在 head 部分：<base>, <link>, <meta>, <script>, <style>, 以及 <title>。
 <title> 定义文档的标题，它是 head 部分中唯一必需的元素。

  HTML 的注释方法 <!--注释内容--> 
 CSS 的注释方法 /*注释内容*/ 
 JavaScript 的注释方法 /* 多行注释方式 */ //单行注释方式

  mozilla 内核 （firefox,flock 等）    -moz
 webkit  内核 （safari,chrome 等）   -webkit
 opera   内核 （opera 浏览器）        -o
 trident 内核 （ie 浏览器）           -ms


前端性能优化主要是为了提高页面的加载速度，优化用户的访问体验。我认为可以从这些方面来进行优化。

 第一个方面是页面的内容方面

 （1）通过文件合并、css 雪碧图、使用 base64 等方式来减少 HTTP 请求数，避免过多的请求造成等待的情况。

 （2）通过 DNS 缓存等机制来减少 DNS 的查询次数。

 （3）通过设置缓存策略，对常用不变的资源进行缓存。

 （4）使用延迟加载的方式，来减少页面首屏加载时需要请求的资源。延迟加载的资源当用户需要访问时，再去请求加载。

 （5）通过用户行为，对某些资源使用预加载的方式，来提高用户需要访问资源时的响应速度。

 第二个方面是服务器方面

 （1）使用 CDN 服务，来提高用户对于资源请求时的响应速度。

 （2）服务器端启用 Gzip、Deflate 等方式对于传输的资源进行压缩，减小文件的体积。

 （3）尽可能减小 cookie 的大小，并且通过将静态资源分配到其他域名下，来避免对静态资源请求时携带不必要的 cookie

 第三个方面是 CSS 和 JavaScript 方面

 （1）把样式表放在页面的 head 标签中，减少页面的首次渲染的时间。

 （2）避免使用 @import 标签。

 （3）尽量把 js 脚本放在页面底部或者使用 defer 或 async 属性，避免脚本的加载和执行阻塞页面的渲染。

 （4）通过对 JavaScript 和 CSS 的文件进行压缩，来减小文件的体积。

扫描二维码登录网页是什么原理： 浏览器获得一个临时 id，通过长连接等待客户端扫描带有此 id  的二维码后，从长连接中获得客户端上报给 server的帐号信息进行展示。并在客户端点击确认后，获得服务器授信的令牌，进行随后的信息交互过程。在超时、网络断开、其他设备上登录后，此前获得的令牌或丢失、或失效，对授权过程形成有效的安全防护。

Html 规范中为什么要求引用资源不加协议头http或者https - 
我们可以省略 URL 的协议声明，省略后浏览器照样可以正常引用相应的资源，这项解决方案称为
  protocol-relative URL，暂且可译作协议相对 URL
  如果使用协议相对 URL，无论是使用 HTTPS，还是 HTTP 访问页面，浏览器都会以相同的协议请求页面中的资源，避免弹出类似
 的警告信息，同时还可以节省5字节的数据量。


 ---------

如果查找元素次数不多的话尽量使用getElementBy系列方法（因为性能高）。
如果查找元素次数比较多的话就使用querySelector方法（因为方便）。

querySelector()方法仅仅返回匹配指定选择器的第一个元素。如果你需要返回所有的元素，请使用 querySelectorAll() 方法替代。

document.getElementById("test") - 动态集合
document.querySelector("#test"); - 静态集合在取出来以后元素与文档的改变无关。

document.getElementsByClassName('red') - 动态集合
document.querySelector('.red') - 静态集合在取出来以后元素与文档的改变无关。


列出页面所有标签：[统计页面某标签 出现次数]
document.querySelectorAll('*')，标准规范实现
document.getElementsByTagName('*')
$$('*')，devtools 实现



协议，域名，端口，三者有一不一样，就是跨域

案例一：www.baidu.com 与 zhidao.baidu.com 是跨域

目前有两种最常见的解决方案：

CORS，在服务器端设置几个响应头，如 Access-Control-Allow-Origin: *
Reverse Proxy，在 nginx/traefik/haproxy 等反向代理服务器中设置为同一域名
JSONP，详解见 JSONP 的原理是什么，如何实现



什么是“技术知识”？知识就是 I KNOW

《TypeScript 从入门到放弃》
《React 从入门到放弃》
《Webpack 从入门到放弃》
......
什么是“技术能力”？能力就是 I CAN

我用 TypeScript 重构了一个大型系统，代码健壮性及研发效率大幅提升。
我用 React Hooks 给全栈同学进行前端培训，培训效果大幅提升。
我深入研究了 Webpack，优化配置，使得系统构建速度大幅提升。
.....



0-------------

Doctype是HTML5的文档声明; 如果没有声明
大部分浏览器将开启最大兼容模式来解析网页，我们一般称为怪异模式，这不仅会降低解析效率，而且会在解析过程中产生一些难以预知的bug

语义化 - 用正确的标签做正确的事情
有利于SEO / 可读性高 / 
<header>：定义页面或区块的页眉，通常包含导航、logo等内容。
<nav>：定义页面的导航部分。
<main>：定义页面的主要内容部分，每个页面应该只有一个<main>标签。
<article>：定义一个独立的、完整的内容块，比如博客文章或新闻内容。
<section>：定义文档中的一个区块，通常包含一个标题。
<aside>：定义页面的侧边栏内容，比如侧边栏导航、广告等。
<footer>：定义页面或区块的页脚，通常包含版权信息、联系方式等内容。

严格模式：是以浏览器支持的最高标准运行  use strict
混杂模式：页面以宽松向下兼容的方式显示，模拟老式浏览器的行为

常见的行级元素：span、a、img、button、input、select
块级元素- 宽度缺少时是它的容器的100%，除非设置一个宽度

使用行内元素需要注意的是：

行内元素设置宽度width无效
行内元素设置height无效，但是可以通过line-height来设置
设置margin只有左右有效，上下无效
设置padding只有左右有效，上下无效

H5新特性
1.Html5 绘画 canvas 元素    
2.用于媒介回放的 video 和 audio 元素
3.本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；sessionStorage 的数据在浏览器关闭后自动删除
4.语意化更好的内容元素，比如 article、footer、header、nav、section
表单控件，calendar、date、time、email、url、search
用正确的标签包含正确的内容,好处就是结构良好，便于阅读，方便威化，也有利于爬虫的查找，提高搜索率。
5.websocket  跨域，聊天室
6.缓存，允许自己控制那些文件需要缓存；给html添加manifest属性；

<canvas id="myCanvas" width="200" height="200"></canvas>
<script>
  var canvas = document.getElementById("myCanvas");
  var ctx = canvas.getContext("2d");
  ctx.fillStyle = "red";
  ctx.fillRect(50, 50, 100, 100);
</script>


CSS3有哪些新特性
1. 在布局方面新增了flex布局；
2. 在选择器方面新增了例如:first-of-type,nth-child等选择器；
3. 在盒模型方面添加了box-sizing来改变盒模型，
4. 在动画方面增加了animation、2d变换、3d变换等。在颜色方面添加透明、rgba等，
5. 在字体方面允许嵌入字体和设置字体阴影，同时当然也有盒子的阴影
6. 媒体查询。为不同设备基于它们的能力定义不同的样式。


cookielocalStoragesessionStorage由谁初始化客户端或服务器，服务器可以使用 Set-Cookie 请求头。客户端客户端过期时间手动设置永不过期当前页面关闭时在当前浏览器会话（browser sessions）中是否保持不变取决于是否设置了过期时间是否是否随着每个 HTTP 请求发送给服务器是，Cookies 会通过 Cookie 请求头，自动发送给服务器否否容量（每个域名）4kb5MB5MB访问权限任意窗口任意窗口当前页面窗口



```