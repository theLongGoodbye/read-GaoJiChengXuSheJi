# read-GaoJiChengXuSheJi

如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为 null 而不是其他值。<br>

可以把 ECMAScript 函数的参数想象成局部变量。<br>

检测类型：
`基本类型 typeof 'a' === 'string'`
```
引用类型 
alert(person instanceof Object); // 变量 person 是 Object 吗？
alert(colors instanceof Array); // 变量 colors 是 Array 吗？
alert(pattern instanceof RegExp); // 变量 pattern 是 RegExp 吗？
(Array.isArray(value)) //判断是否是数组

```


```
在函数内部，有两个特殊的对象：arguments 和 this。其中，arguments 在第 3 章曾经介绍过，
它是一个类数组对象，包含着传入函数中的所有参数。虽然 arguments 的主要用途是保存函数参数，
但这个对象还有一个名叫 callee 的属性，该属性是一个指针，指向拥有这个 arguments 对象的函数。
function factorial(num){ 
 if (num <=1) { 
 return 1; 
 } else { 
 return num * arguments.callee(num-1) 
 } 
} 
```
```
字符串转数组
var colorText = "red,blue,green,yellow"; 

colorText.split() -> ["red,blue,green,yellow"]

colorText.split(',') -> ["red", "blue", "green", "yellow"]
```

```
理解原型对象 P148

无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个 prototype 属性，这个属性指向函数的原型对象（所有原型对象都会自动获得一个 constructor（构造函数）属性）

当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部属性），指向构造函数的原型对象。ECMA-262 第 5 版中管这个指针叫[[Prototype]]。虽然在脚本中没有标准的方式访问[[Prototype]]，但 Firefox、Safari 和 Chrome 在每个对象上都支持一个属性__proto__；而在其他实现中，这个属性对脚本则是完全不可见的。

每当代码读取某个对象的某个属性时，都会执行一次搜索，目标是具有给定名字的属性。搜索首先从对象实例本身开始。如果在实例中找到了具有给定名字的属性，则返回该属性的值；如果没有找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性。如果在原型对象中找到了这个属性，则返回该属性的值。也就是说，在我们调用 person1.sayName()的时候，会先后执行两次搜索。首先，解析器会问：“实例 person1 有 sayName 属性吗？”答：“没有。”然后，它继续搜索，再
问：“person1 的原型有 sayName 属性吗？”答：“有。”于是，它就读取那个保存在原型对象中的函数。当我们调用 person2.sayName()时，将会重现相同的搜索过程，得到相同的结果。而这正是多个对象实例共享原型所保存的属性和方法的基本原理
```
对象的 in 操作符：`a = {name: 'green'}` 'name' in a // true

```
ES5 创建对象：工厂模式 -> 构造函数模式 -> 原型模式 -> 组合使用构造函数模式（属性）和原型模式（方法）， 使用最广泛、认同度最高
```
```
ES5 实现继承：原型链（将父类的实例赋值给子类的原型） -> 借用构造函数（在子类中用 call 方法调用父类构造函数） -> 组合继承，在子类中用 call 方法调用父类构造函数，以继承属性；将父类的实例赋值给子类的原型，以继承方法
```
```
递归：一个函数通过名字调用自己
function factorial(num){ 
  if (num <= 1){ 
   return 1; 
  } else { 
   return num * arguments.callee(num-1); 
  } 
} 
```

```
闭包：是指一个函数有权访问另一个函数作用域中的变量
function createComparisonFunction(propertyName) { 
  return function(object1, object2){ 
  var value1 = object1[propertyName]; 
  var value2 = object2[propertyName]; 

  if (value1 < value2){ 
   return -1; 
  } else if (value1 > value2){ 
   return 1; 
  } else { 
   return 0; 
  } 
  }; 
} 
在匿名函数从 createComparisonFunction()中被返回后，它的作用域链被初始化为包含
createComparisonFunction()函数的活动对象和全局变量对象。这样，匿名函数就可以访问在
createComparisonFunction()中定义的所有变量。更为重要的是，createComparisonFunction()
函数在执行完毕后，其活动对象也不会被销毁，因为匿名函数的作用域链仍然在引用这个活动对象。换
句话说，当 createComparisonFunction()函数返回后，其执行环境的作用域链会被销毁，但它的活
动对象仍然会留在内存中
```
```
间歇调用和超时调用： 
var num = 0; 
var max = 10; 
var intervalId = null; 
function incrementNumber() { 
   num++; 
   //如果执行次数达到了 max 设定的值，则取消后续尚未执行的调用
   if (num == max) { 
     clearInterval(intervalId); 
     alert("Done"); 
   } 
  } 
intervalId = setInterval(incrementNumber, 500); 
----
function incrementNumber() { 
 num++; 
 //如果执行次数未达到 max 设定的值，则设置另一次超时调用
 if (num < max) { 
 setTimeout(incrementNumber, 500); 
 } else { 
 alert("Done"); 
 } 
} 
setTimeout(incrementNumber, 500); 
一般认为，使用超时调用来模拟间歇调用的是一种最佳模式。在开
发环境下，很少使用真正的间歇调用，原因是后一个间歇调用可能会在前一个间歇调用结束之前启动。
而像前面示例中那样使用超时调用，则完全可以避免这一点。所以，最好不要使用间歇调用。
```
```
DOM1 级将 HTML 和 XML 文档看作一个层次化的节点树
可以使用 JS 来操作这个节点树，进而改变底层文档的外观和结构。
DOM 操作往往是 JS 程序中开销最大的部分，应尽量减少
```
#### HTML5新特性
```
操作 DOM 的部分： querySelector()、querySelectorAll()、 classList
document.activeElement 属性始终会引用 DOM 中当前获得了焦点的元素

使用 data- 为元素添加非标准的属性
<div id="myDiv" data-appId="12345" data-myname="Nicholas"></div> 
//取得自定义属性的值
var appId = div.dataset.appId; 
var myName = div.dataset.myname; 
//设置值
div.dataset.appId = 23456; 
div.dataset.myname = "Michael"; 

innerHTML

//作为最后一个子元素插入
element.insertAdjacentHTML("beforeend", "<p>Hello world!</p>"); 
```
```
DOM0 级事件：将一个函数赋值给一个事件处理程序属性
var btn = document.getElementById("myBtn"); 
btn.onclick = function(){ 
 alert("Clicked"); 
}; 
```
```
DOM2级事件 定义了两个方法，addEventListener() 和 removeEventListener()
使用 事件冒泡， 可以最大限度地兼容各种浏览器
```
```
JSON 对象有两个方法：stringify()和 parse()。在最简单的情况下，这两个方法分别用于把
JavaScript 对象序列化为 JSON 字符串和把 JSON 字符串解析为原生 JavaScript 值。
```
```
状态代码为 304 表示请求的资源并没有被修改，可
以直接使用浏览器中缓存的版本；当然，也意味着响应是有效的。
```
```
CORS 背后的基本思想，就是使用自定义的 HTTP 头部
让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。
Origin: http://www.nczonline.net
Access-Control-Allow-Origin: http://www.nczonline.net
```
```
var self = this;
由于 setTimeout()中用到的函数的环境总是 window，所以有
必要保存 this 的引用以方便以后使用。
```
