## 解构对象

~~~js
let {name:a,user:b}={name:'李竹',user:'人类'}
document.write(a,b);//李竹人类
~~~

## 解构扩展运算符  `...`

```js
const arr =[1,2,3];
arr.push(4);
var max = Math.max(...arr);//...解构扩展运算符 arr-> 1,2,3,4
console.log(max);//4
var one = [1,2,3];
var two = [4,5,6];
var three = [7,8,9];
var five = [...one,...two,...three];
console.log(five)//(9) [1, 2, 3, 4, 5, 6, 7, 8, 9]
var obj1 = {name:"李竹"};
var obj2 = {user:'傻逼'};
var obj3 = {...obj1,...obj2};
console.log(obj3);//{name: "李竹", user: "傻逼"}
```

## arguments

>动态的接受所有的参数

~~~js
function memeda(){
    console.log(arguments)//arguments动态接受所有的参数 
}
memeda(100,200,300);//100，200，300
const arr =[1,2,3];
function sum (){
   var sum=0;
   for(i=0;i<arguments.length;i++){
       sum+=arguments[i]
   }
   console.log(sum);
}
sum(...arr)//1+2+3=6
~~~

## 函数默认参数问题

####	Es5

~~~js
function es5(a,b){
    var a=a||1;
    var b= b||2;
    console.log(a,b);
}
es5(100,200);//100,200
~~~

#### Es6

~~~js
function es6(a=1,b=2){
    console.log(a,b)
}
es6(100,200)//100,200
~~~

## Es6简写函数

~~~js
var func = a => {//有一个参数 小括号可以省略
    console.log(a);
}
func(8);//8 

var sum1 = (a,b)=>{
    return a+b;
}
console.log(sum1(10,20));//30
//简写 返回值只有一行代码时候 大括号和return都可以省略
var sum2 = (a,b)=>a+b;
console.log(sum2(10,20));//30
var memeda=a=>{
    return {
        name:a
    }
}
console.log(memeda('李竹'));//{name:'李竹'}
~~~

### Es6数组方法

#### *forEach 遍历*

~~~js
let arr = [1,2,3];
arr.forEach((v,k)=>{//遍历数组
    console.log(v,k)
})//1 0 2 1 3 2
//简写
arr.forEach((v,k)=>console.log(v,k))//1 0 2 1 3 2
arr.forEach(v=>console.log(v));//1,2,3
~~~

#### *Map 映射*

~~~js
//map  映射 主要用于权限控制  vip和Svip不同的功能可以通过这个方法 
//也可以实现遍历
let arr2 = [10,20,30,80,100];
let arr3 = arr2.map((v,k)=>{
    if(v>60){
        return true;
    }else{
        return false;
    }
})
console.log(arr3);//[false, false, false, true, true]
let arr4 = arr2.map(v=>v>60);//简写
console.log(arr4);//[false, false, false, true, true]
~~~

#### *filter 过滤（好东西）*

~~~js
//filter 过滤
let arr5 = [{name:'memeda',age:21},{name:'lizhu',age:20},{name:"heheda",age:18}];
let memeda = arr5.filter(v=>v.age>19)
console.log(memeda);//[{name:'memeda',age:21},{name:'lizhu',age:20}]
~~~

#### *callback 回调函数*

~~~js
//回调函数
let memedae = ()=>{
    console.log(1)
}
setInterval(memedae,1000)//不需要写() 自动执行11111111
~~~

#### *every 判断是否全部满足（全选反选）*

~~~js
//every  判断是否者全部满足 如果全部满足返回true
let arr10 = [10,20,30,80,100];
let xixi = arr10.every(v=>v>9);
console.log(xixi)//ture 
let arr11 = [10,20,30,80,100];
let xixixi = arr11.every(v=>v>10);//有一个元素 10 不满足
console.log(xixixi)//false
~~~

#### *some 判断是否有一个满足判断条件（购物车结账有一个被选中则可以付款）*

~~~js
let arr18 = [10,20,30,80,100];
let la = arr18.some(v=>v>90);//100>90
console.log(la)//ture
let lala = arr18.some(v=>v>100);//没有元素大于100
console.log(lala)//false
~~~

#### *reduce 数组各元素求和求乘积和平均数等*

~~~js
let arr99 = [10,20,30,40];
let arr100 = arr99.reduce((sum,v,k)=>{
console.log(`第${k}次：${sum}+${v}`);//第1次：10+20 第2次：30+30 第3次：60+40
return sum + v;
})
console.log(arr100);//100

