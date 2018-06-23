---
layout: post
title: 使用元素Ajax实现信息读取
description: 写的是如何利用js+php+Ajax实现信息的读取加载
categories:
 - Ajax
tags: Ajax MySQL JavaScript php
---
> 这里通过利用ajax读取`.txt`,`json`,`github接口`,`数据库`的数据来了解ajax的应用

## 请求纯文本（.txt）数据
1. 两种状态码的含义：
```
    //readyState状态码
    //0：请求未初始化
    //1：服务器链接已建立
    //2：请求已接收
    //3：请求处理中
    //4：请求已完成，且响应已就绪
    //
    //
    //HTTP(status)状态码
    //200：服务器成功返回网页
    //404：请求的网页不存在
    //503：服务器暂时不可用
```
2. 创建一个完整的AJax请求的三个步骤：
```
    //第一步：创建一个XMLHttpRequest对象
    var xhr=new XMLHttpRequest();

    //第二步：.open()指定发送方式及所请求数据的URL地址
    xhr.open('GET','sample.txt',true);
    console.log("READYSTATE:",xhr.readyState);//这里可以监听到readystate状态码 1

    //onprogress来监听进程
			xhr.onprogress=function(){
				console.log("测试：");
				console.log("READYSTATE:",xhr.readyState);
				//这里可以放我们加载完成之前所希望浏览器做的事情
			}



    //第三步：设置请求方式：onload()或onreadyStatechange()方法
    //onload()方法一调用就代表readyState状态码一定是4，意味着监听不到2和3了
    //onreadystate()方法就能够监听到状态码2和3
    xhr.onreadystatechange=function(){
				console.log("READYSTATE:",xhr.readyState);//这里可以监听到readystate状态码 2，3，4
				if(this.status==200&&this.readyState==4){
					console.log(this.responseText);
					document.getElementById("text").innerHTML=this.responseText;
				}else if(this.status==404){
					console.log("请求的网页不存在");
					document.getElementById("text").innerHTML="所请求的网页不存在";
				}
			}

    //调用请求方法代表的是我们请求数据成功后所要执行的操作


    //第四步：发送请求
    xhr.send();
```

## 请求json数据
1. 请求单个json和json数组的数据：
```
    //它们步骤集合一样，不同的地方在于如何将json数据读出
    var xhr=new XMLHttpRequest();
			xhr.open("get","user.json",true);
			xhr.onload=function(){
				if(this.status==200){
					// console.log(this.responseText);
					//我们返回的其实是一个json数据，要想使用的话还需要将其解析成一个js对象
					var user=JSON.parse(this.responseText);
					console.log(user.email);
					var output="";
					//使用es6语法模板进行拼接
					output+=`
						<ul>
							<li>${user.id}</li>
							<li>${user.name}</li>
							<li>${user.email}</li>
						</ul>
					`;
					document.getElementById('user').innerHTML=output;
				}
			}
			xhr.send();

	//读取json数组：
	var xhr=new XMLHttpRequest();
			xhr.open("get","users.json",true);
			xhr.onload=function(){
				if(this.status==200){
					// console.log(this.responseText);
					//我们返回的其实是一个json数据，要想使用的话还需要将其解析成一个js对象
					var users=JSON.parse(this.responseText);
					console.log(users.email);
					var output="";
					//遍历数组
					for (var i in users) {
						output+=`
							<ul>
								<li>${users[i].id}</li>
								<li>${users[i].name}</li>
								<li>${users[i].email}</li>
							</ul>
						`;
					}
					document.getElementById('users').innerHTML=output;
				}
			}
			xhr.send();

```
2. 新的知识：使用es6语法模板进行拼接
```
output+=`
	<ul>
		<li>${users[i].id}</li>
		<li>${users[i].name}</li>
		<li>${users[i].email}</li>
	</ul>
`;
```
## 请求GitHub数据接口
1. 步骤：
```
var xhr=new XMLHttpRequest();
			xhr.open("Get","https://api.github.com/users",true);
			xhr.onload=function(){
				var users=JSON.parse(this.responseText);

				var output='';
				for (var i in users) {
					output+=`
						<div class="user">
							<img src="${users[i].avatar_url}" width="70" height="70"/>
							<ul>
								<li>ID:${users[i].id}</li>
								<li>LOGIN:${users[i].login}</li>
							</ul>
						</div>
					`;
				}
				document.getElementById('users').innerHTML=output;
			}
			xhr.send(null);
```
2. 所获取知识：
* 第一次知道它们所说的API接口原来是这样用的，是为我们调取数据等服务的。
## 利用Ajax请求php接口
1. 利用post方法请求php中的数据
```
function postForm(e){
			//表单提交时会有默认刷新的事件
			//e是一个默认事件对象
			e.preventDefault();
			var name=document.getElementById("name2").value;
			//因为post方法的参数不能直接传递，所以需要额外定义
			var params="name="+name;
			var xhr=new XMLHttpRequest();
			xhr.open("post","process.php",true);
			//使用post的话就必须设置请求头
			xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded;');
			xhr.onload=function(){
				if(this.status==200){
					console.log(this.responseText);
				}else{
					console.log("没有页面");
				}
			}
			//这里发送的是post的参数
			xhr.send(params);
		}
	//注意事项：
	1.post的参数不能像get方法一样直接发送，所以需要额外定义。
	2.使用post的话必须额外设置请求头
	3.send（）方法里面发送的是我们额外定义的参数

	4.php中echo 输出的内容就是所返回的数据。只能有一个echo有效，多个的话会报错。所以我们需要对返回的数据进行拼接等处理。

```
2. 数据返回及处理形式：
```
    1. php输出json形式的数据：echo json_encode($data);
    2. 在输出界面对json格式的数据处理：var users=JSON.parse(this.responseText);
```
## 将数据利用Ajax实时的写入数据库
1. [HTML页面](https://github.com/lanxue-of-33/base-of-php/blob/master/Ajaxsandbox/Ajax5.html)
   [php页面](https://github.com/lanxue-of-33/base-of-php/blob/master/Ajaxsandbox/process.php)
2. 最重要的步骤在与如何将数据传入到php文件中并读取出来（这里利用的是get和post传递数据)

## 使用Ajax实时的读取数据库中的数据
1. [HTML页面](https://github.com/lanxue-of-33/base-of-php/blob/master/Ajaxsandbox/Ajax6.html)
   [php页面](https://github.com/lanxue-of-33/base-of-php/blob/master/Ajaxsandbox/users.php)
2. php中所返回数据的格式处理：
```
//这里利用的是自定义类来存储我们所读取的信息
class User
	{
		
		public $id;
		public $username;
	}
	$data=array();
	while ( $users=mysqli_fetch_array($result,MYSQLI_ASSOC)) {
		$user=new User();
		$user->id=$users['id'];
		$user->username=$users['username'];
		$data[]=$user;
	}


	echo json_encode($data);
```
---
[所有代码链接](https://github.com/lanxue-of-33/base-of-php/blob/master/Ajaxsandbox)

