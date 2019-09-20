!>这是自学`React`的第三个晚上，加油加油。

## Ref的使用

以前的案例中，我们写了下面的代码，使用了`e.target`，这并不直观，也不好看。这种情况我们可以使用`ref`来进行解决。

```javascript
inputChange(e){
  this.setState({
    inputValue:e.target.value
  })
}
```

如果要使用`ref`来进行，需要现在`JSX`中进行绑定， 绑定时最好使用ES6语法中的箭头函数，这样可以简洁明了的绑定DOM元素。

```jsx
<input 
  id='memeda' 
  className='input' 
  value={this.state.inputValue} 
  onChange={this.inputChange.bind(this)}
  ref = {input=>this.input=input}//input只是形参 就一个 代表当前Dom
  /> 	
```

绑定后可以把上边的类改写成如下代码:

```js
inputChange(){
  this.setState({
    inputValue:this.input.value
  })
}
```

这就使我们的代码变得语义化和优雅的多。但是不建议用`ref`这样操作的，因为`React`的是数据驱动的，所以用ref会出现各种问题。

### `ref`使用中的坑

比如现在我们要用ref绑定取得要服务的数量，可以先用`ref`进行绑定。

```jsx
<ul ref={res=>this.res=res}>{/* ref绑定*/}
  {/* dangerouslySetInnerHTML={{__html:v}} 使用Html语法解析list内容  标签就可以不写入内容了*/}
  {
    this.state.list.map((v,i)=>{ 
      return (
        <XiaojiejieItem 
          key={v+i} 
          index={i} 
          content={v}//向子组件传值 子组件使用this.props.content接收
          deleteItem = {this.deleteItem} />
      )
    })
  }
</ul>
```

绑定后可以在`addList()`方法中，获取当前`<li>`有几个.

```js
 addList(){
    this.setState({
        list:[...this.state.list,this.state.inputValue],
        inputValue:''
    })
    //关键代码--------------start
    console.log(this.res.querySelectorAll('li').length)
    //关键代码--------------end

}
```

这时候你打开控制台，点击添加服务按钮，你会返现数量怎么少一个？（就是这个坑），其实这个坑是因为React中更多`setState`是一个异步函数所造成的。也就是这个`setState`，代码执行是有一个时间的，如果你真的想了解清楚，你需要对什么是虚拟DOM有一个了解。简单的说，就是因为是异步，还没等虚拟Dom渲染，我们的`console.log`就已经执行了。

那这个代码怎么编写才会完全正常那，其实`setState`方法提供了一个回调函数，也就是它的第二个函数。下面这样写就可以实现我们想要的方法了。

```js
addList(){
    this.setState({
        list:[...this.state.list,this.state.inputValue],
        inputValue:''
        //关键代码--------------start
    },()=>{
        console.log(this.res.querySelectorAll('li').length)
    })
    //关键代码--------------end
}
```

现在到浏览器中查看代码，就完全正常了。

-----------------------

## PropTypes的简单应用

我们在`Xiaojiejie.js`组件里传递了4个值，有字符串，有数字，有方法，这些都是可以使用`PropTypes`限制的。在`XiaojiejieItem.js`需要先引入`PropTypes`。

```js
import PropTypes from 'prop-types'
```

引入后，就可以在组件的下方进行引用了，需要注意的是子组件的最下面（不是类里边），写入下面的代码：

```js
//检验值
XiaojiejieItem.propTypes={
    content:PropTypes.string,
    index: PropTypes.number,
    deleteItem: PropTypes.func
}
```

`XiaojiejieItem.JS`文件的代码。

```js
import React, { Component } from 'react';
import PropTypes from 'prop-types';//数据校验 在子组件里面 检验父组件的值
class XiaojiejieItem extends Component {
    constructor(props){
        super(props)
        this.handleClick = this.handleClick.bind(this)//优化.bind(this)写法 性能优化
    }
    render() { 
        return ( 
            <li onClick={this.handleClick}>
            {this.props.avname}-为你服务{this.props.content}
            </li> 
         );
    }
    handleClick(){
        this.props.deleteItem(this.props.index) //父组件传递的方法和参数在子组件使用
    }
}
//检验值
XiaojiejieItem.propTypes={
    content:PropTypes.string,
    index: PropTypes.number,
    deleteItem: PropTypes.func,
    avname: PropTypes.string.isRequired
}
//默认值
XiaojiejieItem.defaultProps={
    avname:'苍井空'
}
export default XiaojiejieItem;
```