let arr88 = [10,80,200,32];
let arr888 = arr88.reduce((sum,v,k)=>(sum + v)/arr88.length);//平均数
let max = Math.max(...arr88);//最大值
let min = Math.min(...arr88); //最小值
console.log(arr888,max,min);//21.90625 200 10
~~~

####  *Es6函数传参 解构赋值*

~~~js
//对象的解构
let obj8 = {name:"性感李竹",user:"在线发牌"}
let heheda = ({name,user})=>{
console.log(name,user);//性感李竹 在线发牌
}
heheda(obj8);
//数组的解构
let arr666 = [{name:"性感李竹",user:"在线发牌"},{name:"性感李竹",age:80}];
let get = (obj1,obj2)=>{
    console.log(obj1);//{name: "性感李竹", user: "在线发牌"}
    console.log(obj2);//{name: "性感李竹", age: 80}
}
get(...arr666)
//二次解构
let arr666 = [{name:"性感李竹",user:"在线发牌"},{name:"性感李竹",age:80}];
let get = ({name},{age})=>{
    console.log(name);//性感李竹 
    console.log(age);//80
}
get(...arr666)
~~~

#### *Es5和Es6数组去重*

~~~js
var arr567 = [1,2,3,4,5,6,7,8,9,1,2,3,4];
var arr654 =[];
//set es6数据类型 不允许值重复
//from 将一个可以迭代的数据对象 转换为数组
var newarr = Array.from(new Set(arr567));//es6第一种
console.log(newarr);//[1, 2, 3, 4, 5, 6, 7, 8, 9]
let arr369 = [...new Set(arr567)]//es6第二种 解构去重 
console.log(arr369);//[1, 2, 3, 4, 5, 6, 7, 8, 9]
//es5去重
for(var i = 0; i<arr567.length;i++){
        if(arr654.indexOf(arr567[i])==-1){
            arr654.push(arr567[i])
        }
}
console.log(arr654);//[1, 2, 3, 4, 5, 6, 7, 8, 9]
~~~

#### *Es6的 Promise*

~~~js
{
    let a = (callback)=>{
        console.log(callback)//第一个被打印
        setTimeout(() => {//模仿异步
            callback&&callback.call()//最后打印 .call()不带参数代表执行本身
        }, 1000);
    }
    a(()=>console.log("我是回调"))
    a(ax)
    function ax(){
        console.log("我也是回调")
    }
    console.log(3)   //第二个被打印
}
~~~

~~~js
{//作用域生效区间
    let a = (num) =>{
        return new Promise((res,rej)=>{
            if(num>10){
                res("我是比10大的数，成功！")
            }else{
                rej("我是比10小的数，失败！")
            }
        })
    }
    a(11).then(res=>console.log(res),rej=>console.log(rej));//我是比10大的数，成功！
}

//es6优化为一句话 三木运算
{
    let b = (num) =>new Promise((res,rej)=>num>10?res("我是比10大的数，成功！"):rej("我是比10小的数，失败！"));
    b(8).then(res=>console.log(res),rej=>console.log(rej));//我是比10小的数，失败！
}
~~~

####  *封装 jq的Ajax*

~~~js
{
let getData =(url,ops={},type="get")=>{//默认get
   return promiseObj = new Promise((resolve,reject)=>{
            $.ajax({
                    type,
                    url,
                    data:ops,
                    async:true,
                    dataType:"json",
                    success(res){
                        resolve(res);
                    },
                    error(res){
                        reject('失败了');
                    }
                });
    });
}
//调用
getData('https://api.xiaohuwei.cn/toutiao/index.php',{type:"news_food"})
.then(res=>console.log(res.data),res=>console.log(res))
}
~~~

#### *Promise链式调用*

封装后的用法

~~~js
{
let memeda1 =(url)=>{
   return new Promise((res,rej)=>{
    $.ajax({
        url,
        dataType:'json',
        success:(result)=>{
            res(result)
        }
    })
})
}

//链式调用
memeda1('https://api.xiaohuwei.cn/toutiao/type/index.php')
.then(res=>{
    console.log(res.typelist);
    return memeda1('https://api.xiaohuwei.cn/toutiao/index.php?type='+res.typelist[19].typeid)//将第一次拿到的值传到下一个需要使用.then方法的Promise对象并return
})
.then(res2=>{
    console.log(res2.data);
})
}
~~~

#### 第一种

> 直接在第一个Promise 对象的  `.then`  return 下一个要使用  `.then`  的Promise 对象

