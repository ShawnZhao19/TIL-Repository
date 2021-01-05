<script>
1.定义变量   变量类型 变量名=变量值
2.条件控制
</script>

数据类型
1.number  JS不区分小数和整数
2.===为绝对等于（类型一样，值一样）
3.NaN===NaN  结果为false  这个与所有的数值都不相等。
4.对象：对象是大括号，数组是中括号。每个属性之间用逗号隔开，最后一个不需要添加


严格检查模式
1.'use strict'; 严格检查模式，预防JS的随意性导致产生的一些问题
2.局部变量建议都使用let去定义
3.必须写到JS的第一行


字符串
1.正常字符串我们使用单引号或者双引号包裹
2.注意转义字符 \ 
3.`***` 是多行字符串编写
4.大小写转换 student.toUpperCase()/toLowerCase()
5.student.substring(1)截取从第一个字符之外 输出其他的
6.indexOf，通过元素获得下标索引
7.slice()截取array的一部分，返回一个新数组，类似于string中的substring
8.push（）尾部插入元素  pop（）尾部弹出元素
9.unshift（）头部插入元素 shift（）头部弹出元素
10.sort()排序
11.reverse()元素反转
12.concat()并没有修改数组，只是会返回一个新的数组
13.jion连接符


对象（若干个键值对）
1.var 对象名={属性名：属性值   属性名：属性值}    JS中的对象使用大括号表示
2.delete 可以删除对象属性
3.判断属性值是否在这个对象中   xxx in xxx
4.xxx.hasOwnProperty（）判断是否有什么属性、
5.forEach循环  age.forEach(function (value){console.log(value)})
6.set:无序不重复的集合
7.map：var map = new Map{['key',value]}
8.遍历map：for（lex x of map）



函数
1.不存在参数，手动抛出异常来判断 if (typeof x!== 'number' ){throw '****'}
2.arguments：是一个JS赠送的关键字，他代表传递进来的所有的参数，是一个数组
3.rest获取除了已经第一的参数之外的所有参数

变量的作用域
1.假设在函数体中声明，那么在函数体外不可以使用，如果非要使用，则研究一下闭包
2.内部函数，可以访问外部函数的成员 
3.如果在JS中函数查找变量从自身函数开始，由内向外查找。假设外部存在这个内部的变量名，内部函数会屏蔽外部函数的变量
4.建议大家使用let去定义局部作用域
5.const常量关键字


方法
1.就是把函数放在对象的里面，对象只有两个东西：属性和方法
2.var now = new Date（）.getFullYear（）  获取当前年
3.this是无法指向的，是默认指向调用它的对象
4.apply


内部对象
1.Date
2.JSON：任何JS支持的类型都可以用JSON来表示
3.JSON.prase（）为解析数据    将字符串转换为对象
4.JSON.stringify（） 将对象转换为字符串


面向对象编程
1.继承extends
2.操作BOM对象（重点）
BOM：浏览器对象模型
（1）windw代表浏览器窗口
（2）navigator，封装了浏览器的信息
（3）location代表当前页面的URL信息   location.assign（******） 可以设置新的地址
（4）document代表当前的页面，HTML DOM文档树
（5）document可以直接获得rookie
（6）history 中有两个方法history.back()    history.forward()

3.操作DOM对象（重要）
1.DOM:文档对象模型
（1）document.getElementsByTagName（''） 是获得 CSS中的标签结点
2.删除DOM结点：先获取父节点，再通过父节点去删除自己  father.removeChild()

JQuery
1.CDN 在线引入   搜索   CDN JQuery
2.公式 $(selector).action()是唯一公式     selectior是选择器的意思，选择器就是css的选择器 action是事件的意思
3.action分为鼠标事件 键盘事件 其他事件   mouse为鼠标事件            key 为键盘事件
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          