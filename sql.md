# 数据库第一天

## 数据库MySQL

```php
数据库的概述
	1.存储数据的一个仓库，专门管理数据的一个系统
	2.常用的数据库IBM的DB2，Oracle,SQL Lite，SQL servce，MySQL，Access等
	
数据库在WEB开发中的地位
	动态网站都是围绕着数据库进行的。基于数据库开发的网站就是动态网站
```

## 数值类型的属性

```php
名称			取值范围							说明

tinyint		-128 ~ +128 或 0~255					非常小的整数

int			-2的31次方到2的31次方-1 或 2的64次方	 标准的整数（值）

bigint		-2的63次方到2的63次方-1				大整数
```

## 字符串类型的属性

```php
名称				说明			取值范围

char(m)			定长字符串		   m	最大255（长度）

varchar(m)		可变字符串		   m	最大255（长度）

enum('男','女')	枚举类型		括号里面的成员
```

## 日期类型的属性

```php
date	YYYY-MM-DD	1000-01-01 ~ 999-12-31

time	HH:MM:SS	-838:59:59 ~ +838:59:59

year	YYYY		1901 ~ 2155
```

## 特殊属性null

```php
null

1.意味着"没有值"或"未知值"
2.可以用来测试某个值是否为null
3.对null值进行运算，结果还是null
4.不能对null值进行算数运算
5.0或null都代表假，其余的值都意味着真
```

## 数据字段属性

```php
unsigned	只能用于设置数值类型，不允许出现负数，把取值范围扩大了一倍

auto_increment	用于设置字段的自动增长属性,每增加一条数据，该字段的值会自动加1

default	可以通过此属性来指定一个默认值，如果没有在此列添加值，那么默认添加此值

primary key	主键索引，主要作用是确定数据表里一条特定的数据记录位置
```



## 数据库优化

```php
1.选择合适的数据类型

2.将字符段设置notnull
```



## 创建数据库

```php
1.create database	自定义库名(create databas	pe lzx;)

2.使用数据库	use 库名	(use lzx;)

3.创建数据表	create table 自定义表名(字段1，字段2，字段3....)

create table student(
id			int  unsigned not null auto_increment primary key,
name		varchar(20)	not null default "",
age			tinyint unsigned not null default 0,
time		int(10) not null default 0,
sex			enum('男','女') not null default "男",
card		varchar(18) not null default "",
tel			bigint(11) not null default 0
)engine=myisam default charset=utf8;

只要保持编辑器，代码，浏览器，数据库编码一致，就不会出现乱码

create table [if not exists] student(
id int unsigned not null auto_increment primary key, （固定）
name varchar(20) not null default "",
age tinyint unsigned not null default 0,
time int(10) not null default 0,
sex enum('男','女') not null default "男",
card varchar(18) not null default "",
tel bigint(11) not null default 0
)engine=myisam default charset=utf8; （固定）
```

## 删除表的语句

```php
DROP TABLE [if exists](可选) 表名称;
```

## 修改表

```php
ALTER TABLE 表名 操作(ACTION)；

我们可以对表进行修改字段，添加字段，删除字段，更改表名，更改字段名，更改auto_increment属性的初始值

修改字段:
	modify和change关键字
	ALTER TABLE student	CHANGE name(旧名字) username(新) varchar(10) not null defualt "";
	
	ALTER TABLE student MODIFY `name` varchar(15) mot null default "";
	
change可以修改字段名和属性，modify只能修改字段属性不能修改字段名
```

## 查看表

```php
DESC 表名;
SHOW DATABASES;	查看所有的库
SHOW TABLES;	查看已选择的库里面的表
```

## 添加字段

```php
使用add关键字
ALTER TABLE student ADD height varchar(5) not null default "";

ALTER TABLE student ADD height varchar(5) not null default "" [AFTER 字段名];
```

## 删除字段

```php
使用drop关键字
ALTER TABLE 表名 DROP 字段名;

ALTER TABLE student drop height;
```

## 更改表名

```php
使用rename关键字
ALTER TABLE 旧表名 RENAME AS 新表名;

ALTER TABLE student RENAME AS nimama;
```



## 数据库语言(增删改查以及高级)