~~~~js
let  step1 =(resolve, reject)=> {
            console.log('看天气 要不要出去买菜')
            var flag = prompt('请输入今天的天气')
            if (flag == '晴') {
                resolve('我就去买菜做饭')
            } else {
                reject('天气不好 不想动')
            }
        }
let step2 = (resolve, reject)=> {
            console.log('买菜完成')
            var flag = prompt('确认家里有没有煤气啊')
            if (flag == '有') {
                resolve('我就去洗菜做饭')
            } else {
                reject('那我去冲煤气')
            }
        }

let  step3 = (resolve, reject)=> {
            console.log('做饭完成')
            var flag = prompt('我做的饭好吃吗?嘻嘻')
            if (flag == '好吃') {
                resolve('味道好  明天你继续做饭')
            } else {
                reject(' 不吃了  给我去跪键盘 ')
            }
        }
let step4 =  (resolve, reject) => {
            console.log('吃饭完成')
            var flag = prompt('今天该谁洗碗？')
            if (flag=='我') {
                resolve('表现不错 ')
            } else {
                reject('明天没有零花钱 ')
            }
        }
let  p= new Promise(step1).then(res=>{
             console.log(res)
              return new Promise(step2)
         }).then(res=>{
             console.log(res)
             return new Promise(step3)
         },err=>{
             console.log(err)
         }).then(res3 =>{
             console.log(res3)
             return new Promise(step4)
         },
        err3 => console.log(err3))
             .then(res4=>{
                 console.log(res4)
    },err=>console.log(err))
~~~~

效果图

