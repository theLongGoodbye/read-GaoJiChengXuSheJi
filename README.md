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
