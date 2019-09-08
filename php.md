!>PHP（全称：PHP：Hypertext Preprocessor，即“PHP：超文本预处理器”）是一种开源的通用计算机脚本语言，尤其适用于网络开发并可嵌入HTML中使用。PHP的语法借鉴吸收C语言、Java和Perl等流行计算机语言的特点，易于一般程序员学习。PHP的主要目标是允许网络开发人员快速编写动态页面，但PHP也被用于其他很多领域。

##		数据类型

|   名称   |  属性名  |
| :------: | :------: |
|   整形   |   int    |
|  浮点型  |  float   |
|   数组   |   arr    |
| 面向对象 |   obj    |
|   布尔   |   boor   |
|   资源   | resource |
|   ...    |   ...    |

####		数据类型分类

1. 标量类型；
2. 复合类型；//arr obj
3. 特殊类型；//null 资源

##		遍历

```php
foreach($arr as $val){
    echo $val
}
//php独有的遍历方式！//$val 代表每个值
foreach($arr as $k=>$v){
    echo $k."----"$v
}
//$k是索引（键）//$v是值
```

##		PHP数组

> 声明方式

```php
$arr =[1，2，3，4，5];
$arr2 =["user"=>"admin","age"=>25];
$arr[11] ="XXXXX";//增加
$arr2["username"] = "admin";//增加
//关联数组：数组的小标为字符串
//索引数组，下标为数字
//混合数组：字符串+数字
//一维数组......n维数组
print_r($arr)//打印数组
var_dump($arr)//打印数组和数据类型
echo //只能打印标量类型 
```

##		JS补充部分

> 数组splice方法//js对数组处理不友好

```javascript
arr.splice(1,2);
//第一个值代表下标，第二个值代表从该下标截取的长度
arr.splice(1,2,3);
//第一个值代表下标，第二个值代表从该下标截取的长度，第三个值代表加入的字符串
arr.splice(3,0,"helo,wold");
//代表在下标3的后边加上"hello wold"
arr.splice(3,1,"helo,wold");
//代表在下标4的值替换为"hello wold"
```

##		字符串函数

```php
echo count($arr);
//输出数组长度
```



##		超全局变量接受表单
###		H5 表单序列化

```js
$("#table").serialize()
//JQ自动将表单拼串
	<script type="text/javascript">
		$("#btn").click(function(){
			//console.log($("#frm").serialize())
			$.ajax({
				url:"checkLogin.php",
				data:$("#frm").serialize(),   // 表单序列化
				//data:"user=admin&pwd=admin", //拼串
				//data:{user,pwd},    // 对象传值
				type:"post",
				dataType:"json",
				success:function(res){
					console.log(res)
				}
			})	
		})
	</script>
//如果表单要上传文件 需要加上enctype="multipart/form-data"
```

###		PHP

```php
$userName ="xxx";//全局变量->自己定义
print_r($_POST);//超全局变量，接受客户端以post 方式发送的数据//打印出来//一维关联数组
print_r($_FILES);//接受带上传文件功能的表单
```
##		后端响应

> 只要有输出，就是响应。

##		后端打包数据

```php
echo  json_encode($_POST);
//给前端json格式的数据（接受POST到的值打包为json输出去）
```

##		PHP上传文件后

```php
Array
(
    [upfile] => Array
        (
            [name] => 1.png  //文件名
            [type] => image/png  //文件真实地址
            [tmp_name] => C:\Windows\php9A0B.tmp  //临时文件存储路径
            [error] => 0  //错误号 
           //0 正常
           //1 文件过大 超出PHP限制
           //2 文件过大 超出表单限制
           //3 文件上传不完整
           //4 没有选择上传文件
           //6 临时存储文件没有发现
           //7 没有权限
            [size] => 12810  //文件大小
        )

)
    //gbk 一个汉字两个字节
    //utf-8 一个汉字三个字节
```

##		PHP重定向

```php
header("location:un.html");
```

##		PHP上传文件函数

```php
$path= "../uploda/";
move_uploaded_file($upfile["tmp_name"],$path.$upfile("name"));
//$upfile[tmp_name]取到临时文件存储路径
```

##	PHP字符串函数//判断上传文件类型(获取扩展名)

```php
$fileName = "dadsadsadsadd.sadsdsdsd.dsadsds.jpg";
$etxtName =substr($fileName,$index+1);//获取文件后缀（截取字符串）
$index =strpos($fileName,".")//搜索某个字符首次出现的位置
$index =strrpos($fileName,".");//搜索某个字符最后出现的位置
```

##		重命名上传的文件//防止重复

```php
//产生随机数
$newFilename = mt_rand(0,9999).time().mt_rand(0,9999).$etxtName;
```