# JavaScript高级

##	数据类型

> 在JS中 字符串不只是字符串 还是一个对象

```js
string类型
var str = 'xxxxxx';//一般声明方式
var str2 = new String('yyyyyy');//底层原型声明方式->对象

array类型(typeof为object) //万物皆对象（函数）
var arr = [1,2,3];//一般声明方式
var arr2 = new Array();//底层原型声明方式->对象

object类型
var obj = {};//一般声明方式
var arr2 = new Object();//底层原型声明方式->对象
```

###		构造函数(本质为自定义函数)

> 区别：构造函数与自定义函数唯一没有意义的区别为构造函数命名方式采用大驼峰
>
> > 作用：一般用来构建对象（通过`new`关键字实例化）如  `new String()` `new Array()`

~~~js
//构造函数
function Xiaohuwei(){
    console.log('Hello world');
}
//自定义new一个女朋友对象
function Grilfriend(){
    console.log('memeda');
}
//实例化
var gf = new Grilfriend();
~~~

####		判断是否为真数组

```js
var arr = [1,2,3];
通过instance关键字判断//instanceof判断原型对象是否一致（是否在一条原型链上）
console.log(arr instanceof Array)；//返回值为布尔
数组可以为数组或者对象 但是对象不能为数组
```

### 再分类

**数组和对象统一为为引用类型**

----------------

##		作用域问题

```js
var str = 'xxxxxx';//全局(顶级)->所有区域都可使用
function memeda(){
    var str2 = "yyyyy";//二级变量 作用域在memeda内部
    console.log(str);
    function heheda(){
        console.log(str);
        console.log(str2);
    }
    heheda();
    console.log(str2);
}
memeda();
作用域链：作用域的生效区间由外到内延申 形成的作用域区间
```

##	函数闭包

> 也就是内嵌函数

~~~js
function son(){
    var num = 1；//二级变量初始值
    function getNum(){
        num++;
        return num;//将num的值反出去
    }
    return getNum;//将内部嵌套的函数反出去
}
var getnums = son();//代理到了内部嵌套函数
console.log(getnums());//第一次 2 
console.log(getnums());//第二次 3 
console.log(getnums());//第三次 4 
var num = getnums()//第四次 5
console.log(num)//5
num = null //手动销毁（置空）
console.log(getnums());//第五次 null 
普通变量：用完被GC回收 
闭包后的变量:保存在内存中 不被销毁 可以动态获取到局部变量的值
~~~

**过多的使用闭包会导致内存泄露,可以手动销毁（避免内存泄漏）也就是手动置空**

##		自调函数

~~~js
var pras = "tom";
(function(val){
    console.log('hello'+val);
})(pras)
永远自动执行一次 在闭包可以被完美解析
~~~

##  闭包和自调函数应用场景

> 已知页面有诺干个`li`标签 点击每一个弹出相应的下标 

~~~js
for(var i = 0; i<lilist.length;i++){
    (function(){//自调函数
        var id = i;//将i重新赋值给id 保存当前i的值 之后与i无关
        lilist[i].onclick=function(){
            alert(id);//弹出上面保存过的i的值
        }
    })()
}
es6 可以用let声明解决
~~~

##		变量提升

> 在js中 变量的声明是最优先的 赋值之类的操作按照正常程序从上至下
>
> > `var`没有块级作用域 可以变量提升 `let`有块级作用域 不存在提升

```js
变量在使用过程中会使用就近原则
var memeda = 'xxxxxxx';
function heheda(){
    console.log(memeda);//打印underifend  因为它最近作用域在函数内部 而函数内部只申明了还没有赋值
    var memeda = '100';
    console.log(memeda)
}
```

## 	JS面向对象

**本质就是构造函数**

> `new`  关键字 创建一个空对象 同时让这个对象指向自定义的变量 并且执行构造函数 构造函数里面的`this` 指向这个新对象 如果实例对象有参数 会把参数挂载到新对象上相当于传给自己  最后  构造函数会隐式的返回一个对象。让New出来的新对象的隐式原型等于构造函数的原型对象。

~~~js
 对象字面量
 var obj = {};
//创建对象 封装构建函数 不能直接调用！需要通过new关键字
function User(userName,sex){
    this.userName = userName;
    this.sex = sex;
    this.dosomething = function(){
        console.log('i can singing....');
    }
}
//调用
var stu = new User('么么哒','女');
console.log(stu.userName);
~~~

|  `prototype`  | 显示原型  只有构造函数才具有  也是一个对象 具有隐式原型 |
| :-----------: | :-----------------------------------------------------: |
|   `_proto_`   |                隐式原型  只有对象才具有                 |
| `constructor` |           所有对象都具备该属性  指向构造函数            |

**实例化对象的  ``__proto__``  指向其构造函数的  ``ptototype(原型对象)``  既 ``stu.__proto__ == User.prototype``**

**构造函数原型对象的构造器 既`constructor`  指向构造函数本身 既`User`   `User.prototype.constructor=User`**

####  原型链关系

**`stu.__proto__ == User.prototype.__proto__ == object.prototype.__proto__ == null `**

## 有了显示原型才有隐式原型

> **所有的隐式原型都不可用 ,一般用到的方法都是在用底层构造函数的显示原型，因为隐式原型继承的是构造函数的显示原型。所有对象的构造函数（原型）都是  `Object() `  object的父构造函数为空。**

##		继承 Extends

