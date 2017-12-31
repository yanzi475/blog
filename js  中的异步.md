
* 前言
* 同步与异步
* js 中的异步机制
* 异步的进化史
* 回调函数
* Promise
* ES6新形式的异步
* 总结


## 前言
为什么要写这篇文章呢？    
异步的处理是很考验功底的问题，之所以这样说，是因为异步的发展一直在争议中进行。从最初的回调产生和回调地狱的无奈，到Promise/Deferred 规范傻傻不分的混沌，再到ES6 中的 Generator 外加 co 库的方式，和 Async/Await 的异步。这些发展背后的推力是什么呢，异步问题到底为何物，它解决了怎样的问题的，同时是否也存在着怎样的隐患，这篇文章能给为大家阐明缘由，解答疑惑。  
当然，还是先追本溯源异步的产生和发展，结合自己在工作、开发过程中的经验教训，给出一些建议，认真的思考和总结如何更优雅、更高效的处理异步问题。
 
## 同步与异步
所谓的同步编程也可理解为可阻塞操作，是代码逐次执行的，前面操作的执行会阻塞后续的操作，例如：页面交互时请求接口数据时，后续的操作是要依赖ajax返回的数据。所以页面会有等待数据返回的loading效果。 这也是交互友好一种方式。   
异步是不阻塞后续操作的，对于哪些耗时的数据交互操作，javascritp可以执行其他操作，等这些操作执行后，则以回调异步方式通知结果，继续执行下面的操作。   
javascript 执行环境时单线程的，在接口交互时会阻塞下面的执行。但是，为了优化页面交互的友好，提高响应速度，减少等待响应的时间，等这些原因也是推动了js 中必须要有异步来提高性能。下面就来聊聊Javascript之异步。

##  js 中的执行机制

说到js 的异步，先要了解js的执行机制。js 的执行是基于事件循环的Event Loop 的并发模型的。这里说的是并发，并发是同一时间段中多个任务执行，其实是交替执行的过程。即使 javascript 执行环境时单线程的，也可以实现异步。

##### 事件循环（Event Loop）的并发模型 
js 中的操作是在主线程队列的控制下栈结构中执行的，而异步任务是在与栈相关联的队列中，栈是后进先出的数据结构，只有栈中的同步操作处理完，才会在队列中取异步任务，队列中的异步操作通常是接口数据交互的ajax ，执行需要一定的条件。队列中的任务可以执行了，就放到栈中执行出栈操作。下面是更形象的图来展示。定时器setTimeout(func, 0)就是js执行机制的产物。

