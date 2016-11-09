---
bg: "railss.jpg"
layout: post
title:  "js 调用浏览器打印页面!"
crawlertitle: "How to use jekyll"
summary: "菜鸟的一天……！"
date:   2016-09-05 15:00:08 +0700
categories: posts
tags: 'Javascript'
author: redVi
---
##### 简单介绍 js 打印功能,话不多说下面奉上代码。

<pre><code> 
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta http-equiv="Content-Type" content="text/html; charset=utf-8" /&gt;
&lt;title&gt;haorooms js局部打印案例&lt;/title&gt;
&lt;script type="text/javascript"&gt; 
    function doPrint() {    
        bdhtml=window.document.body.innerHTML;    
        sprnstr="&lt;!--startprint--&gt;"; //标记开始打印地方    
        eprnstr="&lt;!--endprint--&gt;";   //标记打印结束的地方
        prnhtml=bdhtml.substr(bdhtml.indexOf(sprnstr)+17);    
        prnhtml=prnhtml.substring(0,prnhtml.indexOf(eprnstr));    
        window.document.body.innerHTML=prnhtml; 
        window.print();    
	}
&lt;/script&gt;
&lt;/head&gt;

&lt;body&gt;
&lt;!--startprint--&gt;
&lt;h1&gt;此处是要打印的页面(●'◡'●)&lt;/h1&gt;
&lt;!--endprint-->
&lt;button type="button" onclick="doPrint()"&gt;打印&lt;/button&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>

#####startprint 标记打印的开始处 endprint 标记打印的结束




