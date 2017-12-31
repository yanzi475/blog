##  node 简单知识了解

*  全局变量  _fileName  _dirname  process export/model.export  global  require()
*  path
*  process
*  http
*  module
*  yargs  yargs 模块提供 argv 对象，用来读取命令行参数 ,(_）属性，可以获取非连词线开头的参数
*  chalk  链式书写的规范，目前项目中主要使用的是提示 log 更改样式，颜色等地使用
*  chokidar  针对 node.js fs.watch / fs.watchFile.封装的插件  [chokidar](https://github.com/paulmillr/chokidar)
*  CORS 跨域资源共享  


yargs 模块能够解决如何处理命令行参数，yargs 模块提供 argv 对象，用来读取命令行参数  
argv 对象有一个下划线（_）属性，可以获取非连词线开头的参数。  
yargs 模块还允许通过 command 方法，设置 Git 风格的子命令。   

对于node的学习，前端同学可以不深入了解，但是一些基本必须的知识点还是要好好学习的，如，我们平时接触到的npm,require 模块，es6 ，path，文件路径解析等等。学习node.js基本的东西还是很有必要的，比如搭建一个简单的服务器，做点基本的逻辑处理和数据处理，对于日常开发中比较提高效率的mock 测试，本地服务的搭建，这些都是最基本的很有卓效的提升工作效率。本文，就以开发过程中Web 前端 Mock Server 自动化测试为例，来简单了解下node简单而强大的功能加速你的工作进度的。


##  node 简单知识了解

*  全局对象和全局变量  __fileName  __dirname  process export/model.export  global  require()
*  path
*  process
*  http
*  module
*  yargs  yargs 模块提供 argv 对象，用来读取命令行参数 ,(_）属性，可以获取非连词线开头的参数
*  chalk  链式书写的规范，目前项目中主要使用的是提示 log 更改样式，颜色等地使用
*  chokidar  针对 node.js fs.watch / fs.watchFile.封装的插件  [chokidar](https://github.com/paulmillr/chokidar) 



##  node 具体用法实践

#### 全局变量和path模块 ####

具体用法，官网文档都有，这里只是以最常用的给介绍下简单而巧妙的用法

全局对象  global   process console
全局函数   require()  Buffer()  setTimeout()  clearTimeout() setInterval()
全局变量   Node提供两个全局变量，都以两个下划线开头。
__filename：指向当前运行的脚本文件名。
__dirname：指向当前运行的脚本所在的目录。
这两个对象经常和 path模块连用`path.resolve(__dirname, 'js/app.js')`

[全局](http://javascript.ruanyifeng.com/nodejs/basic.html#toc7)
__fileName  __dirname  process export/model.export  global  require()

```
path.join()  // path.join方法用于连接路径

var path = require('path');
path.join(js, "app.js");  // js/app.js

path.resolve()方法用于将相对路径转为绝对路径。

path.resolve(__dirname, 'js/app.js')     //user/haiyan/project/webpack/js/app.js

path.parse()方法可以返回路径各部分的信息。
var myFilePath = '/user/haiyan/project/op/test/db.json';
path.parse(myFilePath).base
// "db.json"
path.parse(myFilePath).name
// "db"
path.parse(myFilePath).ext
// ".json"

```

#### module ####
Node模块采用CommonJS规范。只要符合这个规范，就可以自定义模块。在项目中我们经常用到的是，定义一个模块，然后在其他模块中引用上一个模块，  module和require() 结合使用。
test.js

```
function Test(){
	this.fun1 = function(){
		console.log('one');
	}
	this.fun2 = function(){
		console.log('two');
	}
	
}
module.exports = Test

const test1 = function(){
}

module.exports.test1 = test1;

const test2 = function(){
}

module.exports.test2 = test2;


```

app.js

```

let Test = require('./test.js');
var test = new Test();
test.fun1();

//

let {test1,test2} = require('./test.js');


```



#### http ####

 node的http模块主要用于搭建HTTP服务。


```
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
    res.statusCode = 200;
res.setHeader('Content-Type', 'text/plain');
res.end('Hello World\n');
});

server.listen(port, hostname, () => {
    console.log(`Server running at http://${hostname}:${port}/`);
});



// 另一种写法
function onRequest(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}

http.createServer(onRequest).listen(process.env.PORT);



$ node app.js


```

打开浏览器，访问http://localhost:8080。 使用http模块中 get() 请求搭建一个简单的服务

```
var http = require('http');

http.createServer(function(req, res) {

  // 主页
  if (req.url == "/") {
    res.writeHead(200, { "Content-Type": "index/html" });
    res.end("Welcome to the homepage!");
  }

  // About页面
  else if (req.url == "/about") {
    res.writeHead(200, { "Content-Type": "about/html" });
    res.end("Welcome to the about page!");
  }

  // 404错误
  else {
    res.writeHead(404, { "Content-Type": "text/plain" });
    res.end("404 error! File not found.");
  }

}).listen(8080, "localhost");


```

利用上面的就可以完成搭建一个简单的服务器，做一些基本的逻辑和数据处理。  
当然，这些已经脱离了我们的目的，来构建web前端mock server 服务。
