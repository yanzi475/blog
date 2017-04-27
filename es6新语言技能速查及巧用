最近系统查看了es6的文档，结合自己实际中应用的情况中所遇到的问题，以及es6所带来的优势的体验，写了这篇速查字典，供充电收藏。
清单如下：

* 变量
* 字符串
* 数值
* 数组
* 对象
* Set 和 Map
* 箭头函数
* 迭代 Iterator
* Class
* Symbol Proxy Reflect
* 异步

##  变量

* let/var 
* let/const 
* es5 和es6 声明变量的方式
* 块级作用域
* 变量解构赋值

|                |let    |    var |
| ----------     | ------ | ------ |
| 定义变量         | Y      | Y |
| 变量提升         | N      | Y |
| 重复检查         | Y      | N |
| 只作用于声明的块级作用域 | Y | N |


|                |let    |   const|
| ----------     | ------ | ------ |
| 定义变量         |  变量    | 常量|
| 初始化赋值        | N      | Y |
| 可更改            | Y      | N |

|                |es5   |   es6|
| ----------     | ------ | ------ |
| 定义变量         |  var   | let|
| 常量        | -     | const|
| 类和函数            | -/function    |  class/fuction|
| 引入变量            | -     |  import|

 说完以上，来说下块级作用域（在es6 之前没有）let定义的变量的方式，产生块级作用域了，符合习惯上全局变量和局部变量的特性，也解决for循环，可用闭包和立即执行函数表达式来实现每次循环能拿到变量的值。取代了晦涩难懂的IIFE(Immediately Invoked Function Expression：声明即执行的函数表达式)，也规范了for循环时局部变量造成的内存泄漏。
 变量解构赋值为数组，对象，字符串以及函数参数提供了更灵活方便的初始化赋值的方式，用这种方式可以更简单地提取json对象中的数据,更方便地从数组和对象选取指定的元素值。
 
 ```
 let arr = ['code','name','risk','type'];
 let [one,two] = arr;
 console.log(one ,two); // code name

 let obj ={
    code:'pa0123',
    name:'活期',
    rate:{
        month:'11',
        year:'12'

    }

 }

 let {
    
    rate:{
        year
    }
 } = obj;
 console.log(year); //12



  let ratelist=[
    {
        month:'11',
        year:'12'

    },
    {
        month:'13',
        year:'14'

    }
    ]
 for(let{ month,year} of ratelist){
    console.log(`month is ${month};year is ${year}`);

 }
 //month is 11;year is 12
// month is 13;year is 14
 
 ```
 
 
 



## 字符串



* `for...of,includes(),startsWith(),endsWith(),....`
*  `indexof()`,
* `repeat(),padStart(),padEnd()`

* 判断字符的包含新增方式  `includes(),startsWith(),endsWith(),....`
* 原有方式`stringObject.indexOf(searchvalue,fromindex)`
* 字符重复与字符补全 `repeat(),padStart(),padEnd()`
* 字符的遍历`for...of`

* 模版字符串简化了在js 中写字符串和变量混合的书写方式。主要是使用``包裹，${param}引用变量的方式。
    

## 模板字符串


* `$('#result').append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);`

* 模板中可以
 * 添加生成的 SSH key 到 ssh-agent。命令为：
   `$ ssh-add ~/.ssh/...id_rsa...`
   其中，id_rsa,可以通过查看的时候拷贝过来。

   
## 数值

数值的新增方法，将一些全局方法移到Number对象上。

* 数值的新增方法，返回布尔值`Number.isFinite(), Number.isNaN()`
* `Number.parseInt(), Number.parseFloat() Number.isInteger()`
* 数值的范围 Number.MAX_SAFE_INTEGER和Number.MIN_SAFE_INTEGER这两个常量，用来表示这个范围的上下限，Number.isSafeInteger()则是用来判断一个整数是否落在这个范围之内。
* Math.trunc()方法用于去除一个数的小数部分，返回整数部分，当然也可以使用Math.ceil(x),Math.floor(x)来实现。
 

## 数组的新方法

