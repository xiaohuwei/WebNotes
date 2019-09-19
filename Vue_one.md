## 前言

>   前几天做公司项目时候碰到这个问题，需要在Vue项目中使用锚点定位，可是数据又是V-for渲染出来的，于是第一时间想到自己以前写的demo中有一个字母拖动定位的功能。遂去翻了一下那个项目的代码，这个项目使用了`Better-Scroll`作为滚动插件，所以我去找了一下官方文档，照着效果写出来了，先贴一下BS的代码。

## better-scroll滚动效果

```
1.npm中使用npm i better-scroll -S进行安装
2.在要使用的地方引入，使用import BScroll from "better-scroll";
3.下面贴一下基本的结构，很重要！
```

```html
<div ref="wrapper">
   <div class="container">
      <div v-for="(item,key,index) in stepInfo">
            
      </div>
   </div>
</div>
```

```
4.最外层的盒子使用ref获取，然后需要滚动的内容全部丢到container中,至此，基本的结构已经完毕。
5.在data中定义一个变量   aBScroll: null
在Vue的钩子函数mounted中写入如下代码
  mounted() {
    this.$nextTick(() => {
      let bscrollDom = this.$refs.wrapper;	//这里获取到ref元素，也就是最外层的盒子
      //初始化Bs,后面的click设置为true是因为Bs会默认阻止点击事件
      this.aBScroll = new BScroll(bscrollDom, { click: true });	
      this.aBScroll.on("scroll", pos => {
        console.log(pos);	//滚动的时候可以打印滚动的距离
      });
    });
  }
```

```
6.基本的使用已经搞定，现在问题来了，怎么让点击的时候滚动条跳转到对应的Dom元素上，于是我去BetterScroll官网查了，发现了这样一个方法。
scrollToElement(el, time, offsetX, offsetY, easing)；
于是我在自己的项目中这样写

 <!--导航条-->
 <div >
 	<ul>
 		<li v-for="(item,key,index) in stepInfo" :key="index" @click="toStep($event)">
 		<span>{{ key }}</span>
 		</li>
 	</ul>
 </div>
 
 <!--BS滚动区域-->
 <div ref="wrapper">
 	<div>
 		<!--每个单独的盒子 绑定的ref为一个数组-->
 		<div v-for="(item,key,index) in stepInfo" :key="index" :ref="key">
 		</div>		
 	</div>
</div>

/*跳转方法*/
toStep (e) {
	//跳转到指定的项目上
	let c = e.currentTarget.innerText;		//这一步获取到了导航条的文本内容
	const element = this.$refs[c][0];		//这里对下面盒子区域循环的ref进行匹配
	//第一个参数为要跳转到的Dom元素，第二个为跳转事件
	this.aBScroll.scrollToElement(element, 2000)	
}
```

?>至此，效果已经全部实现，路子比较野。然后我兴致勃勃去机器上调试，发现不兼容。内心草泥马一万匹跑过去了，随便说一句。使用的机器和银行的那种自助查询机器类似，平台是谷歌浏览器，强制全屏下无效，于是只能使用原生来解决这个方法，下面附带原生的方法，另外附上BS的API，有兴趣的小伙伴可以去研究一下，BS应该比较适合移动端使用。

官网地址:http://ustbhuangyi.github.io/better-scroll/doc/installation.html#npm

## 原生解决锚点问题

```
在Vue中有一个事件叫@scroll
我们在最外层的大盒子上绑定这个事件

<ul>
    <li v-for="(item,key,index) in stepInfo" :key="index">
    	<span @click="toStep()">{{ key }}</span>
    </li>
</ul>

<div id="wrapper" @scroll="paperScroll">
 	<div>
 		<!--注意这里的ID为循环绑定上的-->
 		<div v-for="(item,key,index) in stepInfo" :key="index" :id="'test' + key">
 		</div>		
 	</div>
</div>

在methods中定义方法
paperScroll () {
    //获取滚动条距离
    let tpScrollTop = document.getElementById("wrapper").scrollTop;
}

toStep () {
    //跳转到指定的盒子上
    const anchorEle = document.querySelector(`#test${key}`);	//找到每个盒子
        if (!!anchorEle) {		//如果有这个盒子存在  !!会把值转为bool型来进行判断
        	//scrollIntoView为把当前视口换成前面的DOM元素区域 behavior为动画效果
            anchorEle.scrollIntoView({behavior: "smooth"});	
    	}
}
```

```
至此，原生的锚点也制作完成，我甚至怀疑BS的根据DOM元素跳转用的就是原生的scrollIntoView方法
附上MDN对这个方法的解释，小伙伴冲冲冲
MDN此方法解释地址:https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView
```

## 最后要说的一些话

?>这个效果看起来很简单，也很常见，但是在这里还是要感谢博主@肖虎威对我的支持，两个人大晚上的去搜了一堆方法才解决的这个问题，希望大家一起努力吧。

@番茄  

2019-9-19 22:42 于武汉


