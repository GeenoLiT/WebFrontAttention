# 关于js和html中的单双引号问题

>1. 在js中项直接输出双引号或者单引号的话

	alert("'");
	alert('\'');
	alert('"');
	alert("\"");
	
>2.对于js事件中的参数：包含了单引号和双引号的话，需要特殊处理
	双引号全局替换为：&quot; , 单引号：\\' 转义处理(由于自己函数内部采用的是单引号进行包裹的
	所以这里只能使用转义字符进行显示)--->但是onclick=这里的双引号转义的话不行
  
  >解决方案：val.replace(/"/g, "&quot;").replace(/'/g, "\\'");
  
	$("body").append("<p onclick=\"sayHello('"+rebuildContent(result)+"')\">tttttttttttt</p>");
