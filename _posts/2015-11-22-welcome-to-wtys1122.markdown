---
bg: "railss.jpg"
layout: post
title:  " Linux下 ssh 扩展的应用 !"
crawlertitle: "How to use jekyll"
summary: "Keepalived！"
date:   2015-11-22 20:00:08 +0700
categories: posts
tags: '服务器'
author: redVi
---
##### ssh操作服务器

首先安装ssh2扩展

下载地址
 wget http://www.libssh2.org/download/libssh2-1.4.2.tar.gz

 http://pecl.php.net/get/ssh2-0.13.tgz Version(0.13)

先安装 libssh2 在安装 SS2
<pre><code>
# tar -zxvf libssh2-1.4.2.tar.gz
# cd libssh2-1.4.2
# ./configure --prefix=/usr/local/libssh2
# make && make install
</code></pre>
以上为安装libssh2，这里需要记住libssh2的安装目录，因为在安装ssh2的时候还会用到。

SSH安装
<pre><code>
# tar -zxvf ssh2-0.12.tgz
# cd ssh2-0.12
# phpize
# ./configure --prefix=/usr/local/ssh2 --with-ssh2=/usr/local/libssh2 --with-php-config=/usr/local/php/bin/php-config
# make && make install	
</code></pre>
修改php.ini文件加入

extension=ssh2.so

接下来用代码实现ssh2连接操作服务器 创建info.php文件
<pre><code>
   header("content-type:text/html; charset=utf-8");  
    $host='192.168.1.121';//被控制的linux的ip  
      
    $user='root';//用户名  
      
    $passwd='root';//密码  
      
    // 链接远程服务器  
      
    $connection = ssh2_connect($host, 22);  
      
    if (!$connection) die('connection to '.$host.':22 failed');  
      
    echo 'connection OK<br/>';  
      
    // 获取验证方式并打印  
      
    $auth_methods = ssh2_auth_none($connection, $user);  
      
    print_r( $auth_methods);  
      
    if (in_array('password', $auth_methods ))  
    {  
      
        // 通过password方式登录远程服务器  
      
        if (ssh2_auth_password($connection, $user, $passwd))  
      
        {  
          echo $user.' login OK<br/>';
      		if(empty($_GET['status']))
      		{
	          $pdd="systemctl status nginx";
            $stream = ssh2_exec($connection, $pdd); // 执行php
      		}
      		else
      		{
 				     if($_GET['status']==1)
            {
              $pdd="systemctl stop nginx";
              ssh2_exec($connection, $pdd); // 执行php
              $stream = ssh2_exec($connection, "systemctl status nginx"); // 执行php
            }
            if($_GET['status']==2)
            {
              $pdd="systemctl start nginx";
              ssh2_exec($connection, $pdd); // 执行php
              $stream = ssh2_exec($connection, "systemctl status nginx"); // 执行php
            }
            if($_GET['status']==3)
            {
              $pdd="systemctl restart nginx";
              ssh2_exec($connection, $pdd); // 执行php
              $stream = ssh2_exec($connection, "systemctl status nginx"); // 执行php
            }            
      		}
              
      
              
      
            stream_set_blocking($stream, true); // 获取执行pwd后的内容  
      
          if ($stream === FALSE) die("pwd failed");  
      		$statuss=stream_get_contents($stream);
          //print_r($statuss);
            if(strstr($statuss,"running"))
            {
            	echo "<a href='info.php?status=1'>停止</a>";
              echo "<a href='info.php?status=3'>重启</a>";
            }
            else
            {
            	echo "<a href='info.php?status=2'>开启</a>";
            }
      
        }  
      
        else  
      
        {  
      
            die( $user.' login Failed<br/>');  
      
        }  
      
    }  	
</code></pre>