> 将一些对象公有的属性和方法挑选出来放到一个父原型里面

###		原型链直接继承

~~~js
人类
function Person(){

}
    Person.prototype.man = '男'；
    Person.prototype.women = '女'；
    Person.prototype.eat = function(){
        console.log("i can eat ...");
    }
    Lower.prototype = Person.prototype//原型链直接继承
	Lower.prototype.constructor = Lower//解决继承链紊乱问题
程序员
function Lower(){
    this.writeBug = function(){
        console.log('i can just bug...')
    }
}
var obj = new Lower();//会导致继承链紊乱
console.log(obj);
~~~

###		直接继承

```js
公有手机原型
function Phone(color){
    this.color=color;
    this.playgame=function(){
        console.log("i can playgames");
    }
}
Huaweipro30.prototype = new Phone("black");//将华为的原型指向手机公有的原型 既继承 
华为手机
function Huaweipro30(color){
    this.color=color;
    this.hwx = function(){
        console.log("hwx....");
    }
}
//原型直接继承
var hw = new Huaweipro30();//会导致继承链紊乱
Huaweipro30.prototype.constructor = Huaweipro30;//解决继承链紊乱问题
console.log(hw.color);//black
```

###		继承链紊乱

> 使用上述实例化后 ` hw`对象的构造函数指向`Phone()`  而不是指向`Huaweipro30()` 解决办法如上

## 注意

> 数组和对象属于**引用类型**不会重新开辟内存，如果用一个构造函数实例化了两个对象A 和B ，并且继承方式采用上面任何一种，如果在对象A中改变了构造函数的数组或者对象，那么改了之后相应的数组或者对象在对象B中也会生效，如果对象A修改了构造函数的属性（字符串）会重新开辟内存，则不会影响对象B，只对A生效。解决办法就是改变继承方式，使用构造函数绑定解决。

##		构造函数绑定 Call 和 Apply 

> 所有函数的构造方法（原型）为  `Function` 而该函数里面具有  `call()` 和 `apply()`  方法
>
> > 注意：这两种方法只有构造函数具有
> >
> > > **作用：将一个构造函数的实例对象赋值给另一个对象（对象复制）**

###		一个参数实例

~~~js
function User(){
    this.userName = "张三"；
    this.doing = function(){
        console.log('i can liaomei...');
    }
}
function Memeda(){
    
}
var obj = new Memeda();//用Memeda实例化
User.call(obj)//将User对象的属性和方法复制给了obj
或者
User.apply(obj)
console.log(obj.userName)//张三
~~~

###		或者

~~~js
function User(){
    this.userName = "张三"；
    this.doing = function(){
        console.log('i can liaomei...');
    }
}
function Memeda(){
    //构造函数绑定继承
    User.call(this);
}
var obj = new Memeda(); 
console.log(obj.userName)//张三
~~~

## 两个参数实例->传参问题

~~~js
function User(userName,sex){
    this.userName = userName；
    this.sex = sex；
    this.doing = function(){
        console.log('i can liaomei...');
    }
}
function Memeda(userName,sex){
    //构造函数绑定继承
    User.call(this,userName,sex);//参数传递匹配
    或者
    User.apply(this,[userName,sex]);
}
var obj = new Memeda('张三','女'); 
console.log(obj.userName)//张三
console.log(obj.sex)//女
~~~

> `call()` 和 `apply()`的区别在构造函数实例化过程中参数传递绑定的方式不一样，如上代码块， 前者需要一行写完，后者只需要一个数组即可。



---------------

#	ES6面向对象

## 语法糖🍬

在``class``类的内部 只能出现方法

~~~js
class User{
    //声明属性->魔术方法 当User类被实例化时直接自动调用 
    constructor(userName,sex){//固定名字
        this.userName = userName；
        this.sex = sex；
        this.doing();//实例化下面的doing方法
    }
    //声明方法
    doing(){
        console.log('...xxx...');
    }
}
var obj = new User('么么哒'，'男');
//调用方法
obj.doing();//...xxx...
//调用属性
obj.userName;//么么哒
obj.sex;//男
~~~

## ES6继承

~~~js
//父
class Phone{
    constructor(color,type){
        this.color = color;
        this.type = type;
    }
    callNum(){
        console.log("i can call....")
    }
}
//子
class Hw extends Phone{ //通过extends关键字继承
    //参数位置要写上父级的参数
    constructor(size,color,type){
        //必须使用super关键字子级和父级的继承关系才正常 这样就会把两级的构造函数合并
        //并且参数位置要写父级构造函数的所有参数 
        super(color,type);
        this.size = size;
    }
    hwx(){
        console.log("i can position...");
    }
}
var obj = new Hw("p30",'red','iphone');//用子构造函数实例化
obj.callNum();//i can call....父级方法
obj.hwx();//i can position .... 子级方法
console.log(obj.type) ;//iphone 父级属性
console.log(obj.color)//red 父级属性
console.log(obj.size)// p30 子级属性
~~~

> 当子函数没有构造函数的时候会驱动父级的构造函数,`es6`继承时需要使用`super()`方法关联 参数位置写父构造函数的参数。

##  类的特征

<font color=red size=4>1.封装 2.继承 3.多态</font>

##		PHP构造函数

~~~php
class Person{
    public $color = 'red';
    //构造函数
    function__construct(){
        
    }
    function doing(){
        
    }
}
~~~