```php
1.数据定义语言(DDL)
	Create语言: 可以创建数据库和数据库的一些对象
	Drop语句:可以删除数据表，索引，条件约束以及数据表的权限
	Alter语句:修改数据表及属性
	
2.数据操作语言(DML)
	Insert语句: 向数据表插入数据
	Delete语句: 删除数据表中的一条或多条数据
	Update语句: 用于修改已存在表中的数据
	
3.数据查询语言(DQL)
	Select语句: 查询数据
	
4.数据控制语言(DCL)
	Grant: 允许对象的创建者给某些用户或用户组中的所有用户某些特定的全选
	Revoke: 可以废除某用户或所有用户的访问权限
	SET TRANSACTION: 设置当前事务状态
	COMMIT: 提交事务
	ROLLBACK: 回滚
```

## 新增数据

```php
Insert语句: 向数据表插入数据
```

## 作业

```php
在word文档里面，写创建库和文章表的语句。
表的字段：
id,title(标题),mapping（简介）,content(内容),addTime,image,click
要求合理安排字段属性。
添加字段pid语句，
修改pid字段属性语句，
删除pid字段语句
修改表名称语句.


create database art;
Use art;
Create table arts(
id int unsigned not null auto_increment primary key, 
Title varchar(20) not null default '',
Mapping text not null default '',
Content text not null default '',
addTime int(10) not null default 0,
Image varchar(10) not null default '',
Click int unsigned not null default 0
)engine=myisam default charset=utf8;


添加字段
alter table arts add about varchar(50) not null default '' after addTime;

删除字段
alter table arts drop about;

修改属性
alter table arts change about aboutss varchar(20) not null default "";

修改表名称
alter table arts rename as article;
```

# 数据库第二天

## SQL语句的设计

```php
通用于所有关系型数据库
	
	根据空值确定查询内容
		null代表无值，和0 '0' '' "" 都不相同
		
	ORDER BY 对查询结果排序
	ASC正序 DESC倒序
	select * from students where subject = '语文' order by score asc; 默认为asc正序 ORDER BY后面要加上字段名，根据什么来排序
	
	LIMIT限定结果的行数
		第一个数字为数据的下标，第二个数字为显示数据的个数
		select * from student limit 0,3;
	
	使用统计函数
		count() 计数
		sum() 求和
		avg() 平均数
		max() 最大数
		min() 最小数
		select sum(score) as add1 from students where subject = "语文";
	
create table students(
id int unsigned auto_increment primary key not null,
name varchar(20) not null default "",
score tinyint unsigned not null default 0,
subject varchar(20) not null default ""
)engine=myisam default charset=utf8;

insert into students (name,score,subject) values ('张三','90','语文'),('李四','60','数学'),('李四','72','语文');

select * from students where subject = '语文';

select * from students where subject = '语文' order by score asc;
```

## 用户名唯一属性

```php
unique
```



## 修改自增属性的值

```php
ALTER TABLE 表名 AUTO_INCREMENT = 7;
```

## INSERT向数据表添加数据

```php
INSERT INTO 表名 (字段名1，字段名2，...) VALUE ('值1','值2',...)

INSERT INTO 表名 (字段名1，字段名2，...) VALUES ('值1','值2',...),('值1','值2',...),('值1','值2',...).....
```

## UPDATE更新数据表中已存在的数据

```php
UPDATE 表名 SET 字段名1=值1[,字段名2=值2]...;

UPDATE student set age=20,name="严作";
```

## DELETE删除数据表中不需要的数据

```php
DELATE FROM 表名 [WHERE 条件1]：
```

## SELECT进行查询数据

```php
选择特定的字段
	SELECT id,name,tel FROM 表名;
	SELECT * FROM 表名; (所有数据)
		
使用关键字DISTINCT去重
	select distinct tel from student;
		
在SELECT使用表达式
	SELECT version();
	SELECT mad5();
```

## 使用WHERE子句按条件查询

```php
操作符:

and或& or或|| not或! > < >= <=
is null 和 is not null

select id from people WHERE username = 'admin' or telnum = 'admin' AND pwd = '111';   people为表 username为选择的库

between 在什么和什么之间
not between
select * from student where age between 10 and 20;

like '%xx%'
not like
select * from student where age like "%10%";
			
in 在()列表里面;
```

## 练习（修改id）

```php
alter table student auto_increment = 1;

insert into student (name,age,time,sex,card,tel) value ('姚杰',19,'2018年9月8号','男',429006199003018712,'1881225612');
```


# 数据库第三天（PHP操作MySQL）

## 存储文章的数据

```php
text类型
```

## PHP操作

