---
bg: "railss.jpg"
layout: post
title:  "ajax原理及应用!"
crawlertitle: "How to use jekyll"
summary: "Javascript！"
date:   2015-11-03 11:00:47 +0700
categories: posts
tags: 'Javascript'
author: redVi
---
##### ajax 简单应用

Ajax的原理简单来说通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。这其中最关键的一步就是从服务器获得请求数据。要清楚这个过程和原理，我们必须对 XMLHttpRequest有所了解。

　XMLHttpRequest是ajax的核心机制，它是在IE5中首先引入的，是一种支持异步请求的技术。简单的说，也就是javascript可以及时向服务器提出请求和处理响应，而不阻塞用户。达到无刷新的效果。
所以我们先从XMLHttpRequest讲起，来看看它的工作原理。
首先，我们先来看看XMLHttpRequest这个对象的属性。
它的属性有：
onreadystatechange  每次状态改变所触发事件的事件处理程序。

responseText     从服务器进程返回数据的字符串形式。

responseXML    从服务器进程返回的DOM兼容的文档数据对象。

status           从服务器返回的数字代码，比如常见的404（未找到）和200（已就绪）

status Text       伴随状态码的字符串信息

readyState       对象状态值

0 (未初始化) 对象已建立，但是尚未初始化（尚未调用open方法）

1 (初始化) 对象已建立，尚未调用send方法

2 (发送数据) send方法已调用，但是当前的状态及http头未知

3 (数据传送中) 已接收部分数据，因为响应及http头不全，这时通
过responseBody和responseText获取部分数据会出现错误，

4 (完成) 数据接收完毕,此时可以通过通过responseXml和responseText获取完整的回应数据
<pre><code>
var ajax=new XMLHttpRequest();
ajax.open('get或post请求','请求地址');
ajax.send();
ajax.onreadystatechange=function(){
if(ajax.readyState==4&&ajax.status==200){
ajax.responseText; //返回值
}
}	
</code></pre>

####js中常用的系统函数

  Split(','):把一个数组用逗号连接起来

  pop()：该方法用于删除并返回数组元素中最后一个元素

  shift()：该方法用于删除并返回数组的第一个元素

  push()：该方法向数组末尾添加一个或多个元素，并返回添加后
  的数组长度

  unshift()：该方法是向数组开头添加一个或者多个元素，并返回
  添加后的数组长度

  reverse()：该方法用于颠倒数组元素的顺序

  sort()：该方法用于将数组进行排序

####时间日期对象 date 类

  方式一：var date=new Date()  //创建一个当前时间

  方式二：var date=new Date(2014,8,10) 指定年月日创建时间 写8实际上是9月份系统默认多一个月

  方式三：var date=new Date(“Jan 2,2008 10:21:22”)根据字符串创建对象

  date()：返回系统当前的日期和时间

  getDate()：从日期对象中返回一个月中的某一天

  getDay()：从日期对象中返回一周中的某一天

  getMonth()：从日期对象中返回月份

  getYear()：从日期对象中返回年份

  getHours()：从日期对象中返回小时

  getMinutes()：从日期对象中返回分钟

  getSeconds()：从日期对象中返回秒数

  getTime()：返回从1970年1月1日至今的毫秒数

  toLocaleString()根据本地时间格式，把日期对象转换为字符串

####基本找对象方法
 1、通过id找对象：document.getElementById(“元素id”)

 2、通过name找对象：document.getElementsByName(“元素name”)

 3、通过标签找对象：document.getElementsByTageName(“标签名”)

 4、通过forms找对象：document.forms[0]找到html文档中第一个表单，[]里的下标也可以写成表单的name属性

####javascript通过style属性来修改css
通过id或Nam或标签找到对象后：用对象名.style.css属性名来操作修改css样式例如：点击按钮，让div显示或隐藏

>`<script>`
Function fun(){
Document.getElementById(“main”).style.display="none"
}
`</script>`
`<div id='main'></div><input type='button' onclick='fun()'>`

####history对象的方法
<pre><code>
History.go(-1)：后退
History.go(0)：刷新
History.go(1)：前进
History.back()：后退
History.forward()：前进	
</code></pre>	


####灵活运用window对象的方法
window.alert()：弹出一个带有确定按钮的对话框

window.confirm(确认信息)
：弹出一个带有确认和取消按钮的对话框,返回bool类型true或false如果点确定，代表 true,否则false;

window.prompt(提示信息)：弹出一个带有输入框的对话框

window.open(url,’_blank’,style)：打开一个新的窗口

window.print() 打印

window.close()：关闭浏览器