![](file:///Users/haiyanSong/Documents/pic/share/EventLoop.png)

如下面这段代码, 首先是windows 下的全局变量s入栈，两个函数声明，执行b(21);函数b入栈，b 中有调用a ,a入栈并执行a，然而执行a中有定时加入callback 队列中，等时间到之后执行，主线程的栈中依次执行，输出s为1，输出x 为21.此时执行队列中的异步任务，输出s为21.

```
let s = '1';
function a(p){
	let s = p;
	setTimeout(
	()=>{
		console.log(s);
	},1000)
	
  
}

function b(x){
	a(x);
	console.log(s);
	console.log(x); 
}

b(21);

```  

## 异步的进化史

了解了js 的运行机制，为解决一些数据先后顺序的依赖关系，保证页面的健全，避免页面长时间的loading，解决无数据返回场景下页面的错乱问题，出现了回调函数。回调函数很大程度上解决了异步请求数据上的依赖关系，接着发现，当有多个回调时，就会出现回调金字塔问题，为解决书写上的优雅，promise应运而生。目前随着es6的普及，generator 加co 库的高效异步，到async 异步的统一，这正是异步的发展历程。由回调函数到Promise／Deferred，再到es6 以来提出的各种新形式的异步。下面详细探讨下。


## 回调函数

##### 经典回调函数模式

基于ajax回调函数可以实现异步的方式，简单的模式如下：

```
let results = "";

$.ajax({
method: "GET",
url: "https://www.google.com.hk/",
success:(data)=> {
	results = data;
	}
});

console.log(results);

```
上面这段代码可以看出问题吗，就是输出的结果是undefined, 并不会没有返回，而是ajax 调用是异步的，上面的执行机制理解的话，可以明白，输出是在返回结果之前出栈，所以是undefined,所以要把数据的处理结果放在callback 里面，ajax 的success或者error 中来处理。如下面所示。

```
let results = "";

$.ajax({
method: "GET",
url: "https://www.google.com.hk/",
success:(data)=> {
	results = data;
	console.log(results);
	}
});

```

##### 基于回调函数的异步方式

基于回调函数可以实现异步的方式，简单的模式如下

```
let callbackAsyc = callback =>{
		// todo
		setTimeout(()=>{
			callback();// 执行回调
		},500);
};

function callback(){
		// todo
		console.log("callback");
			
}

callbackAsyc(callback);

```

##### 基于事件的异步方式

基于事件的异步，大家最熟悉jquery 中的click 事件就是最简单的基于发布订阅模式的异步，对于熟悉angular的同学，很容易想到$on，$emit, $broadcast，这是angularjs 框架中对于异步事件的封装使用，如果想自己来封装基于发布／订阅（publish／subscribe）异步,通用形式如下：

```

//angularjs 中的异步事件
$scope.$emit('onload', 'page is ok');

$scope.$broadcast('onload', {
  // todo
});

$scope.$on('onload', function (event, data) {
  console.log("page is ok"); 
});

// 自定义事件
let event = {
    //订阅
    addSubscriber(callback) {
        this.subscribers[this.subscribers.length] = callback;
    },
 
    //发布
    publish() {
        for (let fn of this.subscribers ) {
            if (typeof fn === 'function') {
                fn();
            }
        }
    }
   
};
//订阅异步事件
event.addSubscriber(()=>{
	//todo
});
window.onload = ()=>{
	//执行异步
	event.publish();
}

```
基于回调的异步和基于事件的异步本质上无根本区别，只是形式不同，核心还是回调处理。
回调函数保证了数据的正确，同时也按照我们想要的方式完成了处理。所以，回调函数的核心思想一直贯穿着异步的发展。但是在一些其他场景下，我们处理的数据依赖多个回调的结果，一个比较常用的技巧是设置布尔标示变量。如下所示。虽然可以解决两层嵌套的问题，如果多级回调数据的嵌套，简单地使用回调函数处理，这样就不容易阅读和后续的维护，也是著名回调金字塔问题，这就出现了promise/Deferred。


```
let result1 = '';
let result2 = '';
let flag1 = false;
let flag1 = false;

$.ajax({
method: "GET",
url: "https://www.google.com.hk/",
success:(data)=> {
	flag1 = true;
	result1 = data;
	
	}
});

$.ajax({
method: "GET",
url: "https://www.baidu.com/",
success:(data)=> {
	flag2 = true;
	result2 = data;
	if(flag1 && flag2){
		console.log(result1);
		console.log(result2);
	}
	}
});

```

    

##  Promise

##### Promise产生的动力

Promise出现的最大的动力就是简化嵌套的回调函数，可以用同步的写法解决异步问题，简洁写法是最主要的特征。es6提出以来又对Promise做了正式的规范,规范了之前Promise/Deferred。我们可以通过Promise对象生产Promise实例。实践中Promise的经典用法如下：

##### Promise定义和用法

```
function doAsync(){
	return new Promise(
			(resolve, reject) =>{
				  // ... some code
				  if (/* 异步操作成功 */){
				    resolve(value);
				  } else {
				    reject(error);
				  }
		});
}

let promiseObject = doAsync().then(
			value =>{
				// when resolve  todo
			},
			err =>{
				//when reject  todo 
			}
);

```

[Promise/A+ 规范](https://promisesaplus.com/)  规定Promise有3种状态，pending,resolve和reject,其中pending (等待)的状态可以转换为resolve（已完成）和reject（已拒绝）的状态，而这两种状态是不能再改变的。 所以，对于那种可以多次调用的的异步方式，回调和promise可以谨慎选择。明白promise 状态的流转，不然就会踩进坑里了。  

##### promise.then方法

 Promise的then 方法是可以接受resolve 和reject 两个回调函数，根据promise的状态会进入不同的回调。当然，reject回调是可选的。
`promise.then(onFulfilled, onRejected)` then 返回的是一个promise,then 方法可以被同一个 promise 调用多次。

```
var promise = new Promise((resolve) => {
  //....
});
promise
  .then(resolve, reject)
  .then(resolve, reject)
  .then(resolve, reject)

```
##### promise.all 方法

上面说到需要依赖多个函数的回调后的数据时，可以使用promise.all 方法，将多个Promise实例，包装成一个新的Promise实例，all有两种状况，只有所有实例都返回Resolve，新实例的状态才变成Resolve，但是只有有一个实例返回了Reject，新实例的状态就变成了Reject。就保证了输出时 result1 和result2 都有值了，后续对result1 和result2 的处理就不会报错啦，下面用promise.all重写上面的例子

```
let result1 = '';
let result2 = '';
let flag1 = false;
let flag1 = false;


var getData = function(url) {
  var promise = new Promise((resolve, reject)=>{
    var client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

    function handler() {
      if ( this.readyState !== 4 ) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
  });

  return promise;
};

let p1 = new getData("https://www.google.com.hk/").then(data=>{
	result1 = data;
});
let p2 = new getData("https://www.google.com.hk/").then(data=>{
	result2 = data;
});



let p = Promise.all([p1, p2]).then(resolve=>{
		console.log(result1);
		console.log(result2);
		
});

```
##### Promise.race方法及踩坑注意点

   除此之外，Promise还有方法race，all 与race 这两个方法都是将多个Promise实例，包装成一个新的Promise实例，不同的是，race是取多个实例中最先返回的实例状态作为新实例的状态。
   Promise解决了回调嵌套，书写也更加优雅，逻辑也更加清晰。还有一个我们很容易忽略的问题是错误流程的处理，我们在使用Promise时，绝大部分不会在then方法的最后添加catch来扑捉异常。如果没有使用catch回调方法，Promise对象抛出的错误不会传递到外层代码，这就导致异常会被吞噬掉，不会显示，经常调试半天代码都走一遍，接口的报错却没找到真正的原因，这应该是多么痛的领悟。当有多个promise 嵌套在then方法后时，catch 方法扑捉只是最后的promise对象的异常，中间对象出现异常同样debug 不出来。好，接下来Generator就粉墨登场了。

   
## ES6提出新形式的异步



### Generator出现背景及特点

在Promise.then方法嵌套的情况下catch异常的难以扑捉，es6 在底层实现了异步Generator。Generator在处理异步的写法上更为接近同步的流程。Generator最大的特点之一就是函数在执行的过程中可以暂停。然后调用next方法，再次执行。 对于执行一段程序中，如读取一系列文件，读取到指定文件后，暂停做一些修改，然后在继续，Generator就是很好的选择。

##### Generator 定义的两种形式
Generator Function（生成器函数）和Generator（生成器）是ES6引入的新特性。Generator有两种形式的定义，你也可以定义 `生成器函数`  使用构造函数  `GeneratorFunction`构造器 和一个  `function* expression` 生成器函数，实际上都是GeneratorFunction的实例对象。

```
//example one
let GeneratorFunction = Object.getPrototypeOf(function*(){}).constructor
let g = new GeneratorFunction("a", "yield a * 2");
let iterator = g(10);
console.log(iterator.next().value); // 20

//example one
let ge  = function* (param){
	yield param*2;
}
ge(10).next();        //{value:20,done:false}
console.log(ge(2).next().value); // 20

```
##### Generator 执行状态
function* 声明 (function关键字后跟一个星号）定义了一个生成器函数 (generator function)，它返回一个  Generator  对象。与普通函数的区别主要有两点：1，function*,yield,2，next调用; 这种形式是最简单普遍采用的。  
generator函数调用只是返回一个指向内部状态的指针对象，一个遍历器对象，
调用next（）方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个yield语句（或return语句）为止。yield 本无返回值或返回undedined。next方法返回一个对象，它的value属性就是当前yield语句的值，done属性的值，表示遍历是否结束，true 是结束，false是遍历没有结束。它的状态执行图如下

![](file:///Users/haiyanSong/Documents/pic/share/generator.png)


##### yield

yield 本无返回值或返回undedined,它是next 函数执行的标示。然而，在generator函数嵌套的情况下，要使用yield* ,来达到遍历的效果。因为yield命令后面跟的是一个遍历器对象，需要在yield命令后面加上星号，表明它返回的是一个遍历器对象。这被称为yield*语句。

##### next
generator的next方法可以带一个参数，该参数就会被当作上一个yield语句的返回值。next方法也可以带一个参数，改参数作为上一次yield 语句的返回值，如果 Generator 函数内有多个yield语句，可以在 Generator 函数运行的不同阶段，从外部向内部注入不同的值，从而调整函数行为。

```
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

let a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }

```
##### co函数库实现的自动执行器
generator与co 函数库可以实现迭代器的自动执行，从[co 函数库](https://github.com/tj/co) 的源码 可以看出本质还是 promise。目前的co函数库可以自动执行自动执行 Generator 函数，其实是自动执行器。上面的代码用co 函数库来执行的话就很简单`co(a);`只要把generator
传入co 函数就可以自动执行。co函数返回的是一个promise对象，可以使用then方法，以上查询网络状态的以generator与co 函数库的实现方式如下

```
co(* ()=> {
  let rs = yield $.ajax('https://www.google.com.hk/');
  return rs;
}).then((value) =>{
  //TODO
}, function (err) {
  //TODO
});

```

####  async



为了更好的规范异步语法，es7 提案的async／await已经受到大家的热捧。async是es7 规范提出的草案，目前还在beta 阶段。只有一些浏览器支持，它的实现方式具体如下

##### async／await的实现形式

```
let testNet = async () => {
    try {
        let rs = await $.ajax('https://www.google.com.hk/');
        return rs;
    } catch(err) {
        console.error(err);
    }
};
testsNet().then(()=>{
	//todo
	console.log(this.data);
});
//当然，最简单的方式
testNet();
```
使用async／await 来实现上面测试网络状态的栗子，可以发现 await必须定义在async函数内部，async函数返回的其实也是一个promise对象，遇到await 就会等待异步的结果返回，有了异步数据之后才会执行下面的语句。可以在then函数之后写回调函数，执行后续具体的操作。await 命令放在 try...catch 代码块中，then之后运行结果可能是rejected， 健壮异常处理。并且在没有闭包，作用域变化的情况下，箭头函数中使用this 是不会有问题的。

##### async／await 优势的具体体现

以上给出来async／await 实现方式，说它的也是可以以promise的方式来使用，不要以为可以把它等同于promise，其实不然，以下给出几个具体的场景，初略展示下它的魅力。

###### async／await  的try／catch 处理更强大
以上面的测试网络正常的例子为准，以promise和 async两种方式来实现，为更好的说明问题，getData（）函数返回服务数据，返回的是一个promise对象。如果其中这条语句抛出异常`if(data instanceof Array){data.filter(callback);}`，promise中的try／catch是扑捉不到的，前面也有提过，而async／await是可以扑获的。

```
let testPro = () => {
    try {
        getData().then(data=>{
        		//todo
        		if(data instanceof Array){
        			data.filter(callback);  //此处若抛异常，catch扑捉不到，再能在增加promise.catch()
        			throw new Error('testPro'); //主动抛异常
        		}
        		console.log(data);
        
	        })
        
    } catch(err) {
        console.error(err);
    }
};

let testAsync = async () => {
    try {
		  await getData();
		  if(data instanceof Array){
    			data.filter(callback);   //此处若抛异常，catch可以抛出异常，
    			throw new Error('testAsync'); 
    		}
        console.log(data);
    } catch(err) {
        console.error(err);
    }
};

testPro();
testAsync();
```
###### async／await  处理嵌套逻辑更清晰
上面依赖多个嵌套的promise的返回结果的，如promise1 的返回结果result1 和promise2 的返回结果result2 ，然后再根据result1 和result2 的结果调用promise3，这样的逻辑上面写了好多，还用了promise.all来处理，下面就用async来处理，简简单单几行代码，逻辑也很清晰。

```

let testNetAsync = async () => {
	let result1 = await p1();
	let result2 = await p2();
	
	return promise3(result1, result2);
	
   
};

```


###### async／await  断点调试实现真正'自由'

在promise多个then链式调用时，在debug 下单步调试是很困难的，then语句是不能设置断点的，可以在then的处理函数中设置断点，这个另说的，但是如果是await 语句是可以设置断点的，为了具体说明改下上面的例子，在调试模式下`await p1();`是可以设置断点的本人已经亲测有效，具体可以在jsbin 中运行本文部分例子代码。

``` 
 let testAsyncDebug = async () => {
	await p1();
	await p2();
   
};

```

async／await 总的来说呢，语法简洁，可以把复杂的嵌套改写同步方式，逻辑明了，可读性更强。对比generator,以下几点不同。

* async 函数对 Generator 函数的改进,是 Generator 函数的语法糖。
* 语法：async取代 function* await 取代 yield,有更好的语义。
* 内置执行器，可以自动执行，不必像Generator依赖next函数的执行。


## 总结
从异步的产生的渊源，javascript 的事件循环的Event Loop 的并发模型,理解setTimeout定时器的原理，回调函数的强大和困惑，以及现有框架中基于事件的发布订阅模式的异步。深入理解promise，灵活的使用promise可以解决很多复杂的问题，generator 和async/await才是解决异步回调的最佳实践，而async/await属于es7,目前一些浏览器不支持,可以使用babel转换成ES5等效代码,也是不远将来的趋势。本文就对这些知识点串联起来，给大家理出一个清晰的的脉络图。如你所愿,就是这些啦。

