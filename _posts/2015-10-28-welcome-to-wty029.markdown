---
bg: "railss.jpg"
layout: post
title:  "PDO msyqli 操作!"
crawlertitle: "How to use jekyll"
summary: "初识php！"
date:   2015-10-29 12:00:47 +0700
categories: posts
tags: 'msyql'
author: redVi
---
##### PHP高级特性八之mysqli函数库的使用

1.什么是mysqli

PHP-MySQL 函数库是 PHP 操作 MySQL 资料库最原始的扩展库，PHP-MySQLi 的 i 代表 Improvement ，相当于前者的增强版，也包含了相对进阶的功能，另外本身也增加了安全性，比如可以大幅度减少 SQL 注入等问题的发生。

2. mysql与mysqli的概念相关

（1）mysql与mysqli都是php方面的函数集，与mysql数据库关联不大。

（2）在php5版本之前，一般是用php的mysql函数去驱动mysql数据库的，比如mysql_query()的函数，属于面向过程

（3）在php5版本以后，增加了mysqli的函数功能，某种意义上讲，它是mysql系统函数的增强版，更稳定更高效更安全，与mysql_query()对应的有mysqli_query()，属于面向对象，用对象的方式操作驱动mysql数据库

3. mysql与mysqli的主要区别

（1）mysql是非持继连接函数，mysql每次链接都会打开一个连接的进程，所以mysqli耗费资源少一些。

（2）mysqli是永远连接函数，mysqli多次运行mysqli将使用同一连接进程,从而减少了服务器的开销。mysqli封装了诸如事务等一些高级操作，同时封装了DB操作过程中的很多可用的方法。


#####  连接数据库并获取相关信息
<pre><code>
	 $mysqli=@new mysqli("localhost", "root", "****", "database");
    //如果连接错误
    if(mysqli_connect_errno()){
        echo "连接数据库失败：".mysqli_connect_error();
        $mysqli=null;
        exit;
    }
    //获取当前字符集
    echo $mysqli->character_set_name()."<br>";
    //获取客户端信息
    echo $mysqli->get_client_info()."<br>";
    //获取mysql主机信息
    echo $mysqli->host_info."<br>";
    //获取服务器信息
    echo $mysqli->server_info."<br>";
    //获取服务器版本
    echo $mysqli->server_version."<br>";
    //关闭数据库连接
    $mysqli->close();
</code></pre>
msyqli 查询
<pre><code>
//连接数据库  第一个参数、 主机ip 参数二、用户名 参数三、密码 参数四、数据库名
$connect = mysqli_connect('localhost','root','root','qq') or die('Unale to connect');
//设置字符编码
mysqli_query($connect,'set names utf8');
$sql = "SELECT * FROM user3
WHERE username='wty';
$result = mysqli_query($connect,$sql);
 while($res=mysqli_fetch_row($result))
 	{
 		print_r($res);	
 	}	
</code></pre>
插入数据
<pre><code>
$mysqli=@new mysqli("localhost", "root", "****", "database");
    //如果连接错误
    if(mysqli_connect_errno()){
        echo "连接数据库失败：".mysqli_connect_error();
        $mysqli=null;
        exit;
    }
    //插入数据
    $sql="insert into designer(name,phone) values('hello','18352682923')";
    //执行插入语句
    $result=$mysqli->query($sql);
    //如果执行错误
    if(!$result){
        echo "SQL语句有误<br>";
        echo "ERROR:".$mysqli->errno."|".$mysqli->error;
        exit;    
    }
    //如果插入成功，则返回影响的行数
    echo $mysqli->affected_rows;
    //关闭数据库连接
    $mysqli->close();		
</code></pre> 
修改内容
<pre><code>
$mysqli=@new mysqli("localhost", "root", "****", "database");
    //如果连接错误
    if(mysqli_connect_errno()){
        echo "连接数据库失败：".mysqli_connect_error();
        $mysqli=null;
        exit;
    }
    //插入数据
    $sql="update designer set name = 'hello' where id = 10062";
    //执行插入语句
    $result=$mysqli->query($sql);
    //如果执行错误
    if(!$result){
        echo "SQL语句有误<br>";
        echo "ERROR:".$mysqli->errno."|".$mysqli->error;
        exit;    
    }
    //如果插入成功，则返回影响的行数
    echo $mysqli->affected_rows;
    //关闭数据库连接
    $mysqli->close();	
</code></pre>
预处理语句
<pre><code>
$mysqli = @new mysqli("localhost", "root", "", "design");
    //如果连接错误
    if(mysqli_connect_errno()){
        echo "连接数据库失败：".mysqli_connect_error();
        $mysqli=null;
        exit;
    }
    //准备好一条语句放到服务器中，插入语句
    $sql = "insert into designer(name, email) values(?, ?)";
    //生成预处理语句
    $stmt = $mysqli->prepare($sql);
    //给占位符号每个?号传值（绑定参数） i  d  s  b，第一个参数为格式化字符，ss代表两个字符串，d代表数字
    $stmt->bind_param("ss", $name, $email);
    //为变量赋值
    $name = "Mike";
    $email = "mike@live.cn";
    //执行
    $stmt->execute();
    //为变量赋值
    $name = "Larry";
    $email = "larry@live.cn";
    //执行
    $stmt->execute();
    //最后输出
    echo "最后ID".$stmt->insert_id."<br>";
    echo "影响了".$stmt->affected_rows."行<br>";
    //关闭数据库连接
    $mysqli->close();	
		
</code></pre>

`上文摘自：http://cuiqingcai.com/1534.html`

####为什么要使用PDO？
PDO拥有更好的编程接口，你可以使用它写出更加简洁，高效，安全的代码。PDO还为不同的SQL数据库提供了不同的驱动，方便你使用新的数据库而不用再学习不同的编程接口。与拼接SQL语句构造查询语句不同，绑定参数可以简洁方便的构造出更加安全的查询语句，使用绑定参数的方法在 多次相似语句查询（仅仅某个参数不同）中也可以提高不少性能。PDO在错误处理方面也提供了多种方法。mysql_*函数缺乏一致的处理，与PDO的异常模式相比，或者说没有处理异常，使用PDO，你可以得到一致的错误处理，这将节省您大量的时间来跟踪问题。

pdo操作代码来自：http://www.runoob.com/php/php-pdo.html
<pre><code>
	$dbms='mysql';     //数据库类型
$host='localhost'; //数据库主机名
$dbName='test';    //使用的数据库
$user='root';      //数据库连接用户名
$pass='';          //对应的密码
$dsn="$dbms:host=$host;dbname=$dbName";


try {
    $dbh = new PDO($dsn, $user, $pass); //初始化一个PDO对象
    echo "连接成功<br/>";
    /*你还可以进行一次搜索操作
    foreach ($dbh->query('SELECT * from FOO') as $row) {
        print_r($row); //你可以用 echo($GLOBAL); 来看到这些值
    }
    */
    $dbh = null;
} catch (PDOException $e) {
    die ("Error!: " . $e->getMessage() . "<br/>");
}
//默认这个不是长连接，如果需要数据库长连接，需要最后加一个参数：array(PDO::ATTR_PERSISTENT => true) 变成这样：
$db = new PDO($dsn, $user, $pass, array(PDO::ATTR_PERSISTENT => true));
</code></pre>
其他教程可以点击这里查看[点我](http://rmingwang.com/pdo-tutorial-for-mysql-developers.html)

