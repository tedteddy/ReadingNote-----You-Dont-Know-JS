# 第二章 进入 JavaScript

## 值与类型

内建类型
`string`
`number`
`boolean`
`null` 和 `undefined`
`object`
`symbol`

```
    var a
    a = null
    typeof a // "object"  => bug 且 注意双引号
```

`typeof a`并不是在询问[a 的类型]，而是[当前 a 中值的类型]

### 对象

- 属性访问
  - 点号标记法 obj.a
  - 方括号标记法 obj["a"]
  - <img src="fg1 object.png">
- 数组
  - `var arr = ['hello', 100, true]; typeof arr //'object'`
  - 一个数组就是一个 object，使用数字索引的位置
  - <img src="fg2 array.png">
- 函数

  ```javascript
  function foo() {
    return 42;
  }
  foo.bar = "hello world";

  typeof foo; // "function"
  typeof foo(); // "number"
  typeof foo.bar; // "string"
  ```

### 内建类型的方法

`String` - 对象包装器形式 => 原生类型;
`string` => 基本类型
`Srting` 与 `string` 配成一对 => 一个为 string 的基本类型被当做对象使用时，JS 自动将这个值【封箱】为它对应的对象包装器 String

### 值的比较

【明确】强制转换

```javascript
var a = "42";
var b = Number(a);
console.log(a); // "42"
console.log(b); // 42 -- 数字
```

【隐含】强制转换

```javascript
var a = "42";
var b = a * 1;
```

### Truthy 与 Falsy

非 boolean => boolean

- falsy
  - ""
  - 0, -0, NaN
  - null, undefined
  - false
- truthy
  - "hello"
  - 42
  - true
  - [], [1,'2',3]
  - {},{a: 42}
  - function foo(){ .. }

### 等价性

`==` 在允许强制转换的条件下检查值的等价性
`===` 允许强制转换的条件下检查值的等价性 【严格等价】
`!=` `!==`

```javascript
var a = "1";
var b = 1;
a == b; //true  => 流程 ‘1’ 变成 1 进而对比 1 == 1
a === b; //false
```

- 规则总结
  - 如果一个比较的两个值之一可能是 true 或 false 值，避免`==` 而使用 `===`。
  - 如果一个比较的两个值之一可能是这些具体的值（0，""，[]），避免`==`而使用`===`。
  - 在 所有 其他情况下，你使用==是安全的。它不仅安全，而且在许多情况下它可以简化你的代码并改善可读性。

对比非基本类型的值 `object` 包括 `function` `array`
这些值实际上是通过引用持有的，`==`和`===`比较都将简单地检查这个引用是否相同，而不是它们底层的值

```javascript
`array` 默认情况下会通过使用[,]连接所有值来被强制转换为`string`
var a = [1,2,3]
var b = [1,2,3]
var c = '1,2,3'
a == c   //true
b == c   //true
a == b   //false
```

### 不等价性

`<` `>` `<=` `>=` [关系比较]
`string` 不等价比较 使用字母顺序

```javascript
var a = 41;
var b = "42";
var c = "43";
a < b; //true => 两个值会强制转换成number
b < c; //true
```

```javascript
var a = 42;
var b = "foo";
a > b; //false
a < b; //false
a == b; //false => 42 == Nan or "42" == "foo"
// b 被转换成 Nan => Nan既不大于也不小于其他值
```

### 变量

保留字 [reserved words] 不可用作变量名
`a`-`z` `A`-`Z` `$` `_` 开头

### 函数作用域

`var` 关键字声明 => 作用于 当前函数作用域
声明位于任何函数外部的顶层 => 全局作用域

- 提升
  - 无论 var 出现在一个作用域内部的何处，这个声明都被任务是属于整个作用域的，而且在作用域的所有位置都是可以访问的

```javascript
var a = 2;
foo(); //可以工作 下方 `foo()`的声明被【提升】了
function foo() {
  a = 3;
  console.log(a); // 3
  var a; // 此处a的声明被提升到`foo()`内的最顶端
}
console.log(a); // 2
```

- 嵌套作用域
  - 声明一个变量时，在作用域内的任何地方都是可用的，包括任何下层、内部作用域

```javascript
function foo() {
  var a = 1;
  function bar() {
    var b = 2;
    function baz() {
      var c = 3;
      console.log(a, b, c); // 1 2 3
    }
    baz(); //1 2 3
    console.log(a, b); // 1,2
  }
  bar(); // 1,2
  console.log(a); //1
}
foo(); //1
```

