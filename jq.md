##		JQuery

> jQuery是一套跨浏览器的JavaScript库，简化HTML与JavaScript之间的操作。[1]由约翰·雷西格（John Resig）在2006年1月的BarCamp NYC上发布第一个版本。当前是由Dave Methvin领导的开发团队进行开发。全球前10,000个访问最高的网站中，有65%使用了jQuery，是当前最受欢迎的JavaScript库。

##	jq操作dom

~~~javascript
$("css选择器")
$("#nnn").css("属性名","值")
$("#nnn").css({"background":"red","fontsize":"25px"})
~~~

##		遍历元素

~~~javascript
$(".memeda").each(function(){

//遍历所有元素
//$(this)------>每一个遍历元素

})
~~~

##		hover  事件 （鼠标移入移出）

~~~javascript
$(".memeda").hover(function(){

 //移入事件
},function(){

//移出事件
})
~~~

##		数组 对象的遍历 

~~~javascript
$.each(arr,function(index,val){
//index 所有下标
//val 所有值
})
~~~

##		创建节点

~~~javascript
$("htmlTag")
追加节点
$(ele).before()/////同胞之前
$(ele).after()////////同胞之后
$(ele).before()////
~~~

##	万物皆对象

~~~javascript
var obj={}
所有对象_proto_(隐士原型) //对象都有这个属性！
attr（‘属性名字’） 当属性不存在 获取不到 可以自定义
prop（“属性名字”） 当属性不存在 也可以获取 不存在自定义
~~~

##		json对象

~~~javascript
obj={name:val,..........,action:function(){
}}
obj.属性
obj.方法（）
$(parent).append(child)          --------->追加子元素
$(ele).before()                  --------->之前追加
$(ele).after()                   --------->之后追加
$(ele).remove()                  --------->删除本身
~~~

##		jq效果属性

~~~javascript
hide（）//隐藏===display:none
show（）//展示===display：block
fadeIN（）//透明度由0到1
fadeOut（）//透明度由1到0
sideDown（）//元素收起 过度宽度 从上往下
sideUp（）//元素放出 过渡高度 从下往上
sideToggle（）//自动缩放
~~~

