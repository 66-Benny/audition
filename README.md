#### 1. 请描述一下cookies，sessionStorage 和 localStorage 的区别？
> ##### SessionStorage, LocalStorage, Cookie这三者都可以被用来在浏览器端存储数据，而且都是字符串类型的键值对。
> ### Cookie
> 是服务器发送到用户浏览器并保存在浏览器上的一块数据，它会在浏览器下一次发起请求时被携带并发送到服务器上。比较经典的，可以用来确定两次请求是否来自于同一个浏览器，从而能够确认和保持用户的登录状态。Cookie的使用使得基于无状态的HTTP协议上记录稳定的状态信息成为了可能。
> #### Cookie主要用在以下三个方面：
> 1.会话状态管理（如用户登录状态、购物车）
> 2.个性化设置（如用户自定义设置）
> 3.浏览器行为跟踪（如跟踪分析用户行为）
> #### Cookie的缺陷：
> 1.每个 HTTP 请求中都包含 Cookies，从而导致传输相同的数据减缓我们的 Web 应用程序。
> 2.每个 HTTP 请求中都包含 Cookies，从而导致发送未加密的数据到互联网上。（除非用HTTPS）
> 3.Cookies 只能存储有限的 4KB 数据，对于复杂的存储需求来说是不够用的。


> ### sessionStorage
> sessionStorage 里面的数据在页面会话结束时会被清除。
> ### localStorage
> 存储在 localStorage里面的数据没有过期时间 <br/>
> sessionStorage和localStorage。 不会被发送到服务器上。同时空间比Cookie大很多，一般支持5-10M。

> ### 三者不同点
> 1.cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。
> 2.存储大小限制也不同
> 3.数据有效期不同, cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭
> 4.作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。
<hr/>

#### 2. 解释下原型继承的原理。
> ### 
<hr/>

#### 3. CommonJs 与 AMD 和 CMD 的区别
> ### CommonJs
> 一个文件就是一个模块, 其内部定义的变量, 方法都处于该模块内, 不会对外暴露.
> 主要语法:
> * 使用 exports 或者 module.exports 暴露模块中的内容
> * 使用 require 来加载模块

#### demo
```js
1. 新建 a.js, 导出 name 和 sayHello
// a.js
const name = 'Bob'
function sayHello(name) {
    console.log(`Hello ${name}`)
}
module.exports.name = name
module.exports.sayHello = sayHello
```
```js
2. 在 b.js 中引入 a 并调用
// b.js
const a = require('./a')
const name = a.name
console.log(name) // Bob
a.sayHello(name) // Hello Bob
```
> 由于 CommonJs 是同步加载的模块的, 在服务端(node), 文件都在硬盘上, 所以同步加载也无所谓, 但是在浏览器端, 同步加载就体验不好了. 所以 CommonJs 主要使用于 node 环境下.

> ### AMD
> AMD 全称为 Asynchromous Module Definition(异步模块定义), 实现了异步加载模块. require.js 实现了 AMD 规范
> 主要语法:
> * define(id, [depends], callback) // 导出模块
> * require([module], callback) // 导入
#### demo
```js
1. 新建 a.js, 输入以下内容
// a.js
define(function() {
  let alertName = function(str) {
    alert('I am ' + str)
  }
  let alertAge = function(num) {
    alert('I am ' + num + ' years old')
  }
  return {
    alertName: alertName,
    alertAge: alertAge
  }
})
```
```html
2. 在 test.html 中调用 a 模块
// test.html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <script src="./require.js"></script>
    <script>
        require(['a'], function (alert) {
            alert.alertName('JohnZhu')
            alert.alertAge(21)
        })
    </script>
</body>
</html>
```
>能够异步加载模块, 适合在浏览器中运行, 但是不能够按需加载, 必须提前加载模块