【错误示例】
在一个作用域内访问一个不可用的变量的值 => `ReferenceError`
为一个还没有声明的变量赋值 => 根据 strict 模式的状态 得到一个在顶层全局作用域中创建的变量 or 得到一个错误

```javascript
function foo() {
  a = 1; //`a`没有被正式声明
}
foo();
a; // 1 => 自动全局变量

//非常差劲！！！
```

- ES6 let

### 条件

```javascript
switch (1) {
  case 1:
    console.log(1);
  case 10:
    console.log(10);
} // 1 10

switch (1) {
  case 1:
  case 10:
    console.log(1, 10);
} //1 10

switch (1) {
  case 1:
    console.log(1);
    break;
  case 10:
    console.log(10);
    break;
} // 1
```

如果你在一个`case`中省略了`break`，并且这个`case`成立或运行，那么程序的执行将会不管下一个`case`

- 三元操作符
  - var b = (a > 41) ? "hello" : "world";

### Strict 模式 `终于了解到了`

ES5 在语言中加入了一个“strict 模式”，它收紧了一些特定行为的规则。一般来说，这些限制被视为使代码符合一组更安全和更合理的指导方针。另外，坚持 strict 模式一般会使你的代码对引擎有更强的可优化性。strict 模式对代码有很大的好处，你应当在你所有的程序中使用它。

```javascript
function foo() {
  "use strict";
  // 这部分代码是strict模式的
  function bar() {
    // 这部分代码是strict模式的
  }
}
// 这部分代码不是strict模式的
```

```javascript
"use strict";
function foo() {
  // 这部分代码是strict模式的
  function bar() {
    // 这部分代码是strict模式的
  }
}
// 这部分代码是strict模式的
```

```javascript
使用strict模式的一个关键`不同`or`改善`是，它不允许因为省略了var而进行隐含的自动全局变量声明：

function foo() {
	"use strict";	// 打开strict模式
	a = 1;			// 缺少`var`，ReferenceError
}

foo();
```

### 函数作为值

```javascript
function foo(){
  // ..
}

foo基本上是一个位于外围作用域的 `变量` 它给了被声明的function一个引用
function本身就是一个值
```

```javascript
var foo = function () {
  //。。
}; // 匿名函数表达式

var x = function bar() {
  // 。。
}; // 命名函数表达式  => x() 执行
```

### 立即被调用的函数表达式

```javascript
(function IIFE(){
  console.log('IIFE')
})()
//IIFE

外部的 `()` 防止函数表达式被看作一个普通的函数函数声明

var x = (function IIFE(){return 1})()

x // 1
```

```js
var a = 42;

(function IIFE(){
	var a = 10;
	console.log( a );	// 10
})();

console.log( a );		// 42

因为IIFE只是一个函数，而函数可以创建变量 作用域，以这样的风格使用一个IIFE经常被用于定义变量，而这些变量将不会影响围绕在IIFE外面的代码
```

###闭包
`即使函数已经完成了运行，它依然可以“记住”并持续访问函数的作用域`

```js
function makeAdder(x) {
  // 参数 `x` 是一个内部变量

  // 内部函数 `add()` 使用 `x`，所以它对 `x` 拥有一个“闭包”
  function add(y) {
    return y + x;
  }

  return add;
}

类似于 add(y) ==> makeAdder(x)(y)
每次调用外部的 `makeAdder(..)` 所返回的对内部 `add(..)函数的引用`可以记住被传入 `makeAdder(..)`的 `x`值
```

```js
// `plusOne` 得到一个指向内部函数 `add(..)` 的引用，
// `add()` 函数拥有对外部 `makeAdder(..)` 的参数 `x`的闭包
var plusOne = makeAdder(1);

// `plusTen` 得到一个指向内部函数 `add(..)` 的引用，
// `add()` 函数拥有对外部 `makeAdder(..)` 的参数 `x`的闭包
var plusTen = makeAdder(10);

plusOne(3); // 4  <-- 1 + 3
plusOne(41); // 42 <-- 1 + 41

plusTen(13); // 23 <-- 10 + 13
```

###模块
闭包常见的用法 模块模式
模块让你定义对外面世界不可见的私有实现细节（变量、函数），和对外面可访问的共有 API

```js
function User() {
  var username, password;
  function doLogin(user, pw) {
    username = user;
    password = pw;

    //做登录操作
  }
  var publicAPI = {
    login: doLogin,
  };

  return publicAPI;
}

// 创建一个 `User` 模块的实例
var fred = User();
fred.login("hello", "qwer1234");

//函数User()作为一个 外部作用域 持有变量 `username` 和 `password`，以及内部`doLogin()`函数；它们都是User模块内部的私有细节，是不能从外部世界访问的。
```