这时候你在浏览器中查看效果，是什么都看不出来的，你需要修改一个错误的校验。比如我们把index改为必须是字符串。

```js
index:PorpTypes.string
```

这时候浏览器的`console`里就会报错了，报错信息如下：

```text
Warning: Failed prop type: Invalid prop `index` of type `number` supplied to `XiaojiejieItem`, expected `string`.
    in XiaojiejieItem (at Xiaojiejie.js:28)
    in Xiaojiejie (at src/index.js:5)
```

意思就是要求传递字符串，而我们却传递了数字过去，所以给了警告。

### 　必传值的校验

比如现在我们加入一个`avname`的属性，并放入`JSX`中，就算不传递这个值也不会报错的。代码如下：

```js
return ( 
  <li onClick={this.handleClick}>
  {this.props.avname}-为你服务{this.props.content}
</li> 
);
```

这时候代码是不会报错的，我们传不传无所谓。比如我们现在传一个属性过来。

```jsx
<ul>
    {
        this.state.list.map((item,index)=>{
            return (
                <XiaojiejieItem 
                key={index+item}  
                content={item}
                index={index}
                avname='波多野结衣'
                deleteItem={this.deleteItem.bind(this)}
                />
            )
        })
    }
</ul>  
```

这时候页面显示正常了，但是怎样避免必须传递avname这个属性值?如果不传递就报错,这就需要使用`isRequired`关键字了,它表示必须进行传递，如果不传递就报错。

```js
avname:PropTypes.string.isRequired
```

### 　使用默认值`defaultProps`

`defalutProps`就可以实现默认值的功能，比如现在把`avname`的默认值设置成"松岛枫" ，然后把`avname`的属性删除掉。

```js
XiaojiejieItem.defaultProps = {
    avname:'松岛枫'
}
```

官方文档还有很多的，能得到比较全面的了解。

----------

## React生命周期

!>`React`的生命周期是非常重要的知识点!`React`的生命周期是非常重要的知识点!`React`的生命周期是非常重要的知识点!

