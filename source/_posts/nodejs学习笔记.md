---
title: nodejs学习笔记
date: 2019-08-28 10:01:54
tags:
---

### 1.起步

---

#### 1.1 安装node环境

- 下载地址:[nodejs.org](https://nodejs.org/en/download '地址')
- 安装:一顿next
- 确认安装成功 `node -v`
- 

#### 1.2执行

- 用命令行执行js文件`node hellowWorld.js` 其中hellowWorld.js为执行的文件名，**命令行定位到文件所在的文件夹中**,文件命名不能是*node.js*

- 没有BOM,DOM

- nodejs具有读写文件的能力,引入fs,就能获取关于操作文件的api

  ```js
  var fs=require('fs');
  
  //1.读文件
  //	fs.readFile('文件路径',function(error,dada){
  	
  //})
  //成功
  //     data 数据   error  null
  //失败
  //     data undefinde  error  null
  fs.readFile('./hellow.txt',function(error,data){
  	if(data){
  		console.log(data.toString())//data是二进制数，所以要通过toString转换成字符串
  	}else {
  		console.log('未读取成功',error)
  	}
  })
  
  
  //2.写文件
  //fs.writeFile('路径'，'内容',function(error){
  //})
  //成功
  // 文件写入成功  error null
  //失败
  //  文件写入失败 error 错误对象
  var fs =require('fs');
  fs.writeFile('./你好.md','你好，这是我写入的md文件',function(error){
  	if(!error){
  		console.log('写入成功')
  	}else{
  		console.log('写入失败')
  	}
  })
  
  ```







---

### 2.创建服务器

------

#### 2.1加载http模块

创建一个server服务器，服务器提供对数据的服务。

- 服务器服务流程
  - 前端向服务器发请求
  - 服务器接收请求
  - 处理请求
  - 反馈请求

```js
var http = require('http');//引入服务器模块

var server=http.createServer();//创建服务器

//Request是客户端发送的一些请求信息，比如路径等
//Response响应对象，可以给用户发送响应信息
server.on('request',function(Request,Response){
	console.log("服务器被请求了",Request.url)
	var outPut={a:'222',b:'333'};
	switch(Request.url){
		case '/hello':
		outPut.a='hello'
		break;
		case '/login':
		outPut.a='登录';
		break;
		case '/register':
		outPut.a='register';
		break;
		default:
		outPut.a='默认值'
	}
	//Response.write('ssss')可以使用多次，但是必须end
	console.log(outPut)
	Response.end(JSON.stringify(outPut));
})//监听服务器请求


//启动服务器，并创建5000端口号
server.listen(3000,function(){
	console.log('服务器启动成功了')
})
```

运行命令node httpCreate.js命令行进入监听状态，当用浏览器访问`locahost:5000`时，命令行打印出服务器被请求

命令行：

![mark](http://pvjq3azwj.bkt.clouddn.com/blog/20190828/qQtrRBdxBJ01.png?imageslim)

浏览器：

![mark](http://pvjq3azwj.bkt.clouddn.com/blog/20190828/NY34oACrwqgo.png?imageslim)

#### 2.2核心模块

Nodejs提供了很多服务器级别的核心模块，比如`fs`操作文件模块，`http`服务器构建模块，`path`路径操作模块,`os`操作系统信息模块

```js
var fs=require('fs');
var http=require('http');
var os =require('os');
```

#### 2.3 简单的模块化

执行`node a.js`

![mark](http://pvjq3azwj.bkt.clouddn.com/blog/20190828/Xv2iiugQ55FH.png?imageslim)

在node中没有全局作用域，只有模块作用域，内部访问不到外部，外部访问不到内部

**require用于加载模块,exports用于导出模块**

a.js

```js
var b=require('./b')//.js后缀名可以省略
console.log('add',b.add(10,20));
```

b.js

```js
exprots.add=function(a,b){
    return a+b
}
```

`node a.js`  //add 30

#### 2.4 端口号和ip的概念

所有互联网程序都需要进行网络通信,发送请求和获取请求的对象都会有自己的端口号和IP地址，这样服务器才能精准的投送请求信息给每一个请求对象，请求对象也能准确对向服务器发送请求。

#### 2.5http请求内容的内容设定

`res.setHeader()`**是设置http请求头的，其中，Conter-Type是设置当前请求的类型，其中text/plain;charset=utf-8表明，设置为普通文本形式，采用utf-8编码形式来进行解析**，

```js
var http = require('http');
var server=http.createServer();
server.on('request',function(req,res){
	console.log('请求数据')
    var url=req.url;
    if(url==='/plain'){
       res.setHeader('Content-Type','text/plain;charset=utf-8')//设置请求头
	res.end('helloWorld解析文字')
       }
    if(url==='/html'){
       res.setHeader('Content-Type','text/html;charset=utf-8')//值得注意的是，当要使网页显示出html标签却存在中文时，也必须设置请求头类型
        res.end('<p>hello<a href="">点我</a></p>')
       }
})



server.listen(3000,function(){
	console.log('服务器启动了')
})
```

#### 2.6其他类型设置

```js
var http=require('http');
var fs=require('fs');



var server=http.createServer();
server.on('request',function(req,res){
	var url=req.url;
	if(url==='/html'){
		fs.readFile('./resource/home.html',function(error,data){
			if(error){
				console.log('出错了')
			}else{
				console.log('=====>')
				res.setHeader('Content-Type','Text/html;charset=utf-8')
				res.end(data)
			}
		})
	}else if(url==='/img'){
		fs.readFile('./resource/img.jpg',function(error,data){
			console.log('进来了')
			if(error){
				console.log('出错了')
			}else{
				res.setHeader('Content-Type','image/jpeg')//不需要编码，因为我们常说的编码一般就是文字编码
				res.end(data)
			}
		})
	}
})
server.listen(3000,function(){
	console.log('服务器启动')
})
```

#### 2.7 art-template的基本使用

```js
var template = require('art-template');

var str='下面是一个要替换的对象{{$value.name}}';
var replaceStr =template.render(str,{
    name:'王伟'
})
console.log(replaceStr) //下面是一个要替换的对象王伟
```

#### 2.8 表单提交重定向

```js
//重定向，当客户端发现服务器响应状态码为302就会自动响应头中找location，并自动跳转
res.statusCode=302;
res.setHeader('Location','/')
res.end()
```





---

### 3.模块系统

---

#### 3.1 什么是模块化？

* 文件作用域
* 通信规则
  * 加载require
  * 导出

#### 3.2 CommonJs模块规范

在node中的js还有一个重要的概念，模块系统

* 模块作用域
* 使用require方法来加载模块
* 使用exports接口对象来导出模块中的成员

##### 3.2.1 导出exports

* Node中是模块作用域，默认文件所有成员只在当前文件模块生效

* 对于希望可以被其他模块访问的成员，我们就需要把这些公开的成员都挂载到exports接口对象中就可以了

  - 导出多个成员（必须在对象中）

    ```js
    exports.a=123;
    exports.b='aaa';
    exports.c=function(){
        console.log('bbbbbb')
    }
    //返回的模块是一个对象，含多个值
    ```

    

  - 导出单个成员（拿到的就是某个方法，字符串，数字，或对象，函数）

    ```js
    module.exports='hello';
    //返回hello字符串
    ```

    

* 原理解析

  ``` js
  let obj={} 
  let obj2=obj; 
  obj2.a=12; 
  obj.a='obj';
  obj2={};//在这里重新赋值变成了另一个对象，所以，obj还是obj
  obj2.a=13333;
  console.log('obj',obj,'obj2',obj2)
  
  //以上从另一个方面解释了
  exports='aaaa';
  //导出模块后，
  exports={};
  ```

  

#### 3.3 package.json

我们建立一个项目必须要有package.json（包描述文件，项目可以通过这个自动生成一个node_modules）

``` json
{
      "devDependencies": {//依赖包
    "@babel/core": "^7.5.5",
    "@babel/plugin-proposal-class-properties": "^7.5.5",
  },
}
```

#### 3.4 npm命令

* ``npm init`` //初始化项目
  * ``npm init -y``//跳过向导快速成



* ``npm install`` //安装所有package.json里的所有未安装的包
  * ``npm install 包名 --save`` //安装指定的包，并且放到package.js依赖中

* ``npm uninstall --save`` //删除包，并且删除依赖信息，不想删除依赖信息可以去除--save







---

### 4. Express

---

原生的http在某些方面表现不足以应对我们开发需求，所以我们就需要用框架来加快我们的开发效率，框架的目的就是提高效率，让我们代码高度统一

* hello world

  ``` js
  var express =require('express');
  var app = express();
  
  
  
  //公开指定目录,这样做就可以直接通过/public/xx的方式访问到public里面的所有资源,注意express.static('./这个点要加上')，app.use('浏览器访问的目录',express.static('根据浏览器访问的目录公开服务器目录'))
  app.use('/public',express.static('./public/'))
  
  app.get('/',function(req,res){
  	//相当于res.end(),并且不用关心转码的问题
      console.log(req.query)//可以直接拿到query不用引url包来解析了
  	res.send('hello world')
  })
  
  //启动服务器，并制定端口地址,通过node app.js开启服务
  app.listen(3000,function(){
  	console.log('app is running at port 3000')
  })
  ```

  

#### 4.1 Nodejs的热更新

通过npm下载一个第三方命令行工具，nodemon来帮助频繁的修改代码重启服务器问题，nodemon是基于node.js开发的一个第三方命令行工具，我们使用的时候要独立安装

`` npm install --global nodemon``

安装完毕以后，使用

``nodemon app.js``来启动服务，当用nodemon启动服务，代码更改后服务器会监视到然后自动重启服务

#### 4.2 基本路由

路由器

* 请求方法
* 请求路径
* 请求函数

get:

``` js
app.get('/',function(){
    res.send('hellow world')
})
```

post:

``` js
app.post('/',function(){
    res.send('get a post requst')
})
```

#### 4.3 静态服务

``` js
app.use(express.static('public'))//公开某个文件，只要浏览器请求文件在public中存在，则直接访问，不用在地址中写入public

app.use('/public',express.static)//与上述相反，文件名前必须含有/public/该文件才可以被访问
```



#### 4.4 express-art-template

**安装**：

``npm i --save art-template express-art-template``

**使用**

``` js
//配置express-art-template
//html表示在views中的所有带后缀名为.html的文件，都可以被解析
app.engine('html',require('express-art-template'))

//使用
//设置404.html后，express会自动到views目录中找到这个文件然后进行操作
app.get('/art',function(req,res){
    res.render('404.html',{
        name:'这是替换的字符串'
    })
})


```

#### 4.5 重写评论

``` js
var template = require('art-template');
var express = require('express');
var app = express();
var fs=require('fs');
var Mock=require('mockjs');
var Random = Mock.Random;
var day = require('dayjs')


Random.csentence(6, 15)
Random.date( 'yyyy-MM-dd' )
Random.cname()
var mockData=Mock.mock({
	"comments|1-10": [
	{
		'name':'@cname',
		'content':'@csentence',
		'date':'@date("yyyy-MM-dd")'
	}
	]
})
app.use('/views/',express.static('./views'))
app.use('/public/',express.static('./public'))
//配置使用art-template模板引擎
//第一个参数表示，当渲染以.art结尾的文件时，使用art-template模板引擎，express-art-template是专门用来吧express中把art-template整合到express中
//虽然外面不需要art-template，但是必须安装，原因是express-art-remplate依赖了art-template
app.engine('html',require('express-art-template'))

//Express为response响应的提供了一个render方法，render方法默认不能使用，但是当配置了模板引擎就能够启用
//格式:res.render('html模板名',{模板数据})
//第一个参数不能写路径，默认会去项目中的views目录查找改模板文件也就是说express有一个约定：开发文件都放到views目录中
app.get('/',function(req,res){
	res.render('index.html',{
		comments:mockData.comments
	})
})

app.get('/comment',function(req,res){
	mockData.comments.push({
		name:req.query.name,
		content:req.query.message,
		date:day().format('YYYY-MM-DD') 
	})

	// res.statusCode=302;
	// res.setHeader('Location','/')
	// res.end()
	//简写重定向
	res.redirect('/')
	
})

app.get('/',function(req,res){
	fs.readFile('./public/html/index.html',function(error,data){
		if(error){
			res.send('请求失败')
		}else{
			res.end(data)
		}
	})
});

app.listen(3000,function(){
	console.log('runing....')
})
```

#### 4.6处理post请求

**使用前准备:**使用post请求前先配置一个中间件，用于解析浏览器传入参数

`` npm i --save body-parser ``

``` js
var bodyParser = require('body-parser');

c
app.post('commente',function(req,res){
    console.log(req.body)//此时的body就是浏览器post的请求参数
})
```

