# 非兄弟组件传值和VUEX

###		事件总线传值

新建一个js文件 `memeda.js`

~~~js
import Vue from 'Vue'
var Bus = new Vue({})
export default Bus
~~~

新建两个组件

~~~js
<template>
    <div>
        {{msg}}
        <button @click="fn"> 发送</button>  
        <Home1></Home1>
    </div>
</template>
<script>
import Bus from '../memeda.js'
import Home1 from './home1'//引入Home1组件 同一页面展示
export default {
    components:{
        Home1
    },
    data(){
       return{
            msg:'我是home'
       }
    },
    methods:{
        fn(){
            Bus.$emit('meme',this.msg)
        }
    }
}
</script>
~~~

~~~js
<template>
    <div>
        {{msg}}
    </div>
</template>
<script>
import Bus from '../memeda.js'
export default {
    data(){
       return{
            msg:'我是home1111'
       }
    },
    mounted(){
        Bus.$on('meme',(res)=>{
            alert(res);//我是home
        })
    }
}
</script>

<style>

</style>
~~~

###	VUEX传值(mutations和state)

#### 逻辑图

![](https://vuex.vuejs.org/vuex.png)

#### 安装VUEX

~~~nginx
npm i vuex --save
~~~

#### 自建js文件

~~~js
import Vue from 'vue'
import Vuex from 'vuex'//引入vuex
Vue.use(Vuex)//挂载
var state1 ={
    count:1
}
var mutations={//定义一个方法 所有的全局方法都可以加在里面
    incCount(){//函数名
        ++state1.count;
    }
}
var state = new Vuex.Store({//实例化Vuex
    state:state1,//固定写法
    mutations//固定写法 通过this.$store.commit调用
})
export default state
~~~

#### 全局`main.js`引入

~~~js
import store from './store/index'
new Vue({
  store,
})
~~~

#### 全局组件调用数据

~~~js
 {{this.$store.state.count}}
~~~

#### 全局调用方法

~~~js
 <button @click="dianji"> 发送</button> 
  methods:{
        dianji(){
           this.$store.commit('incCount');//对应的是自建js文件里面的方法
        }
    }
~~~

##	Getters方法

~~~js
import Vue from 'vue'
import Vuex from 'vuex'//引入vuex
Vue.use(Vuex)//挂载
var state1 ={
    count:1
}
var mutations={//定义一个方法 所有的全局方法都可以加在里面
    incCount(){//函数名
        ++state1.count;
    }
}
var getters={//通过this.$store.getters.computedCount调用
    computedCount(){//当state里面数据发生变化后自己自动计算的值
        return state1.count*20//必须return
    }
}
var state = new Vuex.Store({//实例化Vuex
    state:state1,//固定写法
    mutations,//固定写法 通过this.$store.commit调用
    getters//挂载getters
})
export default state
~~~

#### 调用

~~~js
<h1>Getters数据：{{this.$store.getters.computedCount}}</h1>
~~~

![](https://cdn.xiaohuwei.cn/2019/06/2606598376.png)



## Action异步方法（ajax，计时器）

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

~~~js
import Vue from 'vue'
import Vuex from 'vuex'//引入vuex
Vue.use(Vuex)//挂载
var state1 ={
    count:1
}
var mutations={//定义一个方法 所有的全局方法都可以加在里面
    incCount(){//函数名
        ++state1.count;
    }
}
var getters={//通过this.$store.getters.computedCount调用
    computedCount(){//当state里面数据发生变化后自己自动计算的值
        return state1.count*20//必须return
    }
}
var actions = {//定义actions
    memeda(context){//形参必写！！！ 通过this.$store.dispatch('memeda')调用
        context.commit('incCount');//执行上面的方法 固定写法
    }
}
var state = new Vuex.Store({//实例化Vuex
    state:state1,//固定写法
    mutations,//固定写法 通过this.$store.commit调用
    getters,//挂载getters
    actions//挂载actions
})
export default state
~~~

####	调用

~~~js
<button @click="aaa"> Actions</button> 
methods:{
        aaa(){
            this.$store.dispatch('memeda')
        }
    }
~~~

---------------

vuex提供了一些非常方便的辅助函数，比如mapState、mapGetter、mapMutation、mapAction。初学vuex时，并未深究以上函数，只是走马观花的看了看。

## 暴露问题

在使用vuex的时候，仅仅了解State、Getter、Mutation、Action、Module等概念，在使用过程中，业务功能逐渐增加，会出现很多个状态。当一个组件需要多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。

## 辅助函数

##### mapState

引入

```js
import { mapState } from 'vuex
import { mapGetter } from 'vuex
import { mapMutation } from 'vuex
import { mapAction } from 'vuex
```

3种用法

- 对象 

  ```js
  computed: mapState({
    // 箭头函数
    count: state => state.count,
    // 这里为了能够使用this获取局部变量localCount，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
  ```

如果使用箭头函数，当vuex的state和当前组件的state，含有相同的变量名称时，this获取的是vuex中的变量，而不是局部变量

- 数组
   当映射的计算属性的名称与state的子节点名称相同时，我们也可以给mapState传一个字符串数组。

```js
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```

- 对象展开运算符
   mapState函数返回的是一个对象。如果需要将它与局部计算属性混合使用，通常我们需要一个工具函数将多个对象合并为一个，以使我们可以将最终对象传给computed属性。

```js
computed: {
    localComputed () {},
  ...mapState([‘数据名’])
}
但前提是映射的计算属性的名称与state的子节点名称相同，如果state在vuex的modules中，则不成功。相当于父子组件传值的props 可以在页面直接使用
```

##### mapGetters

mapGetters将store中的getter映射到局部计算属性

```js
computed: {
  ...mapGetters([
    'oneGetter',
    'anotherGetter'
  ])
}
```

##### mapMutations

使用mapMutations辅助函数将组件中的methods映射为store.commit调用。

```js
methods: {
  // 将this.tips映射成 this.$store.commit('tips')
  ...mapMutations(['tips'])
}
```

##### mapAction

使用mapActions辅助函数将组件的methods映射成store.dispatch调用

```js
methods: {
  // 将this.tips映射成 this.$store.dispatch('tips')
  ...mapActions(['tips'])
}
```

