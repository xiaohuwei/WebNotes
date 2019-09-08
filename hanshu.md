##		系统函数

```javascript
parseInt() //转换成整数
parseFloat()//转成浮点数
prompt()//弹出
alert()//弹出
```

##		自定义函数

~~~javascript
function函数名([参数一，参数二，.....]),可能有参数 可能还没有{

			代码块；

			[return 返回值]
}
~~~

1.带参数的函数 
注意：形参随便定义，只在函数内部有效。函数名，数字/字母/下划线（不可以数字开头！）
小驼峰命名法。

2.不带参的函数
定义了函数，不调用是不会执行的

3.匿名函数

##	变量

1.全局变量：在全局生效

2.局部变量：只在块级生效

##		定时器

~~~javascript
语法
var time = setInterval function(){
},时间毫秒数

clearInterval(time);清除
~~~

##	Math方法

~~~javascript
ceil 上舍入
floor 下舍入
round 四舍五入
random 随机数 0.0---1.0
~~~

##		String 对象用于文本处理

~~~javascript
方法：
toLowerCase（）将字符串中所有的大些变成小写

toUpperCase（）将字符串中所有的小写变成大写

charAt（index） 返回指定下标的值

indexOf（）返回指定值的下标 如果没有返回-1

substring（index1，index2）截取index1到index2的字符串 不包含index2 如果index2为0或者负数，则是从开始截取到index1（index1）.

substr（index，length）从下标index开始截取length长度的字符串

split（分隔符） 将字符串分割成数组

window 顶级对象模型!!!!
~~~

##		浏览器对象模型

~~~javascript
4个对象
history  第二级
location  第二级
document 第二级
window.history
back()返回
go()前进
window.location 
属性
href
方法 
reload（）//刷新 
replace（）取代
window.document
~~~

##		文档对象模型

~~~javascript
window.document
~~~