```php
1.连接MySQL数据库服务器
2.判断是否连接成功
3.选择数据库
4.设置字符集（防止乱码）
5.准备SQL语句
6.发送SQL语句到MySQL的服务器上

```

```php
mysql_connect("127.0.0.1","root","root"); 面向过程
mysqli_connect()	面向对象

mysql_errno();	判断是否连接成功 成功返回0
mysql_error();	返回错误信息

mysql_select_db(库名)	选择数据库

mysql_set_charset()	设置字符集 保持编辑器，浏览器，数据库，html meta编码一致

写好sql语句赋值给变量	准备SQL语句

mysql_query($sql)	发送sql语句到服务器
1.返回影响行数 增删改
2.返回资源类型	查询操作

处理结果集
	1.如果执行的是删改(DML) 这时mysql_query()返回的类型，需要通过mysql_affected_rows()获取影响行数
	如果通过查询语句，mysql_query()返回的是资源类型
	
	mysql_fetch_array() 返回索引和关联的混合数组
	mysql_fetch_assoc() 返回关联数组 （常用）
	mysql_fetch_row() 返回索引数组 （常用 echo下标）
	mysql_fetch_ojbect()	返回一个对象
	读取资源类型里面的一条数据，并且指针往下移动一位 使用while循环 配合list()使用
list函数是用来的得到索引数组里面的值
```

## 案例

```php
<?php
    include_once ("./connect.php");
    $sql =  "select * from student";

    $result = mysql_query($sql);

    while(list($id,$name,$age,$time) = mysql_fetch_row($result)) {
        echo "id:".$id."----"."name:".$name."----"."age:".$age."----"."time:".$time;
        echo "<br />";
    }
?>
```

## 释放结果集资源，关闭数据库连接

```php
mysql_free_result()	释放

mysql_close()	关闭
```

## 百度登录注册实战

```php
ajax

<?php
//连接数据库
    include_once("./connect.php");

//获取到用户名
    $username = $_POST['username'];

//拼接数据库
    $sql = "SELECT id FROM people WHERE username = '{$username}'";

//执行 返回资源类型
    $res = mysql_query($sql);

//传入关联数组
    $arr = mysql_fetch_assoc($res);

    if($arr['id']) {
        echo $arr['id'];
    }else {
        echo 0;
    }
?>


<?php
    include_once("connect.php");

    $tel = $_POST['telnum'];

    $sql = "SELECT id FROM people WHERE telnum = '{$tel}'";

    $res = mysql_query($sql);

    $arr = mysql_fetch_assoc($res);

    if($arr['id']) {
        echo $arr['id'];
    }else {
        echo 0;
    }
?>
```

```php
注册

<?php
    include_once("connect.php");
    $username = htmlspecialchars($_POST['username']); //把数据转换为文本
    $telnum = htmlspecialchars($_POST['telnum']);
    $pwd = md5($_POST['pwd']);

    $sql = "INSERT INTO people (username,telnum,pwd) VALUE ('{$username}','{$telnum}','{$pwd}')";

    $res = mysql_query($sql);

    if($res) {
        echo "<script>alert('注册成功,即将返回登录页面');window.location.href='../index.html'</script>";
    }else {
        echo "<script>alert('注册失败');window.history.back()</script>";
    }
?>
```

```php
登录

<?php
    include_once("connect.php");
    $username = $_POST['username'];
    $pwd = md5($_POST['pwd']);

    $pattTel = "/^(13[0-9]|14[579]|15[0-3,5-9]|16[6]|17[0135678]|18[0-9]|19[89])\d{8}$/";

    if(preg_match($pattTel,$username)) {
        $sql = "select id from people WHERE telnum = '{$username}' AND pwd = '{$pwd}'";
    }else {
        $sql = "select id from people WHERE username = '{$username}' AND pwd = '{$pwd}'";
    }

    $res = mysql_query($sql);

    $arr = mysql_fetch_assoc($res);

    if($arr['id']) {
        echo "<script>alert('登录成功');window.location.href = 'http://www.76858586.top/'</script>";
    }else {
        echo "<script>alert('登录失败，请检查用户名和密码是否正确');window.history.back()</script>";
    }
?>
```

```php
连接数据库

<?php
//1.连接数据库
    $link = @mysql_connect("127.0.0.1","root","root");

//2.检查数据库是否连接成功
    if(mysql_errno()) {
        echo mysql_error();
    }

//3.选择数据库
    mysql_select_db("username");

//4.设置字符集
    mysql_set_charset("utf8");

?>
```



