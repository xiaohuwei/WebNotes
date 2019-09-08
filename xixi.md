##		封装连接数据库函数

```php
链接数据库   charset 默认为utf8  可选参数
      function conn($host,$user,$password,$database,$charset="utf8"){
      	$link = mysqli_connect($host, $user, $password, $database);
	    	mysqli_set_charset($link, $charset);
        return $link;
      }
调用方法
    	$link = conn("127.0.0.1", "root", "root", "k8911");
```

##		封装查询函数

```php
查询数据库 $link:查询函数返回的结果  $cols：默认为* 可以为字段名 $group：分组 $order:排序 $limit：分页  function select($link,$tbName,$cols="*",$where="",$group="",$order="",$limit=""){
	  // 传递 条件
	  if($where != ""){
		$where = " where ".$where;
	  }
	    if($group != ""){
		$group = " group by ".$group;
	  }
	      if($order != ""){
		$order = " order by ".$order;
	  }
	      if($limit != ""){
		$limit = " limit ".$limit;
	  }
	  // 组装select 语句
	 $sql = "select $cols from $tbName $where $group $order $limit";
	 //echo $sql;
	  $result =  mysqli_query($link, $sql);
	  if($result === flase){
	  	exit("查询语句有误!!!");
	  }else{
		   $arr = mysqli_fetch_all($result,MYSQLI_ASSOC);
	  }
	     mysqli_free_result($result);
		 return $arr;  // 返回数据
  }
调用方法
$arr= select($link, "stus","*",null,null,null,"0,10");//查询stus这个表里面的从0开始的10条数据
```

##		封装删除数据函数

```php
封装删除数据函数  
function del($link,$tbName,$where){
  	//组装sql语句 
  	if($where == null){
  		$sql = "truncate $tbName";
  	}else{
  			$sql = "delete from $tbName  where $where";
  	}
	//echo $sql;
	  mysqli_query($link, $sql);
	  return  mysqli_affected_rows($link);
  }
调用方法
$del = ($link,stus,"");//清空stus里面所有数据 （truncate stus）
$del = ($link,stus,"id=1");//删除stus表里面id为1的数据
```

##		封装增加函数

```php
  function add($link,$tbname,$arr){
     $keys = array_keys($arr);  // [name,sex,cid]
     $cols = join(",", $keys);
     $vals = array_values($arr);  // [memeda,男] 
	 $values = "'".join("','", $vals)."'";
  	 $sql = "insert $tbname($cols) value ($values)";
	 //echo $sql;
	 mysqli_query($link, $sql);
	 return mysqli_affected_rows($link);
  }
调用方法
 	 $res = add($link,"stus",$_POST);//post传过来的数组
```

##		封装修改函数

```php
function save($link,$tbname,$arr,$where){
		   $str ="";
		  foreach($arr as $k=>$v){
			$str .= $k."='".$v."',";
		  }
		 $str =  trim($str,",");
		if($where !=null){
			$where = " where ".$where;
		}
  	 $sql = "update $tbname set $str $where";
	  mysqli_query($link, $sql);
	  return  mysqli_affected_rows($link); 
  }
	调用方法
        save($link,"stus",$_POST,"sid=$sid");
	
```

##	复杂语句处理封装

```php
function query($link,$sql){
		
		$arr = ["select","update","delete","insert"];
		// 过滤空格
		  $sql= trim($sql);  // trim(str,",")
		  $type= substr($sql, 0,6);
		  if(in_array($type, $arr)){
			    if($type == "select"){
			    	// 查询
			    	// @  错误抑制符
			    	@ $result = mysqli_query($link, $sql);  // false  obj
			    	if($result === false){
						exit("查询语句拼写有误!!!");
			    	}else{
				 	@ $arr =	mysqli_fetch_all($result,MYSQLI_ASSOC);
					return $arr;
			    	}
			    }else{
			    	// 增删改
			    	$re= mysqli_query($link, $sql); // true false
			    	if($re === false){
						exit("语句拼写有误!!");
			    	}else{
						 return mysqli_affected_rows($link);
			    	}
			    }
		  }else{
		  	
			 exit("sql语句非法!!!");
		  }
    }
调用方法
query($link,"select tabname * from xxx");
```
### 下载
[下载源代码](https://cdn.xiaohuwei.cn/2019/04/1987452264.zip)