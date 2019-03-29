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
理解原型对象
```
