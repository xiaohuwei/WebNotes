## 使用Gogs的Web钩子

> 场景就是，当我们使用`coding` 或者`Github`  或者`Gogs` 来协作开发的时候，我们希望在更新好了代码之后，实时的部署到线上环境，本教程针对，宝塔面板和`gogs` 来操作。 我们需要使用宝塔自带的`WebHook 1.0`,如图。

![image-20190910112255059](https://cdn.xiaohuwei.cn/2019/09/888390769.png)

脚本写上以下代码

```c++
#!/bin/bash
echo ""
#输出当前时间
date --date='0 days ago' " %Y-%m-%d %H:%M:%S"
echo "Start"
#判断宝塔WebHook参数是否存在
if [ ! -n "$1" ];
then 
          echo "param参数错误"
          echo "End"
          exit
fi
#git项目路径 宝塔自寻
gitPath="/www/wei/$1"
#git 网址
gitHttp="https://git.xiaohuwei.cn/xiaohuwei/WebNotes.git"

echo "Web站点路径：$gitPath"
cd $gitPath
git pull
echo '拉取成功'
fi
```

然后就是查看密匙获取完整的Url，比如我的就是

~~~
http://19.168.0.1:8888/hook?access_key=123&param=ko.xiaohuwei.cn
~~~

其中ip换成你的面板地址 `access_key ` 换成宝塔给你的`key` ，`param`  换成项目目录名字，保存下来后面要用。

然后我们需要手动清空`ko.xiaohuwei.cn`根目录下的所有文件，连接服务器，把你的仓库克隆过来。

![](https://cdn.xiaohuwei.cn/2019/09/2791171260.png)

然后我们需要吧自己的钩子配置到Gogs 

![](https://cdn.xiaohuwei.cn/2019/09/3627884341.png)

保存就可以了

![](https://cdn.xiaohuwei.cn/2019/09/92304701.png)

![](https://cdn.xiaohuwei.cn/2019/09/1959049157.png)

网站根目录已经完成更新了～

完结。