* 数组的转换 object 和Set Map转换数组 Array.from()与扩展运算符（...）的区别 //扩展运算符背后调用的是遍历接口（Symbol.iterator）用来获取数组元素,而Array.from()支持类似数组的对象（Set Map）即必须要length 属性。
* 数值转换数组  Array.of(3,4,5);//[3,4,5]
* 找出第一个符合条件的数组成员find(), findIndex()
* 数组填充 fill(value,start,end)
* 数组遍历的新方法  entries(),keys(),values()
* 判断包含和下标 includes() 与 indexOf()

## 箭头函数

* 语法 {arg1, arg2} => {} //  arg => {}
* 作用1 this的绑定，可以将{}外部的this 延伸到内部，把this延伸到了外部 ，即this绑定为申明所在的对象上，不会动态改变。
* 作用2 可以简化了函数内部定义函数this变量的转换，可以省略 let that = this;
* 在处理事件的回调以及多次回调中有些使用箭头函数有些没有，this的值就变得不可预期，这种情况下，就要保证写法的统一。但事件的回调如果需要动态改变this指向时不要使用箭头函数。


   

## 对象

* 新的定义对象的方式，允许直接写入变量和函数，作为对象的属性和方法。 var foo='obj'; var bar ={foo};//{foo:'obj'}
* 对象相等 Object.is()，与=== 运算符相同
* 对象合并拷贝 Object.assign(target, source1, source2);
* for...in, Object.keys(obj), Object.values(obj), Object.entries(obj)

## Set 和 Map

* Set 无重复值的类似数组的结构,对于数组的去重问题，可以利用Set这一特性，作为转换达到目的

	```
	let arr = [0, 0, 1, 1];
	let set = new Set(arr);
	let newArr = Array.from(set); // Array.from方法可以将 Set 结构转为数组。
	
	console.log(newArr); // [0, 1]
	```
* Set 的方法主要有
	- `add(value)`：添加某个值，返回Set结构本身。
	- `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
	- `has(value)`：返回一个布尔值，表示该值是否为Set的成员。
	- `clear()`：清除所有成员，没有返回值。
* Map 键值对集合的Object 对象
* Map 的方法主要有
	- `set(key,value)`：添加某个值，返回Set结构本身。
	- `get(key)`：返回指定key的成员。
	- `has(key)`：返回一个布尔值，表示Map对象是否有指定成员。
	- `delete(key)`：删除指定key成员。
	- `size()`：返回成员总数。
	- `clear()`：清除。

## Symbol Proxy Reflect


* Symbol可以为对象新增Symbol类型
* Proxy 用于修改对象原形的方式，从而修改一类结构基类上的方法
* Reflect 与 Proxy 相对应
* 新增类型Symbol，从此，JavaScript 就有7中基本类型：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。


## 迭代 Iterator

* 可迭代协议和迭代器协议
* for...of

 可迭代协议允许 JavaScript 对象去定义或定制它们的迭代行为，一般具有 Symbol.iterator 的属性.	
 迭代器协议定义了一种标准的方式来产生一个有限或无限序列的值。当一个对象被认为是一个迭代器时，它实现了一个 next() 的方法并且拥有done和value 两个属性的返回对象。--MDN

ES6语法中新的遍历方式时for...of循环，对于具有迭代属性的数据结构上（如数组，Set Map）与之前for...in 主要的区别是：for...in循环读取键名，对应数组的下标值；for...of循环读取键值，对应数组的value值。


## Class

```
class hello {
  constructor(){
 	 //生成对象时，调用该方法，只有一个且执行一次
 	 // 默认返回实例对象（即this）
    // 可以定义成员属性
    
  }

  example(){
    // 成员方法
  }
 
}`

// 等同于

hello.prototype = {
  example(){}
};
```
## Generator async


* generator 来实现对象的遍历,数组结构原生具备Symbaol.iterator  属性，而对象不具备
* Generator Function（生成器函数）和Generator（生成器）是ES6引入的新特性
* 本质是迭代器
* 与普通函数的区别主要有两点：1，function*,yield,2，next调用。 yield 本无返回值或返回undedined；next返回一个具有done，value 两个属性的对象，并且调用可以传参。
* 目前的co函数库可以自动执行自动执行 Generator 函数，其实是自动执行器。
* async 函数对 Generator 函数的改进,
* 语法：async取代 function* await 取代 yield
* 内置执行器，可以自动执行，不必像Generator依赖next函数的执行。

## Decorator
修饰器（Decorator）是一个函数，发生在编译阶段用来修改类的行为。
