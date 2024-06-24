```js
Target - 2 css

谨记： 对简历写的东西及周边要很清楚，能举一反三； 因为这是面试官问的重点
面试不是看你啥不会，而是看你回的东东；所以不会的东西就直接说不熟悉就好！！！

盒模型都是由四个部分组成的，分别是margin、border、padding和content。

标准盒模型和IE盒模型的区别在于设置width和height时，所对应的范围不同。标准盒模型的width和height属性的
范围只包含了content，而IE盒模型的width和height属性的范围包含了border、padding和content。

一般来说，我们可以通过修改元素的box-sizing属性来改变元素的盒模型。
    box-sizing: border-box; / content-box;

（1）id选择器（#myid）
（2）类选择器（.myclassname）
（3）标签选择器（div,h1,p）
- 后代选取器(以空格分隔)        div  p             后代选择器是作用于所有子后代元素 
- 子元素选择器(以大于号分隔）    div > p         子选择器（child selector）仅是指它的直接后代
- 相邻兄弟选择器（以加号分隔）   div + p   选取了所有位于 <div> 元素后的第一个 <p> 元素, 且div p有相同的父元素
- 后继兄弟选择器（以破折号分隔）  div ~ p      所有 <div> 元素之后的所有相邻兄弟元素 <p> 
（8）属性选择器（a[rel="external"]）
（9）伪类选择器（a:hover,li:nth-child）
（10）伪元素选择器（::before、::after）
（11）通配符选择器（*）


伪类用于当已有的元素处于某个状态时，为其添加对应的样式，
伪类比如p:active, p:hover都是指元素p的一个“状态” 【active focus hover link visited first-child】
而伪元素的效果则需要通过添加一个实际的元素才能达到 [after before first-letter]
伪元素用于创建一些不在文档树中的元素，并为其添加样式。

:first-child 匹配的是某父元素的第一个子元素，可以说是结构上的第一个子元素。 不管该元素是啥
如果 p:first-child 意味着 某父元素的第一个子元素 且该元素为 p 时，则应用style 否则无效
也可以 :first-child 直接使用意味着 某父元素的第一个子元素，不管该元素是啥

:first-of-type 匹配的是该类型的第一个 【first-of-type只对元素类型生效, 并不对class类型生效。】
p:first-of-type

同样类型的选择器 :last-child  和 :last-of-type、:nth-child(n)  和  :nth-of-type(n) 也可以这样去理解。

单冒号（:）用于CSS3伪类，双冒号（::）用于CSS3伪元素。
伪类一般匹配的是元素的一些特殊状态，如hover、link等，而伪元素一般匹配的特殊的位置，比如after、before等。

------属性继承
每一个属性在定义中都给出了这个属性是否具有继承性，一个具有继承性的属性会在没有指定值的时候，会使用父元素的同属性的值
来作为自己的值。
一般具有继承性的属性有，字体相关的属性，font-size和font-weight等。文本相关的属性，color和text-align等。
表格的一些布局属性、列表属性如list-style等。还有光标属性cursor、元素可见性visibility。

当一个属性不是继承属性的时候，我们也可以通过将它的值设置为inherit来使它从父元素那获取同名的属性值来继承。
如 box-sizing: inherit


判断优先级时，首先我们会判断一条属性声明是否有权重，也就是是否在声明后面加上了!important。一条声明如果加上了权重，
那么它的优先级就是最高的，前提是它之后不再出现相同权重的声明。如果权重相同，我们则需要去比较匹配规则的特殊性。
第一个等级是行内样式，为1000，第二个等级是id选择器，为0100，第三个等级是类选择器、伪类选择器和属性选择器，为0010，
第四个等级是元素选择器和伪元素选择器，为0001。  特殊性值为1000的的规则优先级就要比特殊性值为0999的规则高
如果两个规则的特殊性值相等的时候，那么就会根据它们引入的顺序，后出现的规则的优先级最高。

LVHA
a标签有四种状态：链接访问前、链接访问后、鼠标滑过、激活，分别对应四种伪类:link、:visited、:hover、:active；
要生效的话， 按照这个顺序

CSS3 新增伪类
:nth-child(n)  选中父元素下的第n个子元素
:nth-last-child(n) 作用同上，不过是从后开始查找。
:last-child选中最后一个子元素

:nth-of-type(n)选中父元素下第n个elem类型元素
:not(elem)选择非elem元素的每个元素



如何居中 div？

对于宽高固定的元素
（1）我们可以利用margin:0 auto来实现元素的水平居中。

（2）利用绝对定位，设置四个方向的值都为0，并将margin设置为auto，由于宽高固定，因此对应方向实现平分，可以实现水
平和垂直方向上的居中。

（3）利用绝对定位，先将元素的左上角通过top:50%和left:50%定位到页面的中心，然后再通过margin负值来调整元素
的中心点到页面的中心。 margin: -150px 0 0 -250px;/*外边距为自身宽高的一半*/

（4）利用绝对定位，先将元素的左上角通过top:50%和left:50%定位到页面的中心，然后再通过translate来调整元素
的中心点到页面的中心。   transform: translate(-50%, -50%);

（5）使用flex布局，通过align-items:center和justify-content:center设置容器的垂直和水平方向上为居中对
齐，然后它的子元素也可以实现垂直和水平的居中。

对于宽高不定的元素，上面的后面两种方法，可以实现元素的垂直和水平的居中。

display - 
block	块类型。默认宽度为父元素宽度，可设置宽高，换行显示。
none	元素不显示，并从文档流中移除。
inline	行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。
inline-block 默认宽度为内容宽度，可以设置宽高，同行显示。
list-item	像块类型元素一样显示，并添加样式列表标记。
table	此元素会作为块级表格来显示。
inherit	规定应该从父元素继承display属性的值。
flex 弹性布局， 使得子元素能够灵活地排列在一个方向上（水平或垂直）【块级元素】
inline-flex  弹性布局，【内联元素】

[position - ]
absolute
生成绝对定位的元素，离自己这一级元素最近的一级position设置为absolute或者relative的祖先元素的padding box的左上角为原点的。

fixed（老IE不支持）
生成绝对定位的元素，相对于浏览器窗口进行定位。

relative
生成相对定位的元素，相对于其元素本身所在正常位置进行定位。

static
默认值。没有定位，元素出现在正常的流中（忽略top,bottom,left,right,z-index声明）。

inherit
规定从父元素继承position属性的值。

CSS3 新特性 
新增各种CSS选择器	（:not(.input)：所有class不是“input”的节点）
圆角		（border-radius:8px）
文字特效		（text-shadow） text-shadow: h-shadow v-shadow blur-radius color|none|initial|inherit;

文字渲染		（Text-decoration） text-decoration: underline;
text-decoration: overline red;

线性渐变		（gradient） background: linear-gradient(#f69d3c, #3f87a6); background: radial-gradient(#f69d3c, #3f87a6);
旋转			（transform） CSS transform 属性允许你旋转，缩放，倾斜或平移给定元素。
缩放，定位，倾斜，动画，多背景
例如：transform:\scale(0.85,0.90)\translate(0px,-30px)\skew(-9deg,0deg)\Animation: \ rotate


flex布局
设为Flex布局以后，子元素的float、clear和vertical-align属性将失效。
容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis），项目默认沿主轴排列。

以下6个属性设置在容器上。----------
flex-direction属性决定主轴的方向（即项目的排列方向）。

flex-wrap属性定义，如果一条轴线排不下，如何换行。

flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

justify-content属性定义了项目在主轴上的对齐方式。

align-items属性定义项目在交叉轴上如何对齐。

align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

以下6个属性设置在项目上。-------------
order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。它的默认
值为auto，即项目的本来大小。

flex属性是flex-grow，flex-shrink和flex-basis的简写，默认值为0 1 auto。

align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父
元素的align-items属性，如果没有父元素，则等同于stretch。


CSS 创建三角形
采用的是相邻边框连接处的均分原理。
  将元素的宽高设为0，只设置
  border
  ，把任意三条边隐藏掉（颜色设为
  transparent），剩下的就是一个三角形。
  #demo {
  width: 0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}


满屏品字 布局
简单的方式：
	上面的div宽100%，
	下面的两个div分别宽50%，
	然后用float 或 设置 absolut left / right

多列等高如何实现
使用Flexbox或者CSS表格布局（display: table）来实现； 
display: flex; flex: 1;自动拉伸以填充可用空间

浏览器兼容性问题
浏览器默认的margin和padding不同
解决方案：加一个全局的*{margin:0;padding:0;}来统一。

超链接访问过后hover样式就不出现了，被点击访问过的超链接样式不再具有hover和active了
解决方法：改变CSS属性的排列顺序L-V-H-A

为什么要对CSS初始化样式
-因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。
-当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。
最简单的初始化方法：*{padding:0;margin:0;}（强烈不建议）
淘宝的样式初始化代码：对不同标签，有不同的初始化代码

包含块（containing block）就是元素用来计算和定位的一个框。
（1）根元素（很多场景下可以看成是<html>）被称为“初始包含块”，其尺寸等同于浏览器可视窗口的大小。
（2）对于其他元素，如果该元素的position是relative或者static，则“包含块”由其最近的块容器祖先盒的content box
边界形成。
（3）如果元素position:fixed，则“包含块”是“初始包含块”。
（4）如果元素position:absolute，则“包含块”由最近的position不为static的祖先元素建立

CSS 里的 visibility 属性有个 collapse 属性值是干嘛用的？在不同浏览器下以后什么区别？
（1）对于一般的元素，它的表现跟visibility：hidden;是一样的。元素是不可见的，但此时仍占用页面空间。
（2）但例外的是，如果这个元素是table相关的元素，例如table行，table group，table列，table column group，它的
表现却跟display:none一样，也就是说，它们占用的空间也会释放。

在不同浏览器下的区别：
在谷歌浏览器里，使用collapse值和使用hidden值没有什么区别。
在火狐浏览器、Opera和IE11里，使用collapse值的效果就如它的字面意思：table的行会消失，它的下面一行会补充它的位
置。


width:100%会使元素box的宽度等于父元素的content box的宽度。
width:auto会使元素撑满整个父元素，margin、border、padding、content区域会自动分配水平空间。[里面有占满整个宽度的情况下]

绝对定位元素的宽高百分比是相对于临近的position不为static的祖先元素的padding box来计算的。
非绝对定位元素的宽高百分比则是相对于父元素的content box来计算的。

base64编码是一种图片处理格式，通过特定的算法将图片编码成一长串字符串，在页面上显示的时候，可以用该字符串来代替图片的
url属性。
使用base64的优点是：
（1）减少一个图片的HTTP请求

使用base64的缺点是：
（1）根据base64的编码原理，编码后的大小会比原文件大小大1/3，如果把大图片编码到html/css中，不仅会造成文件体
积的增加，影响文件的加载速度，还会增加浏览器对html或css文件解析渲染的时间。
（2）使用base64无法直接缓存，要缓存只能缓存包含base64的文件，比如HTML或者CSS，这相比域直接缓存图片的效果要
差很多。
（3）兼容性的问题，ie8以前的浏览器不支持。
一般一些网站的小图标可以使用base64图片来引入。

首先我们判断display属性是否为none，如果为none，则position和float属性的值不影响元素最后的表现。
"position:absolute"和"position:fixed"优先级最高，有它存在的时候，浮动不起作用，'display'的值也需要调整；
其次，元素的'float'特性的值不是"none"的时候或者它是根元素的时候，调整'display'的值；最后，非根元素，并且非浮动元素，并且非绝对定位的元素，'display'特性值同设置值。


margin重叠指的是在垂直方向上，两个相邻元素的margin发生重叠的情况。
一般来说可以分为四种情形：
第一种是相邻兄弟元素的marin-bottom和margin-top的值发生重叠。这种情况下我们可以通过设置其中一个元素为BFC
来解决。
第二种是父元素的margin-top和子元素的margin-top发生重叠。它们发生重叠是因为它们是相邻的，所以我们可以通过这
一点来解决这个问题。我们可以为父元素设置border-top、padding-top值来分隔它们，当然我们也可以将父元素设置为BFC
来解决。
第三种是高度为auto的父元素的margin-bottom和子元素的margin-bottom发生重叠。它们发生重叠一个是因为它们相
邻，一个是因为父元素的高度不固定。因此我们可以为父元素设置border-bottom、padding-bottom来分隔它们，也可以为
父元素设置一个高度，max-height和min-height也能解决这个问题。当然将父元素设置为BFC是最简单的方法。
第四种情况，是没有内容的元素，自身的margin-top和margin-bottom发生的重叠。我们可以通过为其设置border、pa
dding或者高度来解决这个问题。

BFC 规范（块级格式化上下文：block formatting context）
BFC是一个独立的布局环境，可以理解为一个容器，在这个容器中按照一定规则进行物品摆放，并且不会影响其它环境中的物品。
•如果一个元素符合触发BFC的条件，则BFC中的元素布局不受外部影响。

创建BFC
（1）根元素或包含根元素的元素
（2）浮动元素float＝left|right或inherit（≠none）
（3）绝对定位元素position＝absolute或fixed
（4）display＝inline-block|flex|inline-flex|table-cell或table-caption
（5）overflow＝hidden|auto或scroll(≠visible)

IFC指的是行级格式化上下文，它有这样的一些布局规则：
（1）行级上下文内部的盒子会在水平方向，一个接一个地放置。
（2）当一行不够的时候会自动切换到下一行。
（3）行级上下文的高度由内部最高的内联盒子的高度决定。

需要清除浮动的主要原因包括：
避免父元素高度塌陷：当父元素包含浮动元素时，如果没有清除浮动，父元素无法正确计算子元素的高度，从而导致父元素高度塌陷，影响整体布局。

使用clear属性：在浮动元素的下方添加一个空的元素，并为该元素设置clear:both;属性。这样可以清除前面浮动元素对布局的影响。<div style="clear:both;"></div>
我们对元素设置clear属性是为了避免浮动元素对该元素的影响，而不是清除掉浮动。

使用overflow属性：将父元素设置为overflow:hidden;或overflow:auto;可以触发BFC（块级格式化上下文），从而清除浮动

一般使用伪元素的方式清除浮动
在父元素上应用 - 
.clear::after{
content:'';
display:table;//也可以是'block'，或者是'list-item'
clear:both;
}


当媒体查询为真时，相关的样式表或样式规则会按照正常的级联规被应用。当媒体查询返回假，标签上带有媒体查询的样式表仍将被
下载（只不过不会被应用）。
包含了一个媒体类型和至少一个使用宽度、高度和颜色等媒体属性来限制样式表范围的表达式。
CSS3 @media
@media only screen and (max-width: 500px) {


CSS预处理器（Sass、LESS 和 Stylus）
为CSS增加一些编程的特性，无需考虑浏览器的兼容问题，例如可以在CSS中使用变量、简单的程序逻辑、函数等等在编程语言中的一些基本技巧，可以让CSS更加简洁、适应性更强、代码更直观。
变量符不一样，Less是@，而Scss是$


css 优化 提升性能
加载性能：
（1）css压缩：将写好的css进行打包压缩，可以减少很多的体积。
（3）减少使用@import,而建议使用link，因为后者在页面加载时一起加载，前者是等待页面加载完成之后再进行加载。


选择器性能：
（1）关键选择器（key selector）。选择器的最后面的部分为关键选择器 【不要嵌套太深】
（3）避免使用通配规则，如*{}计算次数惊人！只对需要用到的元素进行选择。
（4）尽量少的去对标签进行选择，而是用class。
5. 尽量将选择器的深度降到最低，最高不要超过
三层，更多的使用类来关联每一个标签元素。
（6）了解哪些属性是可以通过继承而来的，然后避免对这些属性重复指定规则。

渲染性能：
（1）慎重使用高性能属性：浮动、定位。
（2）尽量减少页面重排、重绘。
（3）去除空规则：｛｝。空规则的产生原因一般来说是为了预留样式。去除这些空规则无疑能减少css文档体积。
（4）属性值为0时，不加单位。
（7）不使用@import前缀，它会影响css的加载速度。
（8）选择器优化嵌套，尽量避免层级过深。
（9）css雪碧图

解析 CSS 选择器
如果采取从右向左的方式，那么只要发现最右边选择器不匹配，就可以直接舍弃了，避免了许多无效匹配。

偶数字号相对更容易和web设计的其他部分构成比例关系。


bg 设置 是在padding及以内

抽离样式模块怎么写，说出思路 ---
把常用的css样式单独做成css文件……通用的和业务相关的分离出来，通用的做成样式模块儿共享，业务相关的，放
进业务相关的库里面做成对应功能的模块儿。

all属性实际上是所有CSS属性的缩写，表示，所有的CSS属性都怎样怎样
initial是初始值的意思
inherit是继承的意思
unset是取消设置的意思，也就是当前元素浏览器或用户设置的CSS忽略

通配符，需要把所有的标签都遍历一遍，大大的加强了网站运行的负载，会使网站加载的时候需要很长一段时间，因此一般大型的网站都有分层次的一套初始化样式。
出于性能的考虑，并不是所有标签都会有padding和margin，因此对常见的具有默认padding和margin的元素初始化即
可，并不需使用通配符*来初始化。

响应式网站设计是一个网站能够兼容多个终端，而不是为每一个终端做一个特定的版本。基本原理是通过媒体查询检测不同的设备屏
幕尺寸做处理。页面头部必须有meta声明的viewport。


怎么让 Chrome 支持小于 12px 的文字？
在谷歌下css设置字体大小为12px及以下时，显示都是一样大小，都是默认12px。
- 可以使用css3的transform缩放属性-webkit-transform:scale(0.5);注意-webkit-transform:scale(0.
75);收缩的是整个元素的大小，这时候，如果是内联元素，必须要将内联元素转换成块元素，可以使用display：block/
inline-block/...；
- 使用图片：如果是内容固定不变情况下，使用将小于12px文字内容切出做图片，这样不影响兼容也不影响美观


webkit内核的私有属性：-webkit-font-smoothing，用于字体抗锯齿，使用后字体看起来会更清晰舒服。
在MacOS测试环境下面设置-webkit-font-smoothing:antialiased;但是这个属性仅仅是面向MacOS，其他操作系统设
置后无效。

font-style
italic是使用当前字体的斜体字体，而oblique只是单纯地让文字倾斜。

设备像素指的是物理像素，一般手机的分辨率指的就是设备像素
css像素和设备独立像素是等价的，不管在何种分辨率的设备上，css像素的大小应该是一致的

dpr指的是设备像素和设备独立像素的比值，一般的pc屏幕，dpr=1
ppi指的是每英寸的物理像素的密度，ppi越大，屏幕的分辨率越大。


多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60*1000ms＝16.7ms

如何让去除 inline-block 元素间间距？
移除空格、使用margin负值、使用font-size:0、letter-spacing、word-spacing

overflow:scroll 时不能平滑滚动的问题怎么处理？
以下代码可解决这种卡顿的问题：-webkit-overflow-scrolling:touch;是因为这行代码启用了硬件加速特性，所以滑动很流
畅。

（1）第一种是BMP格式，它是无损压缩的，支持索引色和直接色的点阵图。由于它基本上没有进行压缩，因此它的文件体积一般比
较大。

（2）第二种是GIF格式，它是无损压缩的使用索引色的点阵图。由于使用了LZW压缩方法，因此文件的体积很小。并且GIF还
支持动画和透明度。但因为它使用的是索引色，所以它适用于一些对颜色要求不高且需要文件体积小的场景。

（3）第三种是JPEG格式，它是有损压缩的使用直接色的点阵图。由于使用了直接色，色彩较为丰富，一般适用于来存储照片。但
由于使用的是直接色，可能文件的体积相对于GIF格式来说更大。

（4）第四种是PNG-8格式，它是无损压缩的使用索引色的点阵图。它是GIF的一种很好的替代格式，它也支持透明度的调整，并
且文件的体积相对于GIF格式更小。一般来说如果不是需要动画的情况，我们都可以使用PNG-8格式代替GIF格式。

（5）第五种是PNG-24格式，它是无损压缩的使用直接色的点阵图。PNG-24的优点是它使用了压缩算法，所以它的体积比BMP
格式的文件要小得多，但是相对于其他的几种格式，还是要大一些。

（6）第六种格式是svg格式，它是矢量图，它记录的图片的绘制方式，因此对矢量图进行放大和缩小不会产生锯齿和失真。它一般
适合于用来制作一些网站logo或者图标之类的图片。

（7）第七种格式是webp格式，它是支持有损和无损两种压缩方式的使用直接色的点阵图。使用webp格式的最大的优点是，在相
同质量的文件下，它拥有更小的文件体积。因此它非常适合于网络图片的传输，因为图片体积的减少，意味着请求时间的减小，
这样会提高用户的体验。这是谷歌开发的一种新的图片格式，目前在兼容性上还不是太好。


PNG 格式的文件大小 》 JPEG格式的文件大小。这是因为JPEG采用有损压缩算法，可以在一定程度上减小文件大小，但会损失一些图像细节；而PNG采用无损压缩算法，保留了图像的所有细节，因此通常会产生较大的文件大小。



浏览器如何判断是否支持 webp 格式图片
（1）宽高判断法。通过创建image对象，将其src属性设置为webp格式的图片，然后在onload事件中获取图片的宽高，如
果能够获取，则说明浏览器支持webp格式图片。如果不能获取或者触发了onerror函数，那么就说明浏览器不支持webp格
式的图片。
（2）canvas判断方法。我们可以动态的创建一个canvas对象，通过canvas的toDataURL将设置为webp格式，然后判断
返回值中是否含有image/webp字段，如果包含则说明支持WebP，反之则不支持。

静态资源放CDN。
因为cookie有域的限制，因此不能跨域提交请求，故使用非主要域名的时候，请求头中就不会带有cookie数据，这样可以降低请
求头的大小，降低请求时间，从而达到降低整体请求延时的目的。

页面加载自上而下当然是先加载样式。写在body标签后由于浏览器以逐行方式对HTML文档进行解析，当解析到写在尾部的样式
表（外联或写在style标签）会导致浏览器停止之前的渲染，等待加载且解析样式表完成之后重新渲染



预处理器例如：LESS、Sass、Stylus，用来预编译Sass或less csssprite，增强了css代码的复用性，还有层级、mixin、
变量、循环、函数等，具有很方便的UI组件模块化开发能力，极大的提高工作效率。

后处理器例如：PostCSS，通常被视为在完成的样式表中根据CSS规范处理CSS，让其更有效；目前最常做的是给CSS属性添加浏
览器私有前缀，实现跨浏览器兼容性的问题。

--------CSSSprites
将一个页面涉及到的所有图片都包含到一张大图中去，然后利用CSS的background-image，background-repeat，background
-position的组合进行背景定位。利用CSSSprites能很好地减少网页的http请求，从而很好的提高页面的性能；CSSSprites
能减少图片的字节。

优点：
减少HTTP请求数，极大地提高页面加载速度
增加图片信息重复度，提高压缩比，减少图片大小
更换风格方便，只需在一张或几张图片上修改颜色或样式即可实现

缺点：
图片合并麻烦
维护麻烦，修改一个图片可能需要重新布局整个图片，样式

-----使用 rem 布局的优缺点？
优点：
在屏幕分辨率千差万别的时代，只要将rem与屏幕分辨率关联起来就可以实现页面的整体缩放，使得在设备上的展现都统一起来了。
而且现在浏览器基本都已经支持rem了，兼容性也非常的好。
缺点：
（1）在奇葩的dpr设备上表现效果不太好，比如一些华为的高端机型用rem布局会出现错乱。
（2）使用iframe引用也会出现问题。


rem 是 CSS3 新增的一个相对单位（root em），即相对 HTML 根元素的字体大小的值。
[以 rem 单位设计的弹性布局，是需要在头部加载一段脚本来进行监听分辨率的变化来动态改变根元素字体大小，使得 CSS 与 JS 耦合了在一起。]

相对单位 em: 也是一个相对单位，却是相对于当前对象内文本的字体大小。

一般建议在 line-height 使用 em。 [ 首行缩进]

视口单位 vw | vh


圣杯布局 - 比较特殊的三栏布局，同样也是两边固定宽度，中间自适应，唯一区别是dom结构必须是先写中间列部分，这样实现中间列可以优先加载。
.双飞翼布局 - 解决了圣杯布局错乱问题，实现了内容与布局的分离。而且任何一栏都可以是最高栏，不会出问题。

画一条0.5px 的线
采用transform:scale()的方式
.hr.scale-half {
    height: 1px;
    transform: scaleY(0.5);
    transform-origin: 50% 100%;
}
由于svg的描边等属性的1px是物理像素的1px，相当于高清屏的0.5px，另外还可以使用svg的rect等元素进行绘制
最后发现transfrom scale/svg的方法兼容性和效果都是最好的，svg可以支持复杂的图形


CSS3中规范引入了两种动画，分别是 transition 和 animation，transition可以让元素的CSS属性值的变化在一段时间内平滑的过渡，形成动画效果，为了使元素的变换更加丰富多彩，CSS3还引入了transfrom 属性，它可以通过对元素进行 平移(translate)、旋转(rotate)、放大缩小(scale)、倾斜(skew) 等操作，来实现2D和3D变换效果。

animation 需要设置一个@keyframes，来定义元素以哪种形式进行变换， 然后再通过动画函数让这种变换平滑的进行，从而达到动画效果
二者还有一个区别是：transition 只能通过主动改变元素的css值才能触发动画效果，而animation一旦被应用，就开始执行动画


对于普通文档流中的元素，百分比高度值要想起作用，其父级必须有一个可以生效的高度值。


（1）max-width会覆盖width，即使width是行类样式或者设置了!important。
（2）min-width会覆盖max-width，此规则发生在min-width和max-width冲突的时候。


属性设置 百分比 ——
公式：当前元素某CSS属性值 = 基准 * 对应的百分比
元素的 position 为 relative 和 absolute 时，top和bottom、left和right基准分别为包含块的 height、width
元素的 position 为 fixed 时，top和bottom、left和right基准分别为初始包含块（也就是视口）的 height、width，移动设备较为复杂，基准为 Layout viewport 的 height、width
元素的 height 和 width 设置为百分比时，基准分别为包含块的 height 和 width
元素的 margin 和 padding 设置为百分比时，基准为包含块的 width（易错）
元素的 border-width，不支持百分比
元素的 text-indent，基准为包含块的 width

元素的 border-radius，基准为分别为自身的height、width
元素的 background-size，基准为分别为自身的height、width
元素的 translateX、translateY，基准为分别为自身的height、width
元素的 line-height，基准为自身的 font-size

元素的 font-size，基准为父元素字体



网格间距是网格轨道之间的间距，可以通过 grid-column-gap，grid-row-gap 或者 grid-gap 在Grid 布局中创建。

gap 简写属性用于设置行与列之间的间隙（网格间距）。grid-gap = gap

----------

q.shanyue.tech

- 盒模型
盒模型都是由四个部分组成的，分别是margin、border、padding和content。

标准盒模型和IE盒模型的区别在于设置width和height时，所对应的范围不同。标准盒模型的width和height属性的范围只包含了content，而IE盒模型的width和height属性的范围包含了border、padding和content。

一般来说，我们可以通过修改元素的box-sizing属性来改变元素的盒模型。
    box-sizing: border-box; / content-box;


css specificity 即 css 中关于选择器的权重，以下三种类型的选择器依次下降
1. id 选择器，如 #app
2. class、attribute 与 pseudo-classes 选择器，如 .header、[type="radio"] 与 :hover
3. type 标签选择器和伪元素选择器，如 h1、p 和 ::before

其中通配符选择器 *，组合选择器 + ~ >，否定伪类选择器 :not() 对优先级无影响
另有内联样式 <div class="foo" style="color: red;"></div> 及 !important(最高) 具有更高的权重


单冒号（:）用于CSS3伪类，双冒号（::）用于CSS3伪元素。
伪类一般匹配的是元素的一些特殊状态，如hover、link等，【active focus hover link visited first-child】

:first-child 匹配的是某父元素的第一个子元素，可以说是结构上的第一个子元素。 不管该元素是啥
如果 p:first-child 意味着 某父元素的第一个子元素 且该元素为 p 时，则应用style 否则无效
也可以 :first-child 直接使用意味着 某父元素的第一个子元素，不管该元素是啥

:first-of-type 匹配的是该类型的第一个 【first-of-type只对元素类型生效, 并不对class类型生效。】
p:first-of-type


而伪元素一般匹配的特殊的位置，比如after、before等。[after before first-letter]

- 后代选取器(以空格分隔)        div  p             
后代选择器是作用于所有子后代元素 

- 子元素选择器(以大于号分隔）    div > p         
子选择器（child selector）仅是指它的直接后代

- 相邻兄弟选择器（以加号分隔）   div + p   
选取了所有位于 <div> 元素后的第一个 <p> 元素, 且div p有相同的父元素

- 后继兄弟选择器（以破折号分隔）  div ~ p      
所有 <div> 元素之后的所有相邻兄弟元素 <p> 


* + 选择器匹配紧邻的兄弟元素
* ~ 选择器匹配随后的所有兄弟元素

只有设置了 position 属性值为 relative、absolute 或 fixed 的元素才会受到 z-index 的影响。对于 static 定位的元素，z-index 不会起作用。

z-index高数值不一定在低数值前面，因为有层叠上下文的概念。当处于两个兄弟层叠上下文时，子元素的层级显示不决定于自身的z-index，而取决于父级的z-index

￼


水平垂直居中
* flex:
    * justify-content: center 【水平居中】
    * Align-items: center。  【垂直居中】


* grid [grid该元素的行为类似块级元素并且根据网格模型布局它的内容。]
    * place-items: center [align-items 和 justify-items 属性的简写]
使用 grid，它是做二维布局的，但是只有一个子元素时，一维布局与二维布局就一样了。
结合 justify-content/justify-items 和 align-content/align-items 就有四种方案
place-content: center;  [align-content 和 justify-content 属性的简写]

* absolute/translate
    .container {position: relative}
    .item { 
        position: absolute
        top: 50% [相对父容器]
        left: 50%
        transform: translate(-50%, -50%) 【相对自己宽高】
    }


* .box {  display: flex;}.item {  margin: auto;}

左侧固定 / 右侧自适应
方法1
// .container {
//   display: grid;
//   grid-template-columns: 200px 1fr; 【fr 是弹性系数，按照剩余比例分配剩余的空间】
// }

方法2
.container {
  display: flex;
}

.left {
  flex: 0 0 200px;
}

.main {
  flex: 1 1;
}


三均分布局
.grid-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 1rem;
}

.flex-container {
  display: flex;
  flex-wrap: wrap;
  // gap: 1rem;
  
  .item {
/*     flex-grow: 1;
    flex-basis: 0;
    flex-shrink: 1; */
    flex: 1 1 0;
  }
}

16列 均分布局
grid-template-columns: repeat(16, 1fr);

现代化的解决方案是使用长宽比的 CSS 属性: aspect-ratio： 1/1  【宽高比】

避免样式冲突 - 
BEM
.home-page {  .btn {}}

CSS Scoped
scoped css 会对当前组件(scope)下所有元素生成唯一的属性或类名
.home-module-card .module-title[data-v-4470f30c]

module css 
会对类名进行 hash 化
.container_3b1ac .label_e4019 .title_0174f

Css 变量
属性名需要以两个减号（--）开始，属性值则可以是任何有效的 CSS 值
自定义属性名是大小写敏感
:root{  --bgcolor: #aaa;  --color: #000;   --table-body: #fff }
  Div {
  Color: var(—bgcolor)
}

灰配色
filter: grayscale(1)
filter，filter 属性将模糊或颜色偏移等图形效果应用于元素
并用了 grayscale 对图片进行灰度转换，允许有一个参数，可以是数字（0到1）或百分比，0% 到 100% 之间的值会使灰度线性变化。

暗黑模式
  filter: invert(1) hue-rotate(180);
invert滤镜可以帮助反转应用程序的颜色方案，因此，黑色变成了白色，白色变成了黑色，所有颜色也是如此。
hue-rotate滤镜可以帮助我们处理所有其他非黑白的颜色。将色调旋转180度，我们确保应用程序的颜色主题不会改变，而只是减弱它的颜色。


```