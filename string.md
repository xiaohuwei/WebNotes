###		1.数据类型转换和强制转换

#### 1>隐式转换  又称自动转换

**1.数字加字符串 将数字转换为字符串**

```javascript
var num =18；
var str ="hello"
var sum = num + str;//结果为18hello
```

**2.数字加布尔 将布尔转为数字**

```javascript
var bool = true；
var num =18；
var sum = num + bool;
```

**3.字符串+布尔 将布尔转换成字符串**

**4.布尔+布尔 将布尔转换为数字**

####		2>强制转换

1.`toString `
将任意类型的数据转换成字符串语法：`var result = 变量.toString();`

2.`parseInt();`
将任意类型转换成整数如果转换不成功，结果为`NaN` （不是一个数值）；`var demo1 =parseInt(numb1);`

3.`parseFloat（）`将任意类型转换成小数如果不成功 为`nan`

4.`Number`将任意类型转换成`Number `  `var result = Number（变量）`如果包含非法字符 则返回为`nan`

##		2.常量

?>定义：在程序中，声明之后不可以被修改的值