> ### CMD
> 实现了按需加载
> 与 AMD 的区别:
> * 对于依赖的模块 AMD 提前执行，而 CMD 是延迟执行
> * CMD 推崇依赖就近, AMD 推崇依赖前置
> * AMD和CMD最大的区别是对依赖模块的执行时机处理不同，而不是加载的时机或者方式不同，二者皆为异步加载模块。

#### demo
```js
// AMD
define(['./a', './b'], function(a, b) {
  a.doSomething()
  b.doSomething()
})
// CMD
define(function(require, exports, module) {
  var a = require('./a')
  a.doSomething()
  var b = require('./b')
  b.doSomething()
})
```
<hr/>

#### 4. 什么是闭包，如何使用它，为什么要使用它？
>  闭包是就是函数中的函数，里面的函数可以访问外面函数的变量，外面的变量的是这个内部函数的一部分。
> 辅助理解:
```js
function outer(){
   var num=0;//内部变量
   return function add(){//通过return返回add函数，就可以在outer函数外访问了。
        num++;//内部函数有引用，作为add函数的一部分了
        console.log(num);
    };
}
var func1=outer();//
func1();//实际上是调用add函数， 输出1
func1();//输出2
var func2=outer();
func2();// 输出1
```
> 闭包可以用在许多地方。它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。
<hr/>

