---
bg: "railss.jpg"
layout: post
title:  "session 入redis!"
crawlertitle: "How to use jekyll"
summary: "session 入redis！"
date:   2015-11-16 20:00:08 +0700
categories: posts
tags: '服务器'
author: redVi
---
##### redis

<pre><code>

ini_set("session.save_handler", "redis");

ini_set("session.save_path", "tcp://localhost:6379");

session_start();

header("Content-type:text/html;charset=utf-8");

$_SESSION['view'] = 'zhangsan';

echo $_SESSION['view'];
   //连接本地的 Redis 服务
   $redis = new Redis();
   $redis->connect('127.0.0.1', 6379);
   $arr=$redis->keys("*");
   print_r($arr);

</code></pre>

session 入memcache
<pre><code>
ini_set("session.save_handler", "memcache");

ini_set("session.save_path", "tcp://localhost:11211");

session_start();

header("Content-type:text/html;charset=utf-8");

$_SESSION['view'] = 'zhangsan';

echo $_SESSION['view'];
</code></pre>