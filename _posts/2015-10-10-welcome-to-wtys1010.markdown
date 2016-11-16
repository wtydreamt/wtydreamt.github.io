---
bg: "railss.jpg"
layout: post
title:  " centos 7.0 lamp环境搭建 !"
crawlertitle: "How to use jekyll"
summary: "lamp！"
date:   2015-10-10 21:00:08 +0700
categories: posts
tags: '服务器'
author: redVi
---
##### centos7.2 + apache2.4.23 + mysql5.7.14 + php7.0.9 + phpMyAdmin4.6.3

准备

    1、创建我存放源码包的文件夹

        mkdir -p /projects/lamp

    2、查看gcc是否安装

        gcc -v 

        提示：如未安装，则进行yum安装

            yum install -y gcc gcc-c++

    3、安装vim编辑器

        yum install -y vim

安装apache

    （一）安装apr

        1、切换到源码目录

            cd /projects/lamp

        2、下载apr包

            wget http://apache.fayea.com/apr/apr-1.5.2.tar.gz

        3、安装apr包需要的扩展

            yum install -y autoconf libtool

        4、解压、配置、编译、安装

            tar -xzvf apr-1.5.2.tar.gz

            cd apr-1.5.2


            ./buildconf

            ./configure --prefix=/usr/local/apr


            make

            make install


            问题1：如果./configure遇到‘executing libtool commands rm: cannot remove 'libtoolT': No such file ordirectory’

                解决：

                    打开configure文件

                        vim configure 

                    找到$RM "$cfgfile"所在行，然后添加#号注释掉此行保存并退出
                    

    （二）安装apr-util

        1、切换到源码目录

            cd /projects/lamp/

        2、下载apr-until包

            wget http://apache.fayea.com/apr/apr-util-1.5.4.tar.gz

        3、解压、配置、编译、安装

            tar -xzvfapr-util-1.5.4.tar.gz

            cd apr-util-1.5.4

            ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr

            make

            make install


     （三）安装pcre包

        1、切换到源码目录

            cd /projects/lamp/

        2、下载pcre包

            wget http://jaist.dl.sourceforge.NET/project/pcre/pcre/8.39/pcre-8.39.zip

        3、安装解压工具unzip

            yum install -y unzip

        4、解压、配置、编译、安装

            unzip pcre-8.39.tar.gz

            cd pcre-8.39

            ./configure --prefix=/usr/local/pcre

            make

            make install


    （四）安装apache

        1、切换到源码目录

            cd /projects/lamp/

        2、下载apache

            wget http://mirrors.cnnic.cn/apache//httpd/httpd-2.4.23.tar.gz

        3、解压、配置、编译、安装

            tar -xzvfhttpd-2.4.23.tar.gz

            cd httpd-2.4.23

             

            ./configure \

            --prefix=/usr/local/apache2 \

            --with-apr=/usr/local/apr \

            --with-apr-util=/usr/local/apr-util/bin/apu-1-config \

            --with-pcre=/usr/local/pcre \

            --enable-so \

            --enable-rewrite

             

            make

            make install

        4、配置apache

            切换到配置文件目录

                cd /usr/local/apache2/conf

            vim打开配置文件

                vim httpd.conf

             去掉ServerName前的注释，修改为localhost:80

        5、启动apache服务

            /usr/local/apache2/bin/apachectlstart

        6、查看apache是否安装成功

            curl localhost

            提示：如返回It works页面，则安装成功


安装mysql

    提示：因为centos7.2默认安装了mariadb-libs，所以先要卸载掉

        查看是否安装mariadb

            rpm -qa | grep mariadb

        卸载mariadb(注意检查依赖，判断是否可以强制删除)

            rpm -e --nodeps mariadb-libs-5.5.44-2.el7.centos.x86_64

    1、切换到源码目录

        cd /projects/lamp

     2、下载所需的rpm包

        wget http://cdn.MySQL.com//Downloads/MySQL-5.7/mysql-community-common-5.7.14-1.el7.x86_64.rpm

        wget http://cdn.mysql.com//Downloads/MySQL-5.7/mysql-community-libs-5.7.14-1.el7.x86_64.rpm

        wget http://cdn.mysql.com//Downloads/MySQL-5.7/mysql-community-client-5.7.14-1.el7.x86_64.rpm

        wget http://cdn.mysql.com//Downloads/MySQL-5.7/mysql-community-server-5.7.14-1.el7.x86_64.rpm

     3、安装需要的扩展库

         yum install -y libaio    //安装server时需要

    4、安装

        rpm -ivh mysql-community-common-5.7.14-1.el7.x86_64.rpm --nosignature

        rpm -ivh mysql-community-libs-5.7.14-1.el7.x86_64.rpm --nosignature

        rpm -ivh mysql-community-client-5.7.14-1.el7.x86_64.rpm --nosignature

        rpm -ivh mysql-community-server-5.7.14-1.el7.x86_64.rpm --nosignature

        提示：加上--nosignature是为了防止报签名错误

    5、初始化

        切换到mysql的bin目录

            cd /usr/bin

        初始化mysql

            mysqld --initialize-insecure --user=mysql     

            提示：-insecure设置root密码为空

        修改mysql文件所有者（报找不到mysql.sock错误时需修改）

            chown mysql:mysql -R /var/lib/mysql  

        启动mysqld

            systemctl start mysqld

        添加root用户密码

            mysqladmin -u root password 密码

        登录mysql

            mysql -u root -p

    退出

        quit


