### Typescript和Javascript的对比

> TypeScript 与JavaScript两者的特性对比，主要表现为以下几点：

?> TypeScript是一个应用程序级的JavaScript开发语言。（这也表示TypeScript比较牛逼，可以开发大型应用，或者说更适合开发大型应用）
?> TypeScript是JavaScript的超集，可以编译成纯JavaScript。这个和我们CSS离的Less或者Sass是很像的，我们用更好的代码编写方式来进行编写，最后还是有好生成原生的JavaScript语言。
?> TypeScript跨浏览器、跨操作系统、跨主机、且开源。由于最后他编译成了JavaScript所以只要能运行JS的地方，都可以运行我们写的程序，设置在node.js里。
?> TypeScript始于JavaScript，终于JavaScript。遵循JavaScript的语法和语义，所以对于我们前端从业者来说，学习前来得心应手，并没有太大的难度。
?> TypeScript可以重用JavaScript代码，调用流行的JavaScript库。
?> TypeScript提供了类、模块和接口，更易于构建组件和维护。

### 环境构建
```node
npm install typescript -g
mkdir filename
cd filename
npm init -y //初始化package.json
tsc --init//初始化tsconfig.json
npm install @types/node --dev-save//解决模块的声明文件问题。
```

## TypeScript中的数据类型

- Undefined :
- Number:数值类型;
- string : 字符串类型;
- Boolean: 布尔类型；
- enum：枚举类型；
- any : 任意类型，一个牛X的类型；
- void：空类型；
- Array : 数组类型;
- Tuple : 元祖类型；
- Null ：空类型。

## 定义变量

```tsx
//数值类型
var age:number = 18//Number 支持整数 浮点 和NAN
var stature:number = 178.5
console.log(age)
console.log(stature)
//字符串类型
var xiaohuwei:string = "xiaohuwei"
console.log(xiaohuwei)
//枚举类型
enum REN{ nan , nv ,yao}
console.log(REN.yao)  //返回了2，这是索引index，跟数组有点像。
enum REN{nan = '男',nv = '女',yao= '妖'}
console.log(REN.yao)  //返回了妖 这个字
//数组类型
var arr: number[] = [1,2,3]//由number组成的数组
var arr: Array<string> = ['test','memeda']//由字符串组成的数组
var arr: Array<boolean> = [true,false]//由布尔值组成的数组
var arr: [number,string] = [1,'test']//元祖类型
```



## 定义函数

```tsx
//普通定义
function searchXiaoJieJie(age:number):string{//string为该函数的返回值
    return '找到了'+age+'岁的小姐姐' 
}
var age:number = 18
var result:string = searchXiaoJieJie(age)
console.log(result)
//Es6定义
const add = (n1:number,n2:number):number=>{
    return n1+n2
}

console.log(add(1,4))
```

## 面向对象-类的声明和修饰符

```tsx
//类的声明 和修饰符
class XiaoJieJie2 {
  public sex: string; //公用属性修饰符
  protected name: string; //受保护的属性修饰符 外部不可用
  private age: number; //私有修饰符 外部不可用
  public constructor(sex: string, name: string, age: number) {
    this.sex = sex;
    this.name = name;
    this.age = age;
  }
  public sayHello() {
    console.log("小哥哥好");
  }

  protected sayLove() {
    console.log("我爱你");
  }
}

var jiejie2: XiaoJieJie2 = new XiaoJieJie2("女", "热巴", 22);

console.log(jiejie2.sex);
//console.log(jiejie2.name); //报错
//console.log(jiejie2.age); //报错
jiejie2.sayHello();
//jiejie2.sayLove(); //报错
```

## 面向对象 -类的继承和重写

```tsx
//类的继承和重写
class Jspang {
  public name: string;
  public age: number;
  public skill: string;
  constructor(name: string, age: number, skill: string) {
    this.name = name;
    this.age = age;
    this.skill = skill;
  }
  public interest() {
    console.log("找小姐姐");
  }
}

let jspangObj: Jspang = new Jspang("技术胖", 18, "web");
jspangObj.interest();//找小姐姐
//extends  关键字继承即可
class JsShuai extends Jspang {
  public xingxiang: string = "帅气";
  public interest() {
    super.interest(); //super关键字调用了父类的方法，实现了技能的继承。
    console.log("建立电商平台");
  }
  public zhuangQian() {
    console.log("一天赚了一个亿");
  }
}
let shuai = new JsShuai("技术帅", 5, "演讲");
shuai.interest();//找小姐姐 ,建立电商平台
shuai.zhuangQian();//一天赚了一个亿
```

## 面向对象-接口规范

### 数据的规范

```tsx
//接口定义规范！
//数据
interface Husband {
    sex:string
    interest:string
    maiBaoBao?:Boolean//可传可不传
}
let myhusband:Husband ={ sex:'男',interest:'看书、作家务',maiBaoBao:true}
console.log(myhusband)//{ sex: '男', interest: '看书、作家务', maiBaoBao: true }
```

### 函数的规范

```tsx
//函数
interface  SearchMan{
    (source:string,subString:string):boolean
}

let mySearch:SearchMan

mySearch = function(source:string,subString:string):boolean{
    let flag =source.search(subString)
    return (flag != -1)
} 

console.log(mySearch('高、富、帅、德','胖')) //false
```

