###		什么是CSS

!>层叠样式表（英语：Cascading Style Sheets，简写CSS），又称串样式列表、级联样式表、串接样式表、阶层式样式表，一种用来为结构化文档（如HTML文档或XML应用）添加样式（字体、间距和颜色等）的计算机语言，由W3C定义和维护。当前最新版本是CSS2.1，为W3C的推荐标准。CSS3现在已被大部分现代浏览器支持，而下一版的CSS4仍在开发中。

?>作用：结合html美化前端网页。

###		CSS引入方式

1.将CSS语句写在style标签中 style写在head标签中.

2.使用link标签将CSS文件引入.

3.在标签的style属性写CSS语句

###		语法

```css
1>标签选择器
标签名{
	属性：值
}

2>class选择器
.类名{
	属性：值
}

3>id选择器
#类名{
	属性：值
}

4>通配符选择器
*{
	属性：值
}
```

###		选择器优先级

!> 通配符选择器<标签选择器<class选择器<id选择器<行内样式<!important

###		代码注释

```
html注释 <!--这里写注释-->
css注释  /*这里写注释*/
```

###		列表

1.无序列表 ul+li

2.有序列表 ol+li

###		表格

!>使用table标签 包含tr标签（代表行）tr标签包含th（加粗）或者td

###		解决margin塌陷

?>在父级加上overflow：hidden

###		浮动布局

float(浮动):让某个元素浮动起来 就是让元素脱离标准文档流

###		定位布局

?>相对定位`position:relative`会保留原先位置

?>绝对定位`position:absolute`不会保留原先位置

!> 想要挪动谁，就给绝对定位。同时观察本身父级，如果父级没有定位，就给父级相对定位。否则就不管。

### 兼容性

条件注释法

```html
<!--[if it IE6]>
中间的内容在IE6中可以显示，其他的都不显示。
<![endif]>
```

### 边框常用属性

**常用属性**

1. border-width；
2. border-style；
3. border-color；

###		颜色取值

1. red yellow green....
2. Rgb/rgba   rgb(255,0,0,1)最后一个是透明度
3. 二进制  #a1b2c3

###		背景颜色

background ： 背景图片地址 平铺方式  背景定位 xy；

###		字体引用

```css
@font-face{
fong-family：‘引入字体命名’；
src：url（）；
}
```

**常用字体属性**

1. font-size 大小
2. font-weight 权重（粗细）
3. font-family 字体样式



###		文本处理

文本超出部分隐藏并且显示三个点

```css
    text-overflow：ellipsis；
    white-space：nowrap；
    overflow：hidden；
    line-height和text-align 实现字体水平垂直居中
    text-decoration:none；去除a链接字体下划线
```

###		伪类选择器

```html
     link 没有单击访问时超链接样式
     visited 单机访问之后超链接样式
     hover鼠标放上去时候
     active  鼠标单击未释放
```

###		阴影

```css
box-shadow
第一个值  在X轴上移动的距离
第二个值  在Y轴的距离
第三个值  距离物体的距离
第四个值  阴影大小 
```



