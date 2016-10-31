---
bg: "railss.jpg"
layout: post
title:  "关于session cookie!"
crawlertitle: "How to use jekyll"
summary: "……session 和 cookie！"
date:   2015-10-30 12:00:47 +0700
categories: posts
tags: 'php'
author: redVi
---
##### session 和 cookie 的介绍
一、
什么是 Cookie？

Cookie 是一小段文本信息，伴随着用户请求和页面在 Web 服务器和浏览器之间传递。用户每次访问站点时，Web 应用程序都可以读取 Cookie 包含的信息。 Cookie 的基本工作原理如果用户再次访问站点上的页面，当该用户输入 URLwww.*****.com时，浏览器就会在本地硬盘上查找与该 URL 相关联的 Cookie。如果该 Cookie 存在，浏览器就将它与页面请求一起发送到您的站点。

Cookie 有哪些用途?

最根本的用途是:Cookie 能够帮助 Web 站点保存有关访问者的信息。更概括地说，Cookie 是一种保持Web 应用程序连续性（即执行“状态管理”）的方法.使 Web 站点记住您.
<pre><code>
参数 	描述
name 	必需。规定 cookie 的名称。
value 	必需。规定 cookie 的值。
expire 	可选。规定 cookie 的有效期。
path 	可选。规定 cookie 的服务器路径。
domain 	可选。规定 cookie 的域名。
secure 	可选。规定是否通过安全的 HTTPS 连接来传输 cookie。	
</code></pre>

二、Session的设置不同于Cookie，必须先启动，在PHP中必须调用session_start()。session_start()函数

当第一次访问网站时，Seesion_start()函数就会创建一个唯一的Session ID，并自动通过HTTP的响应头，将这个Session ID保存到客户端Cookie中。同时，也在服务器端创建一个以Session ID命名的文件，用于保存这个用户的会话信息。当同一个用户再次访问这个网站时，也会自动通过HTTP的请求头将Cookie中保存的Seesion ID再携带过来，这时Session_start()函数就不会再去分配一个新的Session ID，而是在服务器的硬盘中去寻找和这个Session ID同名的Session文件，将这之前为这个用户保存的会话信息读出，在当前脚本中应用，达到跟踪这个用户的目的。

<pre><code>
//启动session的初始化
session_start();
//注册session变量，赋值为一个用户的名称
$_SESSION["username"]="skygao";
//注册session变量，赋值为一个用户的ID
$_SESSION["uid"]=1;	
</code></pre>
执行该脚本后，两个Session变量就会被保存在服务器端的某个文件中，该文件的位置是通过php.ini文件，在session.save_path属性指定的目录下。
可以通过代码更改session的保存路径
<pre><code>
session_save_path($path)
</code></pre> 
在php.ini中设置 session.gc_maxlifetime = 1440 //默认时间

修改session的生存时间
$lifeTime = 24 * 3600;  // 保存一天session_set_cookie_params($lifeTime);
三、session 与 cookie 的区别

1、cookie 数据存放在客户的浏览器上，session 数据放在服务器上。

2、cookie 不是很安全，别人可以分析存放在本地的 COOKIE 并进行 COOKIE 欺骗考虑到安全应当使用 session。

3、session 会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能考虑到减轻服务器性能方面，应当使用 COOKIE。

4、单个 cookie 保存的数据不能超过 4K，很多浏览器都限制一个站点最多保存 20 个 cookie。