#### 5. call和apply和bind的区别是什么？
> [理解 JavaScript 中的 this、call、apply 和 bind](https://juejin.im/post/5b9f176b6fb9a05d3827d03f)
> 对于 apply、call 二者而言，作用完全一样，只是接受参数的方式不太一样
> #### call:
> this 将会指向传递给 call 的第一个参数。call 需要把参数按顺序传递进去
> #### apply:
> this 将会指向传递给 apply 的第一个参数。apply 则是把参数放在数组里。
> #### bind:
>.bind 和 .call 完全相同，除了不会立刻调用函数，而是返回一个能以后调用的新函数。
>再总结一下：
> * apply 、 call 、bind 三者都是用来改变函数的this对象的指向的；
> * apply 、 call 、bind 三者第一个参数都是this要指向的对象，也就是想指定的上下文；
> * apply 、 call 、bind 三者都可以利用后续参数传参；
> * bind 是返回对应函数，便于稍后调用；apply 、call 则是立即调用 。
<hr/>

#### 6. 请解释变量声明提升。
> 变量声明提升： 通过 var 声明的变量在代码执行之前被js引擎提升到了当前作用域的顶部。
> 
```js
console.log(a);
var a = 1;
运行结果：undefined
//没有报错，而是输出了undefined, 说明变量声明 var a 被提升到了作用域的顶部， console.log 进行RHS引用时找到了变量 a, 所以没有报错。
```
> 函数声明提升： 通过函数声明的方式（非函数表达式）声明的函数在代码执行之前被js引擎提升到了当前作用域的顶部，<strong> 而且函数声明提升优先于变量声明提升 <strong/>。
```js
var getName = function(){
    console.log(2);
}
function getName (){
    console.log(1);
}
getName();
```
>这个例子涉及到了变量声明提升和函数声明提升。
>函数声明function getName(){}的声明会被提前到顶部。而函数表达式var getName = function(){}则表现出变量声明提升。
>需要注意的是，函数优先，虽然函数声明和变量声明都会被提升，但是函数会首先被提升，然后才是变量。因此上面的函数可以转换成下面的样子:
```js
function getName(){    //函数声明提升到顶部
    console.log(1);
}
var getName;    //变量声明提升
getName = function(){    //变量赋值依然保留在原来的位置
    console.log(2);
}
getName();   
//在原来的例子中，函数声明虽然是在函数表达式后面，但由于函数声明提升到顶部，因此后面getName又被函数表达式的赋值操作给覆盖了，所以输出2
```
<hr/>

#### 7. \==和===有什么不同？
> `==`  抽象相等，比较时，会先进行类型转换，然后再比较值
> `===` 严格相等，会比较两个值的类型和值
#### 8. 手写一个递归函数
```js
function factorial(num){
  if (num <= 1){
    return 1;
   } else {
    return num * arguments.callee(num-1);
  }
}
//arguments.callee 是一个指向正在执行的函数的指针，因此可以用它来实现对函数的递归调用
```
>使用 arguments.callee 代替函数名，可以确保无论怎样调用函数都不会出问题。因此，在编写递归函数时，使用 arguments.callee 总比使用函数名更保险。但在严格模式下，不能通过脚本访问 arguments.callee，访问这个属性会导致错误。不过，可以使用命名函数表达式来达成相同的结果。例如：
```js
var factorial = (function f(num){
  if (num <= 1){
    return 1;
  } else {
    return num * f(num-1);
  }
});
```
>以上代码创建了一个名为 f()的命名函数表达式，然后将它赋值给变量 factorial。即便把函数赋值给了另一个变量，函数的名字 f 仍然有效，所以递归调用照样能正确完成。这种方式在严格模式和非严格模式下都行得通。
<hr/>

#### 9. 用promise手写ajax
```js
// 使用promise实现一个简单的ajax

/**
 * 首先，可能会使用到的xhr方法或者说属性
 * onloadstart // 开始发送时触发
 * onloadend   // 发送结束时触发，无论成功不成功
 * onload      // 得到响应
 * onprogress  // 从服务器上下载数据，每50ms触发一次
 * onuploadprogress // 上传到服务器的回调
 * onerror     // 请求错误时触发
 * status      // 返回状态码
 * setRequestHeader // 设置请求头
 * responseType // 请求传入的数据
 */

// 默认的ajax参数
let ajaxDefaultOptions = {
  url: '#', // 请求地址，默认为空
  method: 'GET', // 请求方式，默认为GET请求
  async: true, // 请求同步还是异步，默认异步
  timeout: 0, // 请求的超时时间
  dataType: 'text', // 请求的数据格式，默认为text
  data: null, // 请求的参数，默认为空
  headers: {}, // 请求头，默认为空
  onprogress: function () {}, // 从服务器下载数据的回调
  onuploadprogress: function () {}, // 处理上传文件到服务器的回调
  xhr: null // 允许函数外部创建xhr传入，但是必须不能是使用过的
};

function _ajax(paramOptions) {
  let options = {};
  for (const key in ajaxDefaultOptions) {
    options[key] = ajaxDefaultOptions[key];
  }
  // 如果传入的是否异步与默认值相同，就使用默认值，否则使用传入的参数
  options.async = paramOptions.async === ajaxDefaultOptions.async ? ajaxDefaultOptions.async : paramOptions.async;
  // 判断传入的method是否为GET或者POST,否则传入GET 或者可将判断写在promise内部，reject出去
  options.method = paramOptions.method ? ("GET" || "POST") : "GET";
  // 如果外部传入xhr，否则创建一个
  let xhr = options.xhr || new XMLHttpRequest();
  // return promise对象
  return new Promise(function (resolve, reject) {
    xhr.open(options.method, options.url, options.async);
    xhr.timeout = options.timeout;
    // 设置请求头
    for (const key in options.headers) {
      xhr.setRequestHeader(key, options.headers[key]);
    }
    // 注册xhr对象事件
    xhr.responseType = options.dataType;
    xhr.onprogress = options.onprogress;
    xhr.onuploadprogress = options.onuploadprogress;
    // 开始注册事件
    // 请求成功
    xhr.onloadend = function () {
      if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        resolve(xhr);
      } else {
        reject({
          errorType: "status_error",
          xhr: xhr
        });
      }
    };
    // 请求超时
    xhr.ontimeout = function () {
      reject({
        errorType: "timeout_error",
        xhr: xhr
      });
    }
    // 请求错误
    xhr.onerror = function () {
      reject({
        errorType: "onerror",
        xhr: xhr
      });
    }
    // 捕获异常
    try {
      xhr.send(options.data);
    } catch (error) {
      reject({
        errorType: "send_error",
        error: error
      });
    }
  });
}

// 调用示例
_ajax({
  url: 'http://localhost:3000/suc',
  async: true,
  onprogress: function (evt) {
    console.log(evt.position / evt.total);
  },
  dataType: 'text/json'
}).then(
  function (data) {
    console.log(data.response);
  },
  function (error) {
    console.log(JSON.stringify(error))
  });
```
<hr/>

#### 10. 手写一个类的继承，并解释一下
> ### 
<hr/>

#### 11. 介绍一下 Promise, async, await。
> [Promise对象](http://es6.ruanyifeng.com/#docs/promise)
> [async 函数](http://es6.ruanyifeng.com/#docs/async)
<hr/>

#### 12. 解释一下AngularJS和vue，以及区别
> ### 
<hr/>

#### 13. 手写一个js的深克隆
##### 浅克隆
```js
// 浅克隆函数
function shallowClone(o) {
  const obj = {};
  for (let i in o) {
    obj[i] = o[i];
  }
  return obj;
}
// 被克隆对象
const oldObj = {
  a: 1,
  b: [ 'e', 'f', 'g' ],
  c: { h: { i: 2 } }
};

const newObj = shallowClone(oldObj);
const newObj = shallowClone(oldObj);
console.log(newObj.c.h, oldObj.c.h); // { i: 2 } { i: 2 }
console.log(oldObj.c.h === newObj.c.h); // true
```
>我们可以看到,很明显虽然oldObj.c.h被克隆了,但是它还与oldObj.c.h相等,这表明他们依然指向同一段堆内存,这就造成了如果对newObj.c.h进行修改,也会影响oldObj.c.h,这就不是一版好的克隆.
>我们改变了newObj.c.h.i的值,oldObj.c.h.i也被改变了,这就是浅克隆的问题所在.
>当然有一个新的apiObject.assign()也可以实现浅复制,但是效果跟上面没有差别,所以我们不再细说了.
```js
const newObj = Object.assign(oldObj)
```
##### 有缺陷的深克隆
1). JSON.parse方法
>JSON对象parse方法可以将JSON字符串反序列化成JS对象，stringify方法可以将JS对象序列化成JSON字符串,这两个方法结合起来就能产生一个便捷的深克隆.
>这个方法虽然可以解决绝大部分是使用场景,但是却有很多坑.
> * 无法实现对函数 、RegExp等特殊对象的克隆
> * 会抛弃对象的constructor,所有的构造函数会指向Object
> * 对象有循环引用,会报错
##### 较为完美的深克隆
> 由于要面对不同的对象(正则、数组、Date等)要采用不同的处理方式，我们需要实现一个对象类型判断函数。
```js
const isType = (obj, type) => {
  if (typeof obj !== 'object') return false;
  const typeString = Object.prototype.toString.call(obj);
  let flag;
  switch (type) {
    case 'Array':
      flag = typeString === '[object Array]';
      break;
    case 'Date':
      flag = typeString === '[object Date]';
      break;
    case 'RegExp':
      flag = typeString === '[object RegExp]';
      break;
    default:
      flag = false;
  }
  return flag;
};
```
>我们需要通过正则的扩展了解到flags属性等等,因此我们需要实现一个提取flags的函数.
```js
const getRegExp = re => {
  var flags = '';
  if (re.global) flags += 'g';
  if (re.ignoreCase) flags += 'i';
  if (re.multiline) flags += 'm';
  return flags;
};
```
> 做好了这些准备工作,我们就可以进行深克隆的实现了.

```js

// * deep clone
// * @param  {[type]} parent object 需要进行克隆的对象
// * @return {[type]}        深克隆后的对象

const clone = parent => {
  // 维护两个储存循环引用的数组
  const parents = [];
  const children = [];

  const _clone = parent => {
    if (parent === null) return null;
    if (typeof parent !== 'object') return parent;
    let child, proto;
    if (isType(parent, 'Array')) {
      // 对数组做特殊处理
      child = [];
    } else if (isType(parent, 'RegExp')) {
      // 对正则对象做特殊处理
      child = new RegExp(parent.source, getRegExp(parent));
      if (parent.lastIndex) child.lastIndex = parent.lastIndex;
    } else if (isType(parent, 'Date')) {
      // 对Date对象做特殊处理
      child = new Date(parent.getTime());
    } else {
      // 处理对象原型
      proto = Object.getPrototypeOf(parent);
      // 利用Object.create切断原型链
      child = Object.create(proto);
    }

    // 处理循环引用
    const index = parents.indexOf(parent);

    if (index != -1) {
      // 如果父数组存在本对象,说明之前已经被引用过,直接返回此对象
      return children[index];
    }
    parents.push(parent);
    children.push(child);

    for (let i in parent) {
      // 递归
      child[i] = _clone(parent[i]);
    }
    return child;
  };
  return _clone(parent);
};
```
<hr/>

#### 14. 手写JS实现类继承，讲原型链原理，并解释new一个对象的过程都发生了什么
> ### 
<hr/>

#### 15. 常用的排序算法有哪些
> ### 
<hr/>

#### 16. http请求头有哪些字段
> * Accept	这个头信息指定浏览器或其他客户端可以处理的 MIME 类型。值 image/png 或 image/jpeg 是最常见的两种可能值。请求报文可通过一个“Accept”报文头属性告诉服务端 客户端接受什么类型的响应。 
> * Cookie	这个头信息把之前发送到浏览器的 cookies 返回到服务器。
> * Referer 表示这个请求是从哪个URL过来的，假如你通过google搜索出一个商家的广告页面，你对这个广告页面感兴趣，鼠标一点发送一个请求报文到商家的网站，这个请求报文的Referer报文头属性值就是http://www.google.com。 。
> * Cache-Control 对缓存进行控制，如一个请求希望响应返回的内容在客户端要被缓存一年，或不希望被缓存就可以通过这个报文头达到目的。 
<hr/>

#### 17. 使用 new操作符时具体是干了些什么
> ### 
<hr/>

#### 18. 前端跨域的方法
#### 同源策略 ： 何为跨域，跨域是相对于同源而言。协议、域名和端口均相同，则为同源。

> ##### 1. JSONP
> JSONP 的解决方案就是通过 script 标签进行跨域请求。
> * 前端设置好回调函数，并把回调函数当做请求 url 携带的参数。
> * 后端接受到请求之后，返回回调函数名和需要的数据。
> * 后端响应并返回数据后，返回的数据传入到回调函数中并执行。
> 缺点： 只支持 Get 请求

> ##### 2. CORS
> 服务端设置 Access-Control-Allow-Origin 字段，值可以是具体的域名或者 '*' 通配符，配置好后就可以允许跨域请求数据。
> 如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问。关于CORS更多了解可以看下阮一峰老师的这一篇文章：[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)

> ##### 3. 通过HTML5的postMessage方法跨域
> 比如damonare.cn域的A页面通过iframe嵌入了一个google.com域的B页面，可以通过以下方法实现A和B的通信
> A页面通过postMessage方法发送消息：
```js
window.onload = function() {  
    var ifr = document.getElementById('ifr');  
    var targetOrigin = "http://www.google.com";  
    ifr.contentWindow.postMessage('hello world!', targetOrigin);  
};
```
> B页面通过message事件监听并接受消息:
```js
var onmessage = function (event) {  
  var data = event.data;//消息  
  var origin = event.origin;//消息来源地址  
  var source = event.source;//源Window对象  
  if(origin=="http://www.baidu.com"){  
console.log(data);//hello world!  
  }  
};  
if (typeof window.addEventListener != 'undefined') {  
  window.addEventListener('message', onmessage, false);  
} else if (typeof window.attachEvent != 'undefined') {  
  //for ie  
  window.attachEvent('onmessage', onmessage);  
} 
```
> postMessage的使用方法：
> * otherWindow.postMessage(message, targetOrigin);
> * otherWindow:指目标窗口，也就是给哪个window发消息，是 window.frames 属性的成员或者由 window.open 方法创建的窗口
> * message: 是要发送的消息，类型为 String、Object (IE8、9 不支持)
> * targetOrigin: 是限定消息接收范围，不限制请使用 '*
<hr/>

#### 19. lazyload如何实现
> ##### 方法一：使用节流函数优化
```js
	function throttle(fn, delay, atleast) {
	    var timeout = null,
		startTime = new Date();
	    return function() {
		var curTime = new Date();
		clearTimeout(timeout);
		if(curTime - startTime >= atleast) {
		    fn();
		    startTime = curTime;
		}else {
		    timeout = setTimeout(fn, delay);
		}
	    }
	}
	function lazyload() {
	    var images = document.getElementsByTagName('img');
	    var len    = images.length;
	    var n      = 0;      //存储图片加载到的位置，避免每次都从第一张图片开始遍历		
	    return function() {
		var seeHeight = document.documentElement.clientHeight;
		var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
		for(var i = n; i < len; i++) {
		    if(images[i].offsetTop < seeHeight + scrollTop) {
		        if(images[i].getAttribute('src') === 'images/loading.gif') {
			     images[i].src = images[i].getAttribute('data-src');
		        }
			n = n + 1;
		     }
		}
	    }
	}
	var loadImages = lazyload();
	loadImages();          //初始化首页的页面图片
	window.addEventListener('scroll', throttle(loadImages, 500, 1000), false);
```

> ##### 方法二： 使用一个新的 [IntersectionObserver API](http://www.ruanyifeng.com/blog/2016/11/intersectionobserver_api.html)，可以自动"观察"元素是否可见
```js
	function query(selector) {
	    return Array.from(document.querySelectorAll(selector));
	}
	var io = new IntersectionObserver(function(items) {
	    items.forEach(function(item) {
		var target = item.target;
		if(target.getAttribute('src') == 'images/loading.gif') {
		    target.src = target.getAttribute('data-src');
		}
	    })
	});
	query('img').forEach(function(item) {
	    io.observe(item);
	});
```
<hr/>

#### 20. vue如何实现父子组件通信，以及非父子组件通信
> ### 
<hr/>

#### 21. 数组去重
> ##### 方法一：用 indexOf 简化内层的循环
```js
var array = [1, 1, '1'];

function unique(array) {
    var res = [];
    for (var i = 0, len = array.length; i < len; i++) {
        var current = array[i];
        if (res.indexOf(current) === -1) {
            res.push(current)
        }
    }
    return res;
}

console.log(unique(array));
```

> ##### 方法二： 排序去重的方法
```js
var array = [1, 2, 1, 1, '1'];

function unique(array) {
    return array.concat().sort().filter(function(item, index, array){
        return !index || item !== array[index - 1]
    })
}

console.log(unique(array));
```

> ##### 方法三： ES6
```js
var array = [1, 2, 1, 1, '1'];

function unique(array) {
   return Array.from(new Set(array));
}

console.log(unique(array)); // [1, 2, "1"]
```
```js
// ES6 再简化下
var unique = (a) => [...new Set(a)]
```
<hr/>

#### 22. vue的特点？双向数据绑定是如何实现的
> ### 
<hr/>

#### 23. undefined 和 null 区别
> 一 定义
> null 是 javascript 的关键字，表示一个特殊值，常用来描述"空值"，typeof 运算返回"object"，所以可以将 null 认为是一个特殊的对象值，含义是"非对象"。
> undefined 是预定义的全局变量，他的值就是"未定义"， typeof 运算返回 "undefined"

> 二 转义
> 转换成 Boolean 时均为 false，转换成 Number 时有所不同
```js
!!(null); // false
!!(undefined); // false
Number(null); // 0
Number(undefined); // NaN

null == undefined; //true
null === undefined; //false
```
<hr/>

#### 24. 常见的HTTP状态码
> ##### HTTP的响应状态码由5段组成： 

> 1xx 消息，一般是告诉客户端，请求已经收到了，正在处理，别急...
> 2xx 处理成功，一般表示：请求收悉、我明白你要的、请求已受理、已经处理完成等信息.
> 3xx 重定向到其它地方。它让客户端再发起一个请求以完成整个处理。
> 4xx 处理发生错误，责任在客户端，如客户端的请求一个不存在的资源，客户端未被授权，禁止访问等。
> 5xx 处理发生错误，责任在服务端，如服务端抛出异常，路由出错，HTTP版本不支持等。

> ##### 以下是几个常见的状态码： 
> * 200 OK   你最希望看到的，即处理成功！ 
> * 303 See Other  我把你redirect到其它的页面，目标的URL通过响应报文头的Location告诉你。 
> * 304 Not Modified 告诉客户端，你请求的这个资源至你上次取得后，并没有更改，你直接用你本地的缓存吧，我很忙哦，你能不能少来烦我啊！ 
> * 404 Not Found   你最不希望看到的，即找不到页面。如你在google上找到一个页面，点击这个链接返回404，表示这个页面已经被网站删除了，google那边的记录只是美好的回忆。 
> * 500 Internal Server Error   看到这个错误，你就应该查查服务端的日志了，肯定抛出了一堆异常，
<hr/>

#### 25. px和em的区别
> （1）px
> 像素px是相对于显示器屏幕分辨率而言的。(引自CSS2.0手册)

> （2）em
> em相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。(引自CSS2.0手册), em会继承父级元素的字体大小。
>任意浏览器的默认字体都是16px，所有未经调整的浏览器都符合: 1em=16px。
>为了简化font-size的换算，需要在css中的body选择器中声明font-size:62.5%;，这就使>em值变为 16px*62.5%=10px

> （3）rem
> rem是CSS3新增的一个相对单位（root em，根em）。
> 使用rem为元素设定字体大小时，仍然是相对大小，但相对的只是HTML根元素。
> 通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。
<hr/>

#### 26. 其他css方式设置垂直居中
> 1. display：flex-box
```css
.father{
  display:flex; 
  align-items:center;
  justify-content:center;
}
.son{
  width:200px; 
  height:100px; 
  background:red;
}
```
> 2. transform
```css
.father{
  position:relative;
}
.son{
  position:absolute;
  left:50%;top:50%;
  transform:translate(-50%,-50%);
}
```

#### 27. 手写数组扁平化函数

> 使用reduce()
```js
var myNewArray = myArray.reduce(function(prev, curr) {
  return prev.concat(curr);
});
```

> 使用 ES6 的展开运算符
```js
var myNewArray4 = [].concat(...myArray);
console.log(myNewArray4);
```

> 使用concat()和apply()
```js
var myNewArray = [].concat.apply([], myArray);
```
 #### 28. 类数组和数组的区别
 > ##### 类数组对象的定义：
 > *  1）拥有length属性，其它属性（索引）为非负整数（对象中的索引会被当做字符串来处理）；
 > *  2）不具有数组所具有的方法；
      javascript中常见的类数组有 arguments 对象和 DOM 方法的返回结果，比如
document.getElementsByTagName() ；

> ##### 类数组转换成数组
```js
// slice
var toArray = function(arrayLike){  
   try {  
       return Array.prototype.slice.call(arrayLike);  
   } catch(e){  
       var arr = [];  
       for(var i = 0,len = arrayLike.length; i < len; i++){  
          //arr.push(s[i]);  
          arr[i] = arrayLike[i];     //据说这样比push快
       }  
       return arr;  
    } 
}
```
```js
//ES6 Array.from
Array.from(arrayLike);
```
```js
//ES6 ...运算符 作为函数参数的时候可以吧arguments转换成数组
function translateArray(...arguments) {
    // ...
}
```