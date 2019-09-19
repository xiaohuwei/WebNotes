!>记录每天自学的写的`React` 的案例

> 构造一个`React`项目

```node
npm install -g create-react-app   //React 脚手架安装
create-react-app testone  //构造一个React项目
cd testone //进入项目文件夹
npm start //启动
```

?>下面是`React-todoList` 的例子

##  入口文件

```js
//./src/indexx.js
import React from 'react'
import ReactDOM from 'react-dom'//ReactDOM语法支持
import App from './App'//自定义组件 必须大写开头
import Xiaojiejie from './Xiaojiejie'

//JSX JavaScript 和 Xml 混合写法 创建的虚拟Dom 遇到<就当成Html解析 遇到{当成JS解析 将Xiaojiejie组件挂在root上
ReactDOM.render(<Xiaojiejie />,document.getElementById('root'))
```

##  爱啪啪组件

~~~js
//./src/App.js
import React,{Component} from 'react'//es6解构
//等价于下面两行
// import React from 'react'
// const Component = React.Component
//定义一个类 继承Component
 class App extends Component{
     render(){
         return(
            <ul className='myList'>
                <li>{true?'XiaoHuwei':'肖虎威'}</li>
                <li>I Love React......</li>
            </ul>
         )
        //  上面的JSX语法等价于下面
        //  let child1 = React.createElement('li', null, 'XiaoHuwei ')
        //  let child2 = React.createElement('li', null, 'I Love React......')
        //  let parent = React.createElement('ul', {className:'myList'}, child1,child2)
     }
 }
 export default App
~~~

##    小姐姐组件

```js
//./src/Xiaojiejie.js
//Fragment 可以代替组件最外层div
import React,{Component,Fragment} from 'react'
import './Xiaojiejie.css'
class Xiaojiejie extends Component{
    constructor(props){
        super(props) //调用父级方法 ->Component 固定写法
        this.state = {
            inputValue:'',//随便
            list:['头部按摩💆','精油开背💆‍♂️']//存放数据的数组
        }
    }
    render() {
        return (
            <Fragment>
                <div>
                    {/* 第一次写注释 */}
                    <label htmlFor='memeda'>点击聚焦添加服务框</label>
                    <input id='memeda' className='input' value={this.state.inputValue} onChange={this.inputChange.bind(this)}/> 
                    <button onClick={this.addList.bind(this)}>增加服务</button>
                </div>
                    <ul>
                        {/* dangerouslySetInnerHTML={{__html:v}} 使用Html语法解析list内容  li标签就可以不写入内容了*/}
                        {
                            this.state.list.map((v,i)=>{ 
                                return <li key={i+v} dangerouslySetInnerHTML={{__html:v}} onClick={this.deleteItem.bind(this,i)}>
                                             
                                       </li>
                            })
                        }
                    </ul>
            </Fragment>
        )
    }
    //React Change事件绑定 e代表操作的Dom元素 可以获取到input的值
    inputChange(e){
        console.log(e.target.value) //取得每次变化的值
        //不能直接调用this 需要.bind(this)绑定一下
        //this.state.inputValue = e.target.value 错误写法！！！🙅‍♂️
        //正确写法如下√
        this.setState({
            inputValue: e.target.value
        })
    }
    //增加服务按钮绑定click事件
    addList(){
        if(this.state.inputValue!==''){
            this.setState({
                list:[...this.state.list,this.state.inputValue],
                inputValue:''
            })            
        }

    }
    //点击每个服务删除对应的值
    deleteItem(i){
        //this.state.list.splice(i,1) !!!错误写法 不能直接操作state的值 产生性能障碍！！！🙅
        let list = this.state.list
        list.splice(i,1)
       this.setState({
            list:list
       })
    }
}
export default Xiaojiejie
```

## 小姐姐组件样式文件

~~~css
/*./src/Xiaojiejie.css*/
 ul li{list-style: none;}
.input{border:3px solid #ae7000}
~~~



