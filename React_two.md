!>这是`React`第二天学习笔记整理

## JSX语法的一些坑

###  注释

我第一次写`JSX`注释，是直接这样写的，当然这样写是完全不对的。

~~~js
<Fragment>
    //第一次写注释，这个是错误的
    <div>
        <input value={this.state.inputValue} onChange={this.inputChange.bind(this)} />
        <button onClick={this.addList.bind(this)}> 增加服务 </button>
    </div>
~~~

正确写法如下

~~~js
<Fragment>
    {/* 正确注释的写法 */}
    <div>
        <input value={this.state.inputValue} onChange={this.inputChange.bind(this)} />
        <button onClick={this.addList.bind(this)}> 增加服务 </button>
    </div>
~~~

### JSX里面的Html解析

如果想在文本框里输入一个`<h1>`标签，并进行渲染。默认是不会生效的，只会把`<h1>`标签打印到页面上，这并不是我想要的。如果工作中有这种需求，可以使用`dangerouslySetInnerHTML`属性解决。具体代码如下：

~~~js
<ul>
    {
        this.state.list.map((item,index)=>{
            return (
                <li 
                    key={index+item}
                    onClick={this.deleteItem.bind(this,index)}
                    dangerouslySetInnerHTML={{__html:item}}
                >
                </li>
            )
        })
    }
</ul> 
~~~

## 父子组件传值

### 新建服务菜单组件

在`src`目录下，新建一个文件，这里就叫做`XiaojiejieItem.js`

```javascript
import React, { Component } from 'react'; //imrc
class XiaojiejieItem  extends Component { //cc
   
    render() { 
        return ( 
            <div>小姐姐</div>
         );
    }
}
export default XiaojiejieItem;
```

写好这些代码后，就可以到以前写的`Xiaojiejie.js`文件中用import进行引入，代码如下:

```javascript
import XiaojijieItem from './XiaojiejiItem'
```

### 修改`Xiaojiejie`组件

把原来的代码注释掉，当然也可以删除掉，我这里就注释掉了,注释方法如下:

```javascript
{/*
<li 
    key={index+item}
    onClick={this.deleteItem.bind(this,index)}
    dangerouslySetInnerHTML={{__html:item}}
>

</li>
*/ }
```

然后在最外层加入包裹元素`<div>`，为的是防止两个以上的标签，产生错误信息。

最后直接写入`Xiaojiejie`标签就可以了.

```jsx
<XiaojiejieItem />
```

全部代码:

```javascript
import React,{Component,Fragment } from 'react'
import './style.css'
import XiaojiejieItem from './XiaojiejieItem'

class Xiaojiejie extends Component{
//js的构造函数，由于其他任何函数执行
constructor(props){
    super(props) //调用父类的构造函数，固定写法
    this.state={
        inputValue:'' , // input中的值
        list:['基础按摩','精油推背']    //服务列表
    }
}

render(){
    return  (
        <Fragment>
            {/* 正确注释的写法 */}
<div>
    <label htmlFor="jspang">加入服务：</label>
    <input id="jspang" className="input" value={this.state.inputValue} onChange={this.inputChange.bind(this)} />
    <button onClick={this.addList.bind(this)}> 增加服务 </button>
</div>
            <ul>
                {
                    this.state.list.map((item,index)=>{
                        return (
                            //----------------关键修改代码----start
                            <div>
                                <XiaojiejieItem />
                            </div>
                            //----------------关键修改代码----end
                           
                        )
                    })
                }
            </ul>  
        </Fragment>
    )
}

    inputChange(e){
        // console.log(e.target.value);
        // this.state.inputValue=e.target.value;
        this.setState({
            inputValue:e.target.value
        })
    }
    //增加服务的按钮响应方法
    addList(){
        this.setState({
            list:[...this.state.list,this.state.inputValue],
            inputValue:''
        })

    }
//删除单项服务
deleteItem(index){
    let list = this.state.list
    list.splice(index,1)
    this.setState({
        list:list
    })
    
}

}
export default Xiaojiejie 
```

### 父组件向子组件传值

使用组件属性的形式父组件给子组件传值。比如：我们在`<XiaojiejieItem>`组件中加入`content`属性，然后给属性传递`{item}`，这样就完成了父组件向子组件传值。

```jsx
<XiaojiejieItem content={item} />
```

