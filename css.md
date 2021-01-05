基本选择器
1.标签选择器一般用于全局
2.类选择器是.class设置布局，可以复用
3.id选择器为#id，必须保证全局唯一
4.选择器不遵循就近原则，id选择器大于类选择器大于标签选择器



层次选择器
1.后代选择器  body ***{}
2.子选择器  body>p{}
3.相邻兄弟选择器 .class + ***{}
4.通用选择器 .class~***{}



结构伪类选择器
1.p:pth-child(*){}选择当前p元素的父级元素，选中父级元素的第一个，并且是当前元素才生效
2.p:pth-of-type(*){}选中父元素下的p元素的第二个类型



属性选择器
比如a【id】{}   属性名，属性名=属性值(正则)
=为绝对等于，*为通配符为包含，范围更广
^=为以这个属性为开头
$=为以这个属性为结尾


美化网页元素
1.span标签为强调作用
2.font-family表示为字体
3.RGBA中的A为透明度，0-1是范围
4.text-align为排版一般为居中
5.text-indent：2em为首行缩进两个字
6.line-height为行高
7.text-decoration:为各种划线

超链接伪类
a:hover{}鼠标悬浮的颜色
a:active{}鼠标按住未释放的状态颜色
a:visited{}鼠标点击之后的状态颜色
text-shadow为阴影颜色，属性为阴影颜色，水平偏移，垂直偏移，阴影半径


列表
1.font-weight为字体加粗
2.list-style为样式设置为none代表去掉原点  为circle为空心圆



背景
1.background-repeat：no repeat
2.background综合属性：颜色，图片，图片位置，平铺否

渐变
https://www.grabient.com/

盒子模型
1.margin为外边距
2.padding为内边距
3.border为边框

边框
border综合属性：粗细，样式，颜色  solid为实线
body总有一个默认的外边距margin：0 padding：0  这是最开始要设置的也叫做初始化动作

外边距
1.妙用：margin：0 auto


圆角边框
1.border-radius：圆角

盒子阴影
1.box-shadow：x，y，模糊半径，颜色

浮动
1.块级元素：独占一行   h1-h6   p  div   列表
2.行内元素：不独占一行  span a img strong ...
3.display
4.float浮动
5.clear：清除浮动
6.overflow是解决溢出的问题
7.在父类中增加一个伪类：  father：after{content：‘’  display：block变成块级元素    clear：both}


定位
1.相对定位：相对于自己原来的位置进行偏移
2.绝对定位：没有父级元素定位的前提下，相对于浏览器定位；假设父级元素存在定位，我们通常会相对于父级元素进行偏移；在父级元素范围内移动
3.fixed固定定位
3.z-index：层级。漂浮 0-999 一般999
4.opacity：背景透明度        