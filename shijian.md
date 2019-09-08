##	 鼠标单击事件

1.在标签中写onclick，执行后面的JS代码

2.通过Dom操作节点，给节点绑定事件

##		onload事件

1.放在body中

2.在js中，放在window对象中

~~~javascript
window.onload=function(){

}
~~~

##		onchange 事件

?>检测input输入的内容是否发生变化，用户输入完成后，点击文本框以外的区域，是文本框失去焦点才触发的事件

##		事件绑定

1.获取节点，使用on的方式绑定事件

2.行内使用`onclick`属性

3.事件监听`（addEVentListener）`事件监听   移出事件` remove addEVentListener`

##	常用的js事件

**鼠标事件**

~~~javascript
onclick  单击
ondblclick 鼠标双击
onmouseover 鼠标移入
onmouseout  鼠标移出
onmousedown 鼠标按下
onmousemove 鼠标移动
onmouseup 鼠标抬起
~~~

**键盘事件**

~~~javascript
onkeydown  按键按下
onkeyup     按键抬起
onkeypress  按键释放的一瞬间
~~~