![img](https://cdn.xiaohuwei.cn/2019/09/382606137.jpg)



?>上图即为`React`的生命周期，值得注意的是，在`React16`及后面的版本中，有些生命周期的名字前面加上了`UNSAFE_`前缀。请以下面的例子为标准。

可以看到React生命周期的四个大阶段：

- `Initialization`:初始化阶段。

- `Mounting`: 挂载阶段。

- `Updation`: 更新阶段。

- `Unmounting`: 销毁阶段

### 生命周期函数的定义

> 生命周期函数指在某一个时刻组件会自动调用执行的函数

?>特别的，`constructor`我们叫构造函数，它是ES6的基本语法。虽然它和生命周期函数的性质一样，但不能认为是生命周期函数。但是你要心里把它当成一个生命周期函数，我个人把它看成React的`Initialization`阶段，定义属性（props）和状态(state)。

### Initialization 初始化阶段

~~~js
constructor(props){
  //constructor是Es6的语法 自动执行 相当于React的初始化生命周期函数initialization
  console.log('initialization=====>初始化阶段')
}
~~~

### Mounting挂载阶段

Mounting阶段叫挂载阶段，伴随着整个虚拟DOM的生成，它里边有三个小的生命周期函数，分别是：

1. `UNSAFE_componentWillMount` : 在组件即将被挂载到页面的时刻执行。
2. `render` : 页面state或props发生变化时执行。
3. `componentDidMount` : 组件挂载完成时被执行。

**UNSAFE_componentWillMount**代码

```js
//属于Dom挂载阶段Mounting的生命周期 
UNSAFE_componentWillMount() {
  console.log('UNSAFE_componentWillMount=====>组件将要挂载到页面的时刻')
};
```

**componentDidMount**代码

```js
//属于Dom挂载阶段Mounting的生命周期
componentDidMount(){
  console.log('componentDidMount=====>组件挂载完成的时刻')
};
```

**render**代码

```js
render() {
  //属于Dom挂载阶段Mounting的生命周期 
  console.log('render=====>组件挂载中的时刻',3)
}
```

这时候我们查看一下控制台，会为我们打出下面的

![](https://cdn.xiaohuwei.cn/2019/09/84300496.png)

**这个函数书写没有顺序**

**注意的问题**

?>`UNSAFE_componentWillMount`和`componentDidMount`这两个生命周期函数，只在页面刷新时执行一次，而`render`函数是只要有state和props变化就会执行。

### Updation组件更新阶段

React生命周期中的`Updation`阶段,也就是组件发生改变的更新阶段，这是React生命周期中比较复杂的一部分，它有两个基本部分组成，一个是`props`属性改变，一个是`state`状态改变。

### shouldComponentUpdate函数

`shouldComponentUpdate`函数会在组件更新之前，自动被执行。比如写入下面的代码:

```javascript
//属于Updation的生命周期 必须return布尔值 
//为true执行UNSAFE_componentWillUpdate和render函数 false则不执行
shouldComponentUpdate(){
  console.log('shouldComponentUpdate=====>组件更新之前时刻',1)
  return true;
};
```

它要求返回一个布尔类型的结果，必须有返回值，这里就直接返回一个`true`了（真实开发中，这个是有大作用的）。

现在就可以在控制台`console`里看到结果了，并且结果是每次文本框发生改变时都会随着改变。如果你返回了`false`，这组件就不会进行更新了。 简单点说，就是返回true，就同意组件更新;返回false,就反对组件更新。

### UNSAFE_componentWillUpdate函数

`UNSAFE_componentWillUpdate`在组件更新之前，但`shouldComponenUpdate`之后被执行。但是如果`shouldComponentUpdate`返回false，这个函数就不会被执行了。

```javascript
//属于Updation的生命周期
UNSAFE_componentWillUpdate() {
  console.log('UNSAFE_componentWillUpdate=====>组件将要更新的时刻',2)
}
```

### componentDidUpdate

`componentDidUpdate`在组件更新之后执行，它是组件更新的最后一个环节。

```javascript
//属于Updation的生命周期
componentDidUpdate(){
  console.log('componentDidUpdate=====>组件更新完成的时刻', 4)
}
```

为了方便我们看出结果，可以在每个函数后加上序号。当我们输入1可以看到控制台输出的结果如下：

![](https://cdn.xiaohuwei.cn/2019/09/288763954.png)

### UNSAFE_componentWillReceiveProps 函数

我们可以先在`Xiaojiejie.js`组件里写下这个函数，例如下面的代码。

```javascript
//组件第一次存在于虚拟dom中函数是不会执行的
//如果存在虚拟dom中 函数才会被执行
UNSAFE_componentWillReceiveProps() {
  console.log('UNSAFE_componentWillReceiveProps')
}
```

这时候会发现函数什么时候都不会被执行，因为`Xiaojiejie.js`算是一个顶层组件，它并没接收任何的`props`。可以把这个函数移动到`XiaojiejieItem.js`组件中。

凡是组件都有生命周期函数，所以子组件也是有的，并且子组件接收了`props`，这时候函数就可以被执行了。

```javascript
//组件第一次存在于虚拟dom中函数是不会执行的
//如果存在虚拟dom中 函数才会被执行
UNSAFE_componentWillReceiveProps() {
  console.log('UNSAFE_componentWillReceiveProps')
}
```

这个时候再预览，就会看到`UNSAFE_componentWillReceiveProps`执行了 ，下图当我输入1代表子组件会收到Props的时候

![](https://cdn.xiaohuwei.cn/2019/09/819754796.png)

> 子组件接收到父组件传递过来的参数，父组件render函数重新被执行，这个生命周期就会被执行。

- 也就是说这个组件第一次存在于Dom中，函数是不会被执行的;
- 如果已经存在于Dom中，函数才会被执行。

这个生命周期算是比较复杂的一个生命周期，需要花时间消化。

### componentWillUnmount函数

这个函数是组件从页面中删除的时候执行，比如在`XiaojiejieItem.js`，写入下面的代码:

```javascript
//组件被删除之后执行
componentWillUnmount() {
  console.log('componentWillUnmount--组件删除')
}
```

写完后，可以到浏览器终端中查看结果，当我们点击服务项，服务项被删除时，这个函数就被执行了。

![](https://cdn.xiaohuwei.cn/2019/09/698975628.png)

---------

@XiaoHuwei  2019-09-21  Hangzhou

