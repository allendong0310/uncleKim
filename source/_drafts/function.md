---
title: 一些常用的方法
date: 2019-10-21 16:28:38
tags: work
categories: web
---


## 限制输入框只显示整数

尝试过很多的办法，最好用的正则的办法，虽然html5现在已经提供一些方法，但是在具体的项目中是远远不够的，判断的时候一般采取的办法
1.在失去焦点的时候判断。
2.在用户释放键盘按钮时执行，也就是 `onkeyup`。
3.在vue中放在@change事件中，交互不太友好，会在失去焦点的时候触发。


```bash
   <input type='text' onkeyup="value=value.replace(/^(0+)|[^\d]+/g,'')">
```

留个坑：做一套表单验证系统，现在是在submit按钮上验证，没有文字交互，不够友好。
暂有方案：
1.点击按钮时弹框提示所有未通过验证表单
2.点击提交按钮时错误表单提示
3.每个表单失去焦点（or onkeyup）时提示


## 一些常用命令

git设置http免密
`git config --global credential.helper store`

清除git记住的密码
`git config --system --unset credential.helper`

Linux中su、su - 的区别
- su 切换到root用户，但是并没有转到root用户家目录下，即没有改变用户的环境。
- su - 切换到root用户，并转到root用户的家目录下，即改变到了root用户的环境。

添加删除远程仓库
`git remote rm origin`
`git remote remove origin`
`git remote add origin <originUrl>`
关联后，使用命令`git push -u origin maste`r第一次推送master分支的所有内容;
`git remote rename <oldName> <newName>`

git 子项目 init
`git submodule update --remote projectName`

解决npm install cache问题
`npm cache verify`

## 一键粘贴

使用document.execCommand方法

当一个HTML文档切换到设计模式时，document暴露 execCommand 方法，该方法允许运行命令来操纵可编辑内容区域的元素。

`document.execCommand(aCommandName, aShowDefaultUI, aValueArgument)`
aCommandName
	一个 DOMString ，命令的名称。
aShowDefaultUI
	一个 Boolean， 是否展示用户界面，一般为 false。Mozilla 没有实现。
aValueArgument
	一些命令（例如insertImage）需要额外的参数（insertImage需要提供插入image的url），默认为null。

copy 代码

``` bash
	<div id="oDiv">1234567890</div>
	<textarea id="contents" rows="10" cols="10"></textarea>
	<button id="btn">点击</button>

	var btn = document.getElementById("btn");
	btn.onclick = function(){
		copyText();
	}
	function copyText(){ 
		var e=document.getElementById("contents");//获取textarea的id
		e.value = document.getElementById("oDiv").innerText;//把标签的文本内容赋值给textarea
		e.select(); //选择textarea的文本内容
		document.execCommand("Copy"); //执行浏览器复制命令
	}
```

具体的项目中可以隐藏 textarea

## 下载功能

通常情况下，下载都是直接用get请求。
但是在具体的项目中，有一个项目的认证是通过前端将约定好的cookies放在setRequestHeader里，直接get下载的时候会跳过认证。
所以写了原生的请求来解决这种情况

```bash
var exportForm = function(url, name) {
	var self = this;
	var xmlHTTP = new XMLHttpRequest();
	xmlHTTP.open('GET', url, true);
	xmlHTTP.setRequestHeader('Authorization', 'aaa');
	xmlHTTP.responseType = 'arraybuffer';
	xmlHTTP.onload = function(e) {
		var blob = new Blob([this.response]);
		var src = window.URL.createObjectURL(blob);
		var a = document.createElement('a');
		var filename = name + '.csv';
		a.download = filename;
		a.href = src;
		$('body').append(a);
		a.click();
		$(a).remove();	
	};
	xmlHTTP.send();
};
```
前端直接下载
```bash
	function download(filename, text) {
		var element = document.createElement('a');
		element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(text));
		element.setAttribute('download', filename);
		
		element.style.display = 'none';
		document.body.appendChild(element);
		
		element.click();
		
		document.body.removeChild(element);
	}
	
	
	download("hello.txt","This is the content of my file :)");
```

## 获取一段时间内每天的时间字符串

