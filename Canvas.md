#	Canvas

> Canvas是H5提供的绘图容器。

##		绘制线段 

~~~js
var canvas = document.getElementById("canvas");
//创建2d画布
var cxt = canvas.getContext("2d");
//线 图形 圆 文字都可以画....
cxt.beginPath();//设置开始
cxt.moveTo(50,100);//起点 X Y
cxt.lineTo(150,100);//终点  X Y
cxt.strokeStyle = "#ddd";//设置描边颜色
cxt.lineWidth = "5";//设置宽度
cxt.stroke();//描边
cxt.closePath();//设置结束
~~~

## 绘制三角形🔺

~~~js
var canvas = document.getElementById("canvas");
//创建2d画布
var cxt = canvas.getContext("2d");
//线 图形 圆 文字都可以画....
cxt.beginPath();//设置开始
cxt.moveTo(50,50);//起点 X Y
cxt.lineTo(100,100);//中点  X Y
cxt.lineTo(200,50);//中点  X Y
cxt.lineTo(50,50);//终点  X Y
cxt.strokeStyle = "#ddd";//设置描边颜色
cxt.fillStyle = "yellow";//设置填充颜色
cxt.fill()
cxt.lineWidth = "5";//设置宽度
cxt.stroke();//描边 cxt.fill() 填充
cxt.closePath();//设置结束

或者 结束绘制之后再描边 //只需要画两根线

cxt.beginPath();//设置开始
cxt.moveTo(50,50);//起点 X Y
cxt.lineTo(100,100);//中点  X Y
cxt.lineTo(200,50);//中点  X Y
cxt.strokeStyle = "#ddd";//设置描边颜色
cxt.fillStyle = "yellow";//设置填充颜色
cxt.fill()
cxt.lineWidth = "5";//设置宽度
cxt.closePath();//设置结束
cxt.stroke();//描边 cxt.fill() 填充

~~~

## 绘制统计图🅰

~~~js
var cxt = canvas.getContext("2d");
//竖线和横线 X Y 轴
cxt.beginPath();
cxt.moveTo(50,50);
cxt.lineTo(50,350);
cxt.lineTo(451,350);
cxt.strokeStyle="red";
cxt.stroke();
cxt.closePath();
//Y轴刻度
for(var i=1;i<=6;i++){
    cxt.beginPath();
    cxt.moveTo(51,50*i);
    cxt.lineTo(40,50*i);
    cxt.strokeStyle="red";
    cxt.stroke();
    cxt.closePath();
}
//X轴刻度和矩形
for(var i=1;i<=7;i++){
    cxt.save()  // 保存 当前的绘制状态
    cxt.beginPath()
    // y 轴 刻度
    cxt.translate(50,349) //平移 基点(重置 绘制基点)
    cxt.moveTo(58*i,0)   
    cxt.lineTo(58*i,10)  //当前终点
    cxt.stroke()
    var offsetheight = Math.random()*160+40;
    // 矩形 
    cxt.fillStyle = "red";
    cxt.fillRect(58*(i-1)+5 ,0,50,-offsetheight)
    cxt.closePath()
    cxt.restore()  // 初始化
}

~~~

####  属性和方法

|               属性               |             功能              |
| :------------------------------: | :---------------------------: |
|           lineTo(x,y)            |          线段的终点           |
|           moveTo(x,y)            |          线段的起点           |
|          stroke() 方法           |          绘制、描边           |
|       strokeStyle="color";       |         设置描边着色          |
|         lineWidth="px";          |       设置当前描边颜色        |
|         beginPath() 方法         |           起始路径            |
|         closePath() 方法         |           闭合路径            |
|           fill() 方法            |           填充形状            |
|        fillStyle="color"         |           填充颜色            |
|          translate(x,y)          |           平移基点            |
|           save( ) 方法           |           保存状态            |
|         restore( ) 方法          |        撤销保存的状态         |
|    fillRect(x,y,width,height)    |           填充矩形            |
|   strokeRect(x,y,width,height)   |           描边矩形            |
|  fillText("text",x,y,maxwidth)   |           填充字体            |
| strokeText("text",x,y,maxwidth)  |           描边字体            |
|     font="bold px  微软雅黑"     | 设置字体 必须有字体设置才生效 |
|  textAlign="left/right/center"   |      水平文本以基线对齐       |
| textBaseline="top/bottom/center" |      垂直文本以基线对齐       |
|           rotate(deg)            |    旋转度数 括号内不带单位    |