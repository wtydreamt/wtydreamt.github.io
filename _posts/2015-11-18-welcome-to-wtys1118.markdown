---
bg: "railss.jpg"
layout: post
title:  " Linux下配置 Keepalived（心跳检测部署） !"
crawlertitle: "How to use jekyll"
summary: "Keepalived！"
date:   2015-11-16 20:00:08 +0700
categories: posts
tags: '服务器'
author: redVi
---
##### 首先呢，我想先给大家简单介绍一下什么是keepalived：

Keepalived的作用是检测服务器的状态，如果有一台web服务器死机，或工作出现故障，Keepalived将检测到，并将有故障的服务器从系统中剔除，同时使用其他服务器代替该服务器的工作，当服务器工作正常后Keepalived自动将服务器加入到服务器群中，这些工作全部自动完成，不需要人工干涉，需要人工做的只是修复故障的服务器。

1.准备两台服务器

主服务器：192.168.1.111

从服务器：192.168.1.199

虚拟ip:192.168.1.223

两台机器安装

2.安装keepalived需要的依赖包
<pre><code>
yum install openssl-devel
yum install popt-devel
yum install ipvsadm
yum install libnl*	
</code></pre>
3.下载keepalived
<pre><code>
yum install keepalived	
</code></pre>
4.修改主服务器配置文件
<pre><code>
vim /etc/keepalived/keepalived.conf
! Configuration File for keepalived

global_defs {
   notification_email {
     #acassen@firewall.loc没有服务器配置邮箱可将其注释掉
     #failover@firewall.loc
     #sysadmin@firewall.loc
   }
   #notification_email_from Alexandre.Cassen@firewall.loc
   #smtp_server 192.168.200.1
   #smtp_connect_timeout 30
   router_id LVS_DEVEL
}

vrrp_instance VI_1 {
    state MASTER
    interface eno16777736 #绑定虚拟IP的网络接口
    virtual_router_id 51#和slave一样
    priority 100#主机高于slave
    advert_int 1#检测服务器状态间隔时间
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.1.223#虚拟IP地址，可以为多个
    }
}	
</code></pre>
开启服务
<pre><code>
systemctl start keepalived
</code></pre>
5.修改slave配置
<pre><code>
vim /etc/keepalived/keepalived.conf
! Configuration File for keepalived

global_defs {
  #notification_email {
  #  644856452@qq.com
  #}
  #notification_email_from Alexandre.Cassen@firewall.loc
  #smtp_server 127.0.0.1
  #smtp_connect_timeout 30
  router_id LVS_DEVEL
}

vrrp_instance VI_1 {
   state SLAVE
   interface eno16777736
   virtual_router_id 51
   priority 80#低于主服务器100
   advert_int 1
   authentication {
       auth_type PASS
       auth_pass 1111#验证密码，两台机器保持一致
   }

   virtual_ipaddress {
       192.168.1.223
   }
}
</code></pre>
开启服务
<pre><code>
systemctl start keepalived
</code></pre>
1.启动两台机器上的nginx

2.启动两台机器上的keepalived

此时使用命令 ip addr 查看虚拟IP绑定 可以看到主 有，从没有，将主机的keepalived关掉，可以看到vip绑定到了从的上面。

那么如何根据服务某个端口的开与关来进行虚拟Ip的绑定呢？
<pre><code>
Vim /usr/share/doc/keepalived-1.2.13/samples/keepalived.conf.vrrp.localcheck	
</code></pre>
参考提供的例子
<pre><code>
Configuration File for keepalived
vrrp_script chk_http_port {
       script "</dev/tcp/127.0.0.1/80" # connects and exits
       interval 1                      # check every second
       weight -2                       # default prio: -2 if connect fails
}
vrrp_script chk_mysql_port {
       script "</dev/tcp/127.0.0.1/80" # connects and exits
       interval 1                      # check every second
       weight -2                       # default prio: -2 if connect fails
}

    track_script {
       chk_http_port
       chk_mysql_port
}	
</code></pre>

将将以上信息复制到两台服务器的/etc/keepalived/keepalived.conf文件里
变成如下，参考，注意从机不一样，为了讲清楚上面的信息放入的位置。
<pre><code>
global_defs {
   notification_email {
     acassen@firewall.loc
     failover@firewall.loc
     sysadmin@firewall.loc
   }
   notification_email_from Alexandre.Cassen@firewall.loc
   smtp_server 192.168.200.1
   smtp_connect_timeout 30
   router_id LVS_DEVEL
}
vrrp_script chk_http_port {
       script "</dev/tcp/127.0.0.1/80" # connects and exits
       interval 1                      # check every second
       weight -2                       # default prio: -2 if connect fails
}

vrrp_script chk_mysql_port {
       script "</dev/tcp/127.0.0.1/3306" # connects and exits
       interval 1                      # check every second
       weight -2                   # default prio: -2 if connect fails
}
vrrp_instance VI_1 {
    state MASTER
    interface eno16777736
    virtual_router_id 51
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass aaa
    }
    virtual_ipaddress {
        192.168.1.229
    }
    track_script {
       chk_http_port
	chk_mysql_port
    }
}
</code></pre>

此时再重启两台机器的keepalived服务

Systemctl restart keepalived

此时分别关闭主机80和3306端口服务

可以发现虚拟Ip绑定到了从机上。