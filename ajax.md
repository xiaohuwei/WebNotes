!>AJAX即“Asynchronous JavaScript and XML”（异步的JavaScript与XML技术），指的是一套综合了多项技术的浏览器端网页开发技术。Ajax的概念由杰西·詹姆士·贾瑞特所提出。

###		 GET

```javascript
 $.get("https://text.xiaohuwei.cn/xs/svip.php",{key:"xiaohuwei"},(res)=>{
	res = JSON.parse(res); //将后端返回的json字符串变成json对象
	console.log(res);
	})
```

###		POST

```javascript
$.post("https://text.xiaohuwei.cn/xs/svip.php",{vip:"xiaohuwei"},(res)=>{
	res = JSON.parse(res); //将后端返回的json字符串变成json对象
	console.log(res);
	 })
```

###		JSONP（跨域问题）

```javascript
$.getJSON("https://douban.uieee.com/v2/movie/top250?callback=?",{},(res)=>{
     console.log(res);
	 })
```

###		ES6新语法（可能要jq3.X版本）

```javascript
  $.ajax({
			url:"https://text.xiaohuwei.cn/xs/svip.php",
			type:"get",
			dataType:"json",
			data:{key:"xiaohuwei"}
	}).done((res)=>{
		console.log(res)
	}).fail((res)=>{
		console.log(res)
	})
```

##		PROMISE ES6 语法封装通用ajax

```javascript
function getData(url,ops={},type="get"){//默认get
	//  es6 promise语法  封装通用异步
	var promiseObj = new Promise(function(data,reject){
			$.ajax({
		    		type,
		    		url,
		    		data:ops,
		    		async:true,
		    		dataType:"json",
		    		success:(res)=>{
		    		data(res);
		    		},
		    		error:function(res){
		    			reject(res);
		    		}
		    	});
	});
	   return promiseObj;
}
```

**调用方法**

```javascript
var obj = getData("https://text.xiaohuwei.cn/xs/svip.php?key=xiaohuwei");
  		     obj.then((res)=>{
					 console.log(res);
					 })
					 
var obj2 = getData("https://text.xiaohuwei.cn/xs/svip.php",{key:"xiaohuwei"});
  		     obj2.then((res)=>{
					 console.log(res);
  		     })
//post  		     
var obj3 = getData("https://text.xiaohuwei.cn/xs/svip.php",{vip:"xiaohuwei"},type="post");
  		     obj3.then((res)=>{
					 console.log(res);
  		     })
```

