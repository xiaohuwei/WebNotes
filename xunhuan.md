###		FOR循环

~~~javascript
for循环
语法：
for（条件一；条件二；条件三）{
}
条件一：起始值
条件二：值的限制
条件三：起始值变化
~~~

###		WHILE循环

~~~javascript
var.....
while（表达式）{
 	循环体
               迭代条件
}
注意：使用while首先要在外部申明一个已知变量 再循环过程中，一定要让这个值发生变化 否则死循环！
do while循环
~~~

###		DO WHILE循环

~~~javascript
var ...
do{
循环体
迭代条件
}while(表达式)
无论是否满足条件 首先执行一遍循环体！然后在进行判断 如果满足条件就继续执行循环体
~~~

###	FOR IN 循环（针对数组）

~~~javascript
var arr =[.....];
for(var i in arr){
 
}
~~~

###	跳出循环

1.`break` 跳出循环 继续执行循环外面的代码；破坏了本次循环！

2.`continue` 跳出本次循环 继续下次循环！

###		SWITCH 

~~~javascript
switch（条件）{
	case 判断变量值：
	程序体；
                 break；//结束判断，跳出！
	  ....
	 default
	 当上面的条件都不满足执行此行代码！
	 break；
}
~~~