安装php

    1、切换到源码目录

        cd /projects/lamp

    2、下载源码包

        wget http://cn2.PHP.net/distributions/php-7.0.9.tar.gz

    3、安装所需的扩展库

        yum -y install libxml2 libxml2-devel openssl openssl-devel curl-devel libjpeg-devel libpng-devel freetype-devel libmcrypt-devel

    4、解压、配置、编译、安装

        tar -zvxf php-7.0.9.tar.gz

        cd php-7.0.9

         

        ./configure \

        --prefix=/usr/local/php7 \

        --with-apxs2=/usr/local/apache2/bin/apxs \

        --with-mysqli=shared,mysqlnd \

        --with-pdo-mysql=shared,mysqlnd

        提示：此处只配置了这几项，其他扩展可以通过添加动态扩展方式打开（见第7步）


        make&& make install

    5、配置php.ini

        添加配置文件

            cp /projects/lamp/php-7.0.9/php.ini-production /usr/local/php7/lib/php.ini

            提示：因我configure时未指定配置文件位置，系统默认位置在/usr/local/php7/lib下面

        让apache支持php

            编辑httpd.conf

                vim /usr/local/apache2/conf/httpd.conf

            找到DirectoryIndex，在index.html后添加

                 index.php

            找到AddType，在之后添加一行

                AddType application/x-httpd-php .php

        重启apache

            /usr/local/apache2/bin/apachectl -k restart        

    6、测试

        切换到apache默认的文档根目录

            cd /usr/local/apache2/htdocs/

        新增一个index.php

            vim index.php

        写入内容：

            <?php

                     phpinfo();

         

        保存并退出

        删除index.html

            rm -f index.html

        测试能否访问到index.php

            curl localhost

            提示：如打印出phpinfo页面则安装成功

     

    7、安装动态扩展（以下两个扩展在用phpMyAdmin访问数据库时会用到）

        例1、安装php的gettext扩展

            切换到php源码包的gettext扩展目录

                cd /projects/lamp/php-7.0.9/ext/gettext/

            执行phpize

                /usr/local/php7/bin/phpize

            配置、编译、安装

                ./configure --with-php-config=/usr/local/php7/bin/php-config --with-gettext

                make&& make install

            修改php.ini添加扩展

                打开php.ini

                    vim /usr/local/php7/lib/php.ini

                文件最后添加一行

                    extension=/usr/local/php7/lib/php/extensions/no-debug-zts-20151012/gettext.so

                    提示：如未改动过扩展默认目录配置，可直接写成extension=gettext.so，其他扩展同理。

            重启apache使配置生效

                /usr/local/apache2/bin/apachectl -k restart

        例2、安装php的mbstring扩展

            切换到php源码包的mbstring扩展目录

                cd /projects/lamp/php-7.0.9/ext/mbstring/

            执行phpize

                /usr/local/php7/bin/phpize

            配置、编译、安装

                ./configure --with-php-config=/usr/local/php7/bin/php-config --enable-mbstring

                make&& make install

            修改php.ini添加扩展

                打开php.ini

                    vim /usr/local/php7/lib/php.ini

                文件最后添加一行

                    extension=mbstring.so

            重启apache使配置生效

                 /usr/local/apache2/bin/apachectl -k restart


安装phpMyAdmin

    1、切换到源码目录

        cd /projects/lamp/

    2、下载

        wget https://files.phpmyadmin.Net/phpMyAdmin/4.6.3/phpMyAdmin-4.6.3-all-languages.tar.gz

    3、解压

        tar -zvxf phpMyAdmin-4.6.3-all-languages.tar.gz

    4、复制解压后文件到apache网站根目录pma文件夹下

        cp -fr phpMyAdmin-4.6.3-all-languages /usr/local/apache2/htdocs/pma

    5、浏览器输入IP地址/pma访问


    问题1：访问时如提示缺少mysqli扩展，添加mysqli扩展

        解决：动态添加php的mysqli扩展

            切换到php源码包的mysqli扩展目录

                cd /projects/lamp/php-7.0.9/ext/mysqli/

            执行phpize

                /usr/local/php7/bin/phpize

            配置

                ./configure --with-php-config=/usr/local/php7/bin/php-config --with-mysqli=mysqlnd

            编译安装

                make && make install

            修改php.ini添加扩展

                打开php.ini

                    vim /usr/local/php7/lib/php.ini

                文件最后添加一行

                    extension=mbstring.so

            重启apache

                /usr/local/apache2/bin/apachectl -k restart


    问题2：提示‘mysqli_real_connect():(HY000/2002): 没有那个文件或目录’错误

        解决：

            修改php.ini

                打开phi.ini

                    vim /usr/local/php7/lib/php.ini

                修改pdo_mysql.default_socket=/var/lib/mysql/mysql.sock（mysql.sock地址）

                修改mysqli.default_socket =/var/lib/mysql/mysql.sock

            重启apache

                /usr/local/apache2/bin/apachectl -k restart


后续

    1、将apache命令加入到环境变量中

        打开profile文件

            vim /etc/profile

        文件最后添加一行

            PATH=/usr/local/apache2/bin:$PATH

        使配置生效

            source /etc/profile

    2、设置httpd开机自启

        切换到系统service目录

            cd /lib/systemd/system

        新建一个httpd.service文件

            vim httpd.service

        写入如下数据：

            [Unit]

            Description=ApacheServer

            After=network.target


            [Install]

            WantedBy=multi-user.target


            [Service]

            Type=forking

            ExecStart=/usr/local/apache2/bin/apachectl-k start

            ExecReload=/usr/local/apache2/bin/apachectl-k restart

            ExecStop=/usr/local/apache2/bin/apachectl-k stop

            PrivateTmp=true

        保存并退出

        设置httpd.service开机自启

            systemctl enable httpd.service

        重启电脑

            shutdown -r now

        查看是否自启

            ps -ef | grep httpd        