![](https://cdn.xiaohuwei.cn/2019/06/3725410418.png)

#### 第二种(带参数的解决异步回调地狱)

~~~js
let memeda1 =()=>{
   console.log('这是第一层');
   return new Promise((res,rej)=>{
    let Tip = prompt('输入memeda');//这里默认memeda
    if(Tip=='memeda'){//如果Tip不为空
        res(Tip);//成功返回Tip
    }else{
         rej('第一层失败标识');
    }
})
}

let memeda2 =(data)=>{
    console.log('我是第二层 这是第一层你输入的数据:'+data);
   return new Promise((res,rej)=>{
    let Tip = prompt('随便heheda');//默认heheda
    if(Tip=='heheda'){//如果Tip不为空
        res(Tip);
    }else{
         rej('第二层失败标识');
    }
})
}

let memeda3 =(data)=>{
    console.log('我是第三层 这是第二层你输入的数据:'+data);
    return new Promise((res,rej)=>{
        res('第三层成功标识');
         rej('第三层失败标识');
})
}
//链式调用
memeda1().then(res=>{
    console.log('第一次拿到的值'+res);
    return memeda2(res)//将第一次输入的值传过去  return整个memeda2以便下次.then
})
.then(res2=>{
    console.log('第二次拿到的值'+res2);
    return memeda3(res2)//将第二次输入的值传过去  return整个memeda3以便下次.then
})
.then(res3=>console.log(res3),res3=>console.log('第三次失败了？'))
~~~

效果图

![](https://cdn.xiaohuwei.cn/2019/06/994696962.png)

#### *Promise的.all方法*

> 此方法必须所有的数据都要请求成功，只要有一个失败，全部失败。

~~~js
{
let memeda1 =(url)=>{
   return new Promise((res,rej)=>{
    $.ajax({
        url,
        dataType:'json',
        success:(result)=>{
            res(result)
        }
    })
})
}
let memeda2 =(url)=>{
   return new Promise((res,rej)=>{
    $.ajax({
        url,
        dataType:'json',
        success:(result)=>{
            res(result)
        }
    })
})
}
//链式调用
Promise
.all([memeda1('https://api.xiaohuwei.cn/toutiao/type/index.php'),memeda2('https://api.xiaohuwei.cn/toutiao/index.php')])
// .then(res=>{
//     console.log(res);//合并结果打印
// })
.then(([arr1,arr2])=>{//拆开数据单独打印
    console.log(arr1);
    console.log(arr2);
}).catch(res=>{//打印所有错误请求
    console.log(res);
})
}
~~~

#### *Promise的.race方法（和.all用法一样）*

> 只要一个数据错误 后面的都会错误 如果第一个成功了 后面的全部自动置为成功

#### *Promise的.catch方法*

>可以捕获所有错误 在最后一个.then后面加上

~~~js
{
let memeda =(url)=>{
   return new Promise((res,rej)=>{
    $.ajax({
        url,
        dataType:'json',
        success:(result)=>{
            res(result)
        },
        error:(result)=>{
            rej(result)
        }
    })
   })

}
//链式调用
Promise
.all([memeda('2.json'),memeda('https://api.xiaohuwei.cn/toutiao/index.php')])
// .then(res=>{
//     console.log(res);//合并结果打印
// })
.then(([arr1,arr2])=>{//拆开数据单独打印
    console.log(arr1);
    console.log(arr2);
}).catch(err=>{
    console.log(err);//打印所有错误请求
})
}
~~~

###  Es7的async和await 解决异步线程独立

> 效果：可以从上至下顺序执行了 不用担心异步的线程独立

~~~js
    {
        //await 后面跟Promise 配合async使用 解决异步数据问题 
       let memeda = async ()=>{
        let a = '么么哒';
        let b = await new Promise((res,rej)=>res('小可爱'));
            console.log(a+b);
        } 
        memeda();//么么哒小可爱
    }
~~~

#### await 配合ajax

~~~js
{
let memeda = (url)=>{
  return new Promise((res,rej)=>{
    $.ajax({
        url,
        dataType:'json',
        success:(result)=>{
            res(result)
        }
        })
         })
     }
let xixi  = async () =>{
    let res = await memeda('https://api.xiaohuwei.cn/toutiao/type/index.php');//拿第一个ajax的值
    let data = res.typelist[19].typeid;//拿到第一个ajax数据里面分类19的typeid
    console.log(data)//news_food 美食分类
     //用上一个的数据做下一个ajax的参数发请求
    let list = await memeda(`https://api.xiaohuwei.cn/toutiao/index.php?type=${data}`);
    console.log(list.data);
}
xixi()
}
~~~

#### *es6函数和数据导出*

~~~js
// 1.js
export let one = 1 ;
export let two = 2 ;
export let three = 3 ;
//函数导出 2.js
export let get = ()=>console.log('我是get函数');
export let memeda = ()=>console.log('我是memeda函数');
//3.js
export let get1 = ()=>console.log('我是get1函数');
export let memeda1 = ()=>console.log('我是memeda1函数');
// 4.js
export let love = 3 ;
let obj = {"name":"张三"}
let obj1 = {"age":18}
export default{obj,obj1}
//5.js
export let lalala = 3 ;
export let keai = '可爱' ;
let obj = {"name":"张三"}
let obj1 = {"age":18}
let heheda = ()=>console.log('我是5.js的函数heheda');
export default{obj,obj1,heheda}
~~~

#### *函数和数据的导入和使用*

~~~js
//如果导出的是export let a = 1 这种类型; export一个页面可以有无数个
//那么引入必须带上{} 建议导入导出用一个名字
//如果是 exprot default 的直接取名 一个页面只能有一个exprot default
import {one,two,three} from './1.js'
import {get,memeda} from './2.js'
import * as type from './3.js'//全部导入
import four,{love} from './4.js'
import five,{lalala,keai} from './5.js'
console.log(one);
console.log(two);
console.log(three);
get();//我是get函数
memeda();//我是memeda函数
type.get1();//我是get1函数
type.memeda1()//我是memeda1函数
console.log(four.obj.name)//张三
console.log(four.obj1.age)//18
console.log(love);//3
console.log(lalala,keai)//3 "可爱"
console.log(five.obj.name,five.obj1.age)//张三 18
five.heheda();//我是5.js的函数heheda
~~~

![](https://cdn.xiaohuwei.cn/2019/06/2919691965.png)

### 多线程按顺序执行封装

~~~js
        function yibu() {
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    resolve('我是异步函数执行后的数据')
                    //  reject('我是错误的')
                }, 2000);
            })
        }
       
var obj = {
            name: 'zhangsan',
            age: 18,
            showName: function () {
                console.log(this.name)
            },
            showName2: async () => {
                console.log('showName2异步方法')
                let res = await yibu()
                console.log(res)
                //res 后面所有的数据必须等await 执行完后才会执行
                console.log('执行后的数据2')
            },
            showName4: async () => {
                console.log('showName4异步方法')
                let res = await yibu()
                console.log(res)
                //res 后面所有的数据必须等await 执行完后才会执行
                console.log('执行后的数据4')
            },
            showName5: async () => {
                console.log('showName5异步方法')
                let res = await yibu()
                console.log(res)
                //res 后面所有的数据必须等await 执行完后才会执行
                console.log('执行后的数据5')
            },
            p: async () => {
                await obj.showName2()
                await obj.showName4()
                await obj.showName5()
            }
        }

        //代码以同步的形式去写 异步执行
        export default {
            obj
        }

~~~