# 数据库第四天

## session和cookie

```php
PHP会话管理

打开一个网站（淘宝），浏览、购物、关闭网站就是一次会话
```

## cookie

```php
1、数据存储在客户端
	保存上次登录时间等信息
	保存用户名、密码、在一定时间不用重新登录
	记录用户访问网站的喜欢（比如有无背景音乐，网页的背景是什么）
	cookie存储是明文方式，所以一般存储一些不是很重要的数据，而且不要太多（2KB）
	
在服务器端创建cookie
返回值为bool
设置为setcookie()
第一个参数 cookie的name
第二个参数 cookie的值
第三个参数 超时的时间(存储多久) 以时间戳为值
第四个参数 作用的域名范围 '/' 一般不用

setcookie("name","lzx",time()+(3600*24)); 存在一天

setcookie("name","lzx",time()-1);	设置过去的时间就可以删除了

echo "设置成功"

总结: 方便与js做交互，方便获取用户信息，风险是浏览器可能禁用cookie
```

## session

```php
数据保存在服务器
特点是高效、安全、不依赖浏览器环境、服务器会为每一个用户用一个id来标识

配置session
	session.auto_start = 0;	默认不自动开启
	
	session.cookie_lifetime = 0;	设置按秒记的cookie保存时间，相当于设置session的过期时间，为0的时候表示直到浏览器被重启
	
	session.auto_start = 1;	无需每次使用session之前都要调用
	
	session_start() 但是启用该选项也有一些限制，如果确实启用了，那么session.auto_start则不能将对象放入会话中，因为类定义必须在启动会话之前加载以在会话中重建对象
	
	session.cookie_path = /	cookie的有效路径
	
	seesion.name = PHPSESSID	用在cookie里面session的名字
	
	session.save_path = /tmp 存放路径
	
	session.use_cookie = 1	是否使用cookie
	
	seesion的声明和使用
		必须先启动，使用session_start() 返回值为bool
		注意，seesion_start函数之前不能有任何输出
		session以数组的形式使用
		$_SESSION['键名']
		
销毁变量与销毁SESSION
session_destory();

删除服务器端保留的session信息
unset($SESSION['键名']); 

删除内存中由session数组保存的变量
$_SESSION = Array();

<?php
	session_start();	//重启后需要再开启
	
	$user = array('name'=>'张三','age'=>20,'sex'=>'男')；
	
	$_SESSION['user'] = 'lzx';
	
	echo "<pre>;
	
	print_r($_SESSION)；

?>


if($_POST['autoLoad'] == 'on') {
	setcookie($username,$username,time()+(3600*24*7),'/');
}
```

# 数据库第五天

## GD库

```php
输入localhost可以查看GD库版本

PHP不仅限于只产生html的输出，还可以创建及操作多种不同格式的图像文件。PHP提供一些内置的图像信息函数，也可以使用GD函数库创建新图像或处理已有的图像。目前GD2库支持GIF/JPEG/PNG和WBMP格式。此外还支持一些FREETYPE、TYPE1等字体库。

JPEG	是一种压缩标准的名字，通常是用来存储照片或者存储具有丰富色彩和色彩层次的图像。这种格式使用的是有损压缩。

PNG	是可移植的网络图像，对图像采用了无损压缩的标准。

GIF	原义是“图像互换格式”，是一种基于LZW算法的连续色调的无损压缩格式。

head("content-type: image/png");
```



## 画布管理

```php
1、创建画布方式

imagecreate	新建一个基于调色板的图像
resource imagecreata(int x_size,int y_size);

2、创建真彩色画布方式(使用较多)

imagecreatatruecolor	新建一个真彩色图像
resource	imagecreatatruecolor(int x_size,int y_size);
```

## 设置颜色

```php
imagecolorallocate	为图像分配颜色
参1： 资源类型的图像
参234： rgb
```

## 生成图像

```php
imagepng
imagejpeg
imagegif
imagewbmp
```

## 绘制总和