现在值已经顺利的传递了过去，这时候可以通过`this.props.xxx`的形式进行接受，比如传递过来的值，可以用如下代码进行接收。

```javascript
import React, { Component } from 'react'; //imrc
class XiaojiejieItem  extends Component { //cc
   
    render() { 
        return ( 
            <div>{this.props.content}</div>
         );
    }
}
 
export default XiaojiejieItem;
```

修改完小姐姐子项的组件后，可以打开浏览器进行预览了。试着添加几个新的选项试一下，比如躺式采耳.....。

玩笑归玩笑，学到这里你要记住一点：**父组件向子组件传递内容，靠属性的形式传递。**

### 子组件向父组件传递数据

现在要作这样一个功能：点击组件中的菜单项后，删除改菜单项。涉及了一个子组件向父组件传递数据的知识需要掌握。

先来绑定点击事件，这时候当然是要在`XiaojiejieItem`组件中绑定了，代码如下：

```javascript
import React, { Component } from 'react'; //imrc
class XiaojiejieItem  extends Component { //cc
   
    render() { 
        return ( 
            <div onClick={this.handleClick}>{this.props.content}</div>
         );
    }

    handleClick(){
        console.log('撩拨了小姐姐')
    }
    
}
 
export default XiaojiejieItem;
```

这时候进行预览，打开F12，再点击服务菜单项，就会再`console`里显示出"撩拨了小姐姐"的字样。但是`console`里还有一个`warning`警告，这个警告我们见过，就是要求循环时必须设置key值。

修改`XiaoJieJie`组件的`render`代码如下：

```javascript
<ul>
    {
        this.state.list.map((item,index)=>{
            return (
                <XiaojiejieItem 
                key={index+item}  
                content={item} />
            )
        })
    }
</ul>  
```

绑定成功后，现在就要通过操作子组件删除父组件里的数据了。但是React有明确规定，子组件是不能操作父组件里的数据的，所以需要借助一个父组件的方法，来修改父组件的内容。其实在以前已经写了一个删除方法`deleteItem`，现在要做的就是子组件调用这个方法。

```javascript
//删除单项服务
deleteItem(index){
    let list = this.state.list
    list.splice(index,1)
    this.setState({
        list:list
    })
    
}
```

**获取数组索引下标**

那现在问题来了，要删除就要知道索引值，还是需要通过父组件传递给子组件.这里还是通过`props`属性的形式进行传递。

```jsx
<ul>
    {
        this.state.list.map((item,index)=>{
            return (
                <XiaojiejieItem 
                key={index+item}  
                content={item}
                index={index} />
            )
        })
    }
</ul>  
```

然后修改`XiaojiejieItem`组件，在`handleClick`方法里，写入下面代码：

```javascript
handleClick(){
    console.log(this.props.index)
}
```

这时候预览一下，你会发现点击后报错，错误还是我们的老朋友,没有`bind(this)`。那可以用以前的老方法绑定this.

```jsx
return ( 
    <div onClick={this.handleClick.bind(this)}>
        {this.props.content}
    </div>
);
```

`constructor`绑定this方法。推荐使用 提高性能

```javascript
import React, { Component } from 'react'; //imrc
class XiaojiejieItem  extends Component { //cc
   //--------------主要代码--------start
   constructor(props){
       super(props)
       this.handleClick=this.handleClick.bind(this)
   }
   //--------------主要代码--------end
    render() { 
        return ( 
            <div onClick={this.handleClick}>
                {this.props.content}
            </div>
        );
    }
    handleClick(){
        console.log(this.props.index)
    }
}
 
export default XiaojiejieItem;
```

**子组件调用父组件方法**

如果子组件要调用父组件方法，其实和传递数据差不多，只要在组件调用时，把方法传递给子组件就可以了,记得这里也要进行`this`的绑定，如果不绑定子组件是没办法找到这个父组件的方法的。

```jsx
<ul>
    {
        this.state.list.map((item,index)=>{
            return (
                <XiaojiejieItem 
                key={index+item}  
                content={item}
                index={index}
                //关键代码-------------------start
                deleteItem={this.deleteItem.bind(this)}
                //关键代码-------------------end
                />
            )
        })
    }
</ul>  
```

传递后，在`XiaojiejieItem`组件里直接使用就可以了，代码如下：

```javascript
handleClick(){
    this.props.deleteItem(this.props.index)
}
```

