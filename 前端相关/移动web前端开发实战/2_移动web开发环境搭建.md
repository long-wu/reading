第二章 移动web开发环境搭建
============================


## 1、跨平台编辑器sublime
### sublime介绍
	Sublime Text 是一个代码编辑器，也是HTML和散文先进的文本编辑器。Sublime Text是由程序员Jon Skinner于2008年1月份所开发出来，它最初被设计为一个具有丰富扩展功能的Vim
			
	另外Sublime Text是一款跨平台代码编辑器(Code Editor)从最初的Sublime Text 1.0到现在的Sublime Text 3.0
	Sublime Text从一个不知名的编辑器演变到现在几乎是各平台首选的GUI编辑器
		

### sublime优点
* 跨平台： Vim和Sublime Text均为跨平台编辑器（在Linux、OS X和Windows下均可使用）
* 可扩展： Vim和Sublime Text都是可扩展的（Extensible），并包含大量实用插件，可通过安装插件来成倍提高工作效率
* 互补：	  Vim和Sublime Text分别是命令行环境和图形界面环境下的最佳选择，同时使用两者会大大提高工作效率

### 安装及使用
	自行根据需要在特定的时间百度把、、、


## 2、专业前端开发工具WebStorm
### WebStorm介绍
	WebStorm 是jetbrains公司旗下一款JavaScript 开发工具。
	目前已经被广大中国JS开发者誉为“Web前端开发神器”、“最强大的HTML5编辑器”、“最智能的JavaScript IDE”等
	其与IntelliJ IDEA同源，继承了IntelliJ IDEA强大的JS部分的功能。

### WebStorm优点
* 版本控制: webstorm中集成了版本控制工具
* 本地历史: 可以查看本地的修改历史，随时可以恢复某个时间的修改
* 即时模板: 提供代码模板的功能，通过模板快速编写一些固定样式的代码
* coffeescript: 支持coffeescript的即时编译、代码提示、代码跳转，代码调试等功能
* node.js: 支持node.js调试
* 代码自动保存 代码格式化等好用的功能

### 安装及使用
	自行根据需要在特定的时间百度把、、、


## 3、使用node.js
### 什么是node.js
	Node.js是一个Javascript运行环境(runtime environment) 发布于2009年5月 其实质是对Chrome V8引擎进行了封装
	Node.js 不是一个JavaScript框架，不同于CakePHP、Django、Rails
	Node.js 更不是浏览器端的库，不能与 jQuery、ExtJS 相提并论
	Node.js 是一个让 JavaScript 运行在服务端的开发平台，让 JavaScript 成为与PHP、Python、Perl、Ruby 等服务端语言平起平坐的脚本语言
	Node.js是一个基于Chrome JavaScript运行时建立的平台， 用于方便地搭建响应速度快、易于扩展的网络应用
	Node.js 使用事件驱动， 非阻塞I/O 模型而得以轻量和高效，非常适合在分布式设备上运行数据密集型的实时应用

### 安装node.js
	去node.js的官网下载最新版本并安装
	打开终端 输入node -v查看是否安装成功(安装成功就会显示当前版本信息)

### 调试node.js
	下面是一个使用node.js编写的简单demo: 
	使用webstorm编写如下代码:
		const http = require('http')
		const hostname = '127.0.0.1'
		const port = 3000
			
		// 创建一个HTTP服务
		const server = http.createServer((req, res)=>{
		    res.statusCode = 200
		    res.setHeader('Content-Type', 'text/plain')
		    res.end('Hello World\n')
		})	
			
		// 监听指定端口并启动服务
		server.listen(port, hostname, ()=>{
		    console.log(`Server running at http://${hostname}:${port}/`)
		})
			
	然后在webstorm的Terminal中进入代码目录下运行该代码:
		cd server		// 进入代码目录
		node xxx.js 	// 运行上述代码
	运行代码后Terminal中出现:
		Server running at http://127.0.0.1:3000/
	打开浏览器访问http://127.0.0.1:3000/:
		可以在浏览器的页面上看到Hello World

### 什么是NPM
	简单说npm全称是Node Package Manager 也就是是一个包管理工具
	在安装node的时候就自动安装了npm 因为npm是node.js的标准模块管理工具 随同node.js一起安装
		
	npm常用命令:
		npm install		安装模块
		npm uninstall	卸载模块
		npm update		更新模块
		upm outdated	检查模块是否过时
		npm ls			查看安装的模块
		npm init		在项目中创建一个package.json文件

### HTTP服务器http-server
	http-server是一个简单的零配置命令行HTTP服务器 非常适合日常的测试、本地开发等环境
		
	安装http-server:
		npm install http-server -g
		
	进入项目文件目录启动HTTP服务:
		http-server						# 进入http://localhost:8080查看服务
		http-server [path] [options] 	# 也可以使用多种参数
		注: 
			path默认为当前项目的public文件夹 如果不存在就指定为当前根目录
			options如下:
				-p 端口号(默认8080)
				-a IP地址(默认0.0.0.0)
				、、、(其他要看的话自行百度)
