---
layout: post
title:  "jekyll github 个人博客搭建!"
crawlertitle: "How to use jekyll"
summary: "Jekyll default page"
date:   2016-06-28 23:09:47 +0700
categories: posts
tags: 'jekyll'
author: redVi
---
搭建一个jekyll github


####搭建一个jekyll github 博客首先在github上创建一个`username.github.io`的仓库熟悉github的程序猿可以直接参考`https://pages.github.com/`这个地址来搭建`username.github.io`仓库如果不熟悉的可以先看`http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/`这个教程在开始上面的步骤。基本准备工作做完后就开始安装jekyll这里我是在windows下安装的安装中我们需要注意的是版本
***
<h3>安装Ruby</h3>
Jekyll是一款静态网站生成工具，允许用户使用HTML、Markdown或Textile通过模块的方式建立所需网站，然后通过模板引擎Liquid（Liquid Templating Engine）来运行或者生成对应的静态网站文件.
在GitHub上使用较多，通过GitHub搭建自己的博客一般来说就是使用Jekyll；因为GitHub的渲染引擎默认为Jekyll。<br>
Jekyll是一款基于Ruby的插件，安装Ruby是必须的
<br>
下载ruby：[点击下载](https://www.ruby-lang.org/zh_cn/downloads/)
<br>安装时选中“Add Ruby executables to your PATH”这样将自动完成环境变量的配置。打开cmd
<pre><code>
#=>在cmd中输入 
ruby -v
</code></pre>
如果显示ruby的版本号则安装成功

***
<h3>安装 jekyll</h3>
<pre><code>
#=>在cmd中输入 
gem install jekyll
</code></pre>

####在这里可能会出现这样的错误
<pre><code>ERROR: Could not find a valid gem ‘jekyll’ (>= 0), here is why:
Unable to download data from https://rubygems.org/ - SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://api.rubygems.org/latest_spece.4.8.gz)</code></pre>

####解决办法
<pre><code>
$ gem --remove https://rubygems.org/ #移除原有镜像
$ gem sources --add http://gems.ruby-china.org/ #添加新镜像 
$ gem sources -l #查看镜像
https://gems.ruby-china.org
# 确保只有 gems.ruby-china.org
</code></pre>

####接着执行
<pre><code>
gem install jekyll -v '3.0'
</code></pre>

#####jekyll版本过高可能会出错，如果已经安装了jekyll过高的版本可以用下面的命令删除后重新安装
<pre><code>
gem uninstall jekyll -a
</code></pre>

***

####上面准备工作做完后就可以开始做我们的博客了首先创建一个blog
打开命令行在任意位置
<pre><code>
jekyll new blog
</code></pre>
这个时候就会生成一个blog的文件夹命令行进入到blog文件夹中然后执行命令
<pre><code>
jekyll serve
</code></pre>
<br>
最后输出Now browse to http://localhost:4000访问`http://localhost:4000`这样就ok了。如果在本地运行后没问题那你就可以把它推到git上了就是上面第一步骤中的仓库里。接着就可以根据自己的爱好来搭建博客了如果还有什么疑问可以点击这里来看看:[点我](http://wiki.jikexueyuan.com/project/jekyll/)
