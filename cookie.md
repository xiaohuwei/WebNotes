#		后端Cookie🧀

##		特性

跨页面，全局范围，不跨服务器。

## 1.session会话（更安全）

> 运行在服务器。
>
> > 作用：记录用户信息，保存用户状态。

## 2.Cookie/饼干🍪

> 运行在客户端。



###		生成一个session

~~~php
session_start();//启用session
//存值
$_SESSION["test"]="memeda";
//接受session
session_start();
echo $_SESISON["test"];
~~~

###		PHP生成cookie(可以设置时间)

~~~php
setcookie("test","memeda","time()+秒数");
//接受Cookie
$_REQUEST["test"];
//接受COOKIE
$_COOKIE["test"];
~~~

<hr/>

#	前端cookie🍪

## 两种协议

1. http协议 无状态请求
2. https协议 

> 运行在客户端，与服务器共享。默认浏览器关闭失效。不超过4KB。
>
> > 运用地方：记住密码，购物车。

##	设置cookie

```js
document.cookie = "test=memeda...";
```

## cookie取值

```js
console.log(document.cookie);
//PHP
$_COOKIE
```

## cookie在存值的时候只能存字符串

~~~js
//序列化，序列成有序的json字符串
var product = [{pid:1,pname:"手机"，price：2999.00},
              {pid:2,pname:"电脑"，price：2999.00},
              {pid:3,pname:"手表"，price：2999.00}]

var datastr = JSON.stringify(product);
document.cookie = "obj="+datastr;
~~~

##		拆分cookie拿到具体值

```js
var str = document.cookie;
var arr = str.split(";");
var data = [];
for( k in arr ){
   var val =  arr[k].split("=");
    data.push(val[1]);
};
var obj = JSON.parse(data[3]);
console.log(data[3]);
```

##		设置带生命周期的cookie

```js
var date = new Date();//获取时间对象
var timestamp = date.gettime();//获取时间戳 1970-01-01 到现在的毫秒
var offsetTime = timestamp + 30*1000；//过期时间 30s
date.setTime(offsetTime);//设置过期时间
document.cookie = "memeda=12331....;expires="+data.toGMTString(); 
```

##		PHP序列化

```php
serialize($arr);//php存值需要单独存每一个属性 不然前端读不出来
```

##		H5 JS获取元素

~~~js
var Element = queryselect("#id")//id选择器 找到第一个元素
var Element = queryselectall("#id")//id选择器 获取所有元素
~~~