```php
imagefill	区域填充
bool imagefill(resource image,int x,int y,color);

imagesetpixel	画一个像素
bool imagesetpixel(resource image,int x,int y,color);

imageline	画一条线段
bool imageline(resource image,int x1,int y1,int x2,int y2,color);

imagefilledrectangle	画一个矩形并填充
bool imagefilledrectangle(resource image,int x1,int y1,int x2,int y2,color);

imagepolygon	画一个多边形
bool imagepolygon(resource image,array points,int num_points,color);

imagefilledpolygon	画一个多边形并填充
bool bool imagefilledpolygon(resource image,array points,int num_points,color);

imageellipse	画一个椭圆
bool imageellipse(resource image,int x,int y,int w,int h,color)
x,y圆心坐标

imagefilledellipse	画一个椭圆填充
bool imageellipse(resource image,int x,int y,int w,int h,color)

imagearc	画一个圆弧
bool imagearc(resource image,int x,int y,int w,int h,int start,int end,color); 三点钟方向为起始点
```

## 在图像当中绘制文字

```php
imagestring	水平画一行字符串
bool imagestring(resource image,int font,int x,int y,string,color);

font取值范围1-5

imagestringup	垂直方向画一行字符串
bool imagestringup(resource image,int font,int x,int y,string,color);

imagechar	水平画一个字符
bool imagechar(resource image,int font,int x,int y,string,color);

imagecharup	垂直画一个字符
bool imagecharup(resource image,int font,int x,int y,string,color);

imagettftext	使用自定义的字体库
array imagettftext(resource image,float size,float angle,int x,int y,color,string fontfile,string);	angle是角度
```



## 销毁图像 

```php
imagedestroy(resource);	释放资源
```

## 案例

```php
<?php
    $image = imagecreatetruecolor(500,500);

    $color = imagecolorallocate($image,0,255,255);

    imagefill($image,0,0,$color);

    $color = imagecolorallocate($image,255,0,0);

    imagefilledrectangle($image,5,5,350,180,$color);

    header("content-type: image/png");

    imagepng($image);

    imagedestroy($image);
?>
```

## 验证码案例

```php
<?php
    function code ($w=200,$h=100,$n=4) {
        //生成画布
        $image = imagecreatetruecolor($w,$h);

        //加入画布背景色，颜色选择随机数保证每次底色不同
        $color = imagecolorallocate($image,rand(100,255),rand(100,255),rand(100,255));

        //填充画布
        imagefill($image,0,0,$color);

        //加入干扰项(点和线)
            //加入1000个干扰点
        for($i = 0; $i < 1000; $i++) {
            $x = rand(0,$w);    //每次随机出现的点 x坐标
            $y = rand(0,$h);    //每次随机出现的点 y坐标
            $color_point = imagecolorallocate($image,rand(0,255),rand(0,255),rand(0,255));
            imagesetpixel($image,$x,$y,$color_point);

            //加入20条干扰线
            if($i % 50 == 0) {
                $startX = rand(0,$w);   //线开始的x坐标
                $endX = rand(0,$w);     //线结束的x坐标
                $startY = rand(0,$h);   //线开始的y坐标
                $endY = rand(0,$h);     //线结束的y坐标
                imageline($image,$startX,$startY,$endX,$endY,$color_point);
            }
        }

        //加入验证码(图片)文字
        $str = "0123456789qwertyupkhgfjdsazxcvbnmQWERTYUPKHGJDSAZXCVBNM";

        //获取自定义验证码的长度
        $len = strlen($str);

        //定义一个数组存放每次生成的文字
        $arr = "";

        //循环取出文字
        for($j = 0; $j < $n; $j++) {
            //获取每个文字的下标
            $index = rand(0,$len - 1);

            //把每次循环的文字放入外面定义的数组中
            $arr .= $str[$index];
        }
        $fontsize = ceil(($w - 20) / $n);   //字体大小
        $y = ceil($h / 2 + $fontsize / 2);
        //随机提取4个字生成放入图片中

        imagettftext($image,$fontsize,0,20,$y,$color_point,'./1.ttf',$arr);

        //协议头
        header("content-type: image.png");

        //生成图像
        imagepng($image);

        //销毁图像释放资源
        imagedestroy($image);
    }

    //调用
    code();
?>
```

## PHP设置时间

```php
<?php echo date("Y-m-d H:i:s",$createTime) ?>

 date_default_timezone_set("Asia/ShangHai");
```



## 插入多条数据

```php
insert into cms_user (username,password,sex) select username,password,sex from cms_user;
不可设置唯一

insert into mail(pid,username,tel,address,notes) select pid,username,tel,address,notes from mail;     不可设置唯一
```





# 字符集和排序规则

```php
字符集选择 utf8 -- 	UTF-8 Unicode
排序规则选择 utf8_general_ci
```