```bash
function getDateArray(startStr, endStr) {
    var dateArray = []
    function getDate(datestr){
      var temp = datestr.split("-");
      var date = new Date(temp.join('/'));
      return date;
    }
    var startTime = getDate(startStr);
    var endTime = getDate(endStr);
    while((endTime.getTime()-startTime.getTime())>=0){
      var year = startTime.getFullYear();
      var month = startTime.getMonth() +1
      month = month < 10?"0"+month:month;
      var day = startTime.getDate().toString().length==1?"0"+startTime.getDate():startTime.getDate();
      startTime.setDate(startTime.getDate()+1);
      dateArray.push(year+"-"+month+"-"+day)
    }
    return dateArray
}

getDateArray('2019/09/10','2019/09/19')
getDateArray('2019-09-10','2019-09-19')
getDateArray('2019-09','2020-10')
getDateArray('2019','2020')
```

## 遗留问题

想通过a标签的css伪类实现点击过的链接变成指定样式，刷新之后恢复正常，参考样式百度的搜索页面。
后来发现好像实现不了，就在vue中保存在store里。
留坑

## tab
tab这种东西在刚写jq的时候已经写栏了吧
这个实在一个非洲的项目中又重写的一个，因为怕vue太重，我们国内现在网络状态已经没有机会用了。
>https://codepen.io/allendong0310/pen/JjjEZJx

## 前端查询（vue）

>https://codepen.io/allendong0310/pen/yLLgRWq

## 一个ajax请求方法（待测试）

```bash
*参数说明：
*opts: {'可选参数'}
**method: 请求方式:GET/POST,默认值:'GET';
**url:    发送请求的地址, 默认值: 当前页地址;
**data: string,json;
**async: 是否异步:true/false,默认值:true;
**cache: 是否缓存：true/false,默认值:true;
 **contentType: HTTP头信息，默认值：'application/x-www-form-urlencoded';
   **success: 请求成功后的回调函数;
   **error: 请求失败后的回调函数;
 */

function ajax(opts){
//一.设置默认参数
	var defaults = {    
		method: 'GET',
		url: '',
		data: '',        
		async: true,
		cache: true,
		contentType: 'application/x-www-form-urlencoded',
		success: function (){},
		error: function (){}
	};    

	//二.用户参数覆盖默认参数    
	for(var key in opts){
		defaults[key] = opts[key];
	}

	//三.对数据进行处理
	if(typeof defaults.data === 'object'){    //处理 data
		var str = '';
		var value = '';
		for(var key in defaults.data){
			value = defaults.data[key];
			if( defaults.data[key].indexOf('&') !== -1 ) value = defaults.data[key].replace(/&/g, escape('&'));   //对参数中有&进行兼容处理
			if( key.indexOf('&') !== -1 )  key = key.replace(/&/g, escape('&'));   //对参数中有&进行兼容处理
			str += key + '=' + value + '&';
		}
		defaults.data = str.substring(0, str.length - 1);
	}

	defaults.method = defaults.method.toUpperCase();    //处理 method

	defaults.cache = defaults.cache ? '' : '&' + new Date().getTime() ;//处理 cache

	if(defaults.method === 'GET' && (defaults.data || defaults.cache))    defaults.url += '?' + defaults.data + defaults.cache;    //处理 url    

	//四.开始编写ajax
	//1.创建ajax对象
	var oXhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject('Microsoft.XMLHTTP');
	//2.和服务器建立联系，告诉服务器你要取什么文件
	oXhr.open(defaults.method, defaults.url, defaults.async);
	//3.发送请求
	if(defaults.method === 'GET')    
		oXhr.send(null);
	else{
		oXhr.setRequestHeader("Content-type", defaults.contentType);
		oXhr.send(defaults.data);
	}    
	//4.等待服务器回应
	oXhr.onreadystatechange = function (){
		if(oXhr.readyState === 4){
			if(oXhr.status === 200)
				defaults.success.call(oXhr, oXhr.responseText);
			else{
				defaults.error();
			}
		}
	};
}
```

## 监听函数(addEventListener)

>https://codepen.io/allendong0310/pen/oNNBQgX

