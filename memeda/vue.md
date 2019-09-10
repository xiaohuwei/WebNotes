# vue 起步

!>**Vue.js**（[/vjuː/](https://zh.wikipedia.org/wiki/Help:%E8%8B%B1%E8%AA%9E%E5%9C%8B%E9%9A%9B%E9%9F%B3%E6%A8%99)，或简称为**Vue**）是一个用于创建用户界面的[开源](https://zh.wikipedia.org/wiki/%E5%BC%80%E6%BA%90%E8%BD%AF%E4%BB%B6)[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)框架，也是一个创建[单页应用](https://zh.wikipedia.org/wiki/%E5%8D%95%E9%A1%B5%E5%BA%94%E7%94%A8)的[Web应用框架](https://zh.wikipedia.org/wiki/Web%E5%BA%94%E7%94%A8%E6%A1%86%E6%9E%B6)[[4\]](https://zh.wikipedia.org/wiki/Vue.js#cite_note-4)。 2016年一项针对JavaScript的调查表明，Vue有着89%的开发者满意度。[[5\]](https://zh.wikipedia.org/wiki/Vue.js#cite_note-stateofjs-5) 在[GitHub](https://zh.wikipedia.org/wiki/GitHub)上，该项目平均每天能收获95颗星，[[5\]](https://zh.wikipedia.org/wiki/Vue.js#cite_note-stateofjs-5)[[6\]](https://zh.wikipedia.org/wiki/Vue.js#cite_note-6)为Github有史以来星标数第3多的项目。[[7\]](https://zh.wikipedia.org/wiki/Vue.js#cite_note-7)

> vue  移动端hybird 框架    混合app框架

> 安卓+ iOS  跨平台的套壳程序

> vue ： 适合中小型APP开发  react/angularJS：适合大型APP开发



### SPA:single page application ( 单页应用程序 )

- 渐进式
  - 渐进增强  （向上兼容）
  - 优雅降级  （向下兼容）
- **mvc设计理念**
  - **m** model 模型（数据层）
  - **v** view试图 （模板）
  - **c ** controller 控制层（业务层）

> 实际开发中，如果使用了vue之后，尽可能不操作dom，不使用jq js等。

##		vue指令

> 操作指定元素
>
> > 用法：放置在元素内部做属性

|          名字          |                         作用                         |
| :--------------------: | :--------------------------------------------------: |
|  `v-test="dataname"`   |                  等价于`innerText`                   |
|   `v-html="elname"`    |             将原来的节点替换为声明的节点             |
|    `v-show="true"`     |                 改掉元素的`display`                  |
|     `v-if="true"`      |   跟`v-show`一样的功能 但是会删掉节点//`remove()`    |
|        `v-else`        |                   跟`v-if`组成逻辑                   |
|      `v-else-if`       |                   跟`v-if`组成逻辑                   |
| `v-for="(v,k) in arr"` |      循环遍历指令 值：`{{ v }}`  键：`{{ k }}`       |
|      `v-on`or`@`       | 绑定事件`v-on:事件名="函数名"`  or `@click="函数名"` |
|  `v-bind:属性 `or `:`  |                动态绑定一个或多个属性                |
|  `v-model:"msg"`or`#`  |      双向数据绑定 只能给表单元素 指向`value`值       |
|        `v-pre`         |                  忽略解析 原样输出                   |
|       `v-cloak`        |             解决数据被解析之前的闪烁问题             |
|        `v-once`        |                      只渲染一次                      |













































