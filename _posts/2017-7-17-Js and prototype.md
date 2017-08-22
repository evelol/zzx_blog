函数声明 函数表达式, 声明会提前, 只有表达式才可以立即调用(将声明转化为表达式), 但函数表达式没有被包含在变量/活动对象中, 除非赋值给了一个活动变量.  .  闭包 , 立即执行函数

	1. arguments 活动对象
	2. 函数表达式 不包含在 对象变量 和 活动变量之中.
	3. window对象, 大多情况就是this. 只有函数才能形成作用域?
	4. this的几种绑定方式.
	5. 关注时薪.....
	6. js 用new的方式定义对象. 为什么不用json对象方式呢?
	7. 3种创建类的: 工厂, 构造, 原型
	8. 函数对象(构造器对象 ) 函数属性
	9. 函数对象的prototype属性 和 普通对象的__proto__属性
	10. Function 是所有函数对象的基础, 而Object则是所有对象（包括函数对象）的基础.然而Object也使用prototype来访问自己的原型的, 类似于函数对象.

var Person = function(){};
var p = new Person();


new的过程拆分成以下三步：
(1) var p={}; 也就是说，初始化一个对象p
(2) p.__proto__ = Person.prototype;
(3) Person.call(p); 也就是说构造p，也可以称之为初始化p

关键在于第二步，我们来证明一下：
```javascript
var Person = function(){};
var p = new Person();
alert(p.__proto__ === Person.prototype);
```

发现,   实例的__proto__就是个普通的Object对象.
this是执行上下文的一个属性，而不是变量对象的一个属性;
这个特性非常重要，因为与变量相反，this从不会参与到标识符解析过程。换句话说，在代码中当访问this的时候，它的值是直接从执行上下文中获取的，并不需要任何作用域链查找。this的值只在进入上下文的时候进行一次确定


window对象,获取路径
---
var protocol = window.location.protocol;  // "http:"
var host = window.location.host;  //"localhost" or "www.baidu.com"
var pathname = window.location.pathname;  // "/dkrl/services/cases.php"
var href = window.location.href;  // "http://localhost/dkrl/services/cases.php?noId=7"
var search = window.location.search;  // "?noId=7"

利用js获取当前页名字
var strUrl = window.location.href;
var arrUrl = strUrl.split("/");
var strPage = arrUrl[arrUrl.length-1];
var indexof = strPage.indexOf("?");
if(indexof != -1){
  strPage = strPage.substr(0,strPage.indexOf("?"));
}
alert(strPage);
 


