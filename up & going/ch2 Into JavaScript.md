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
`<` `>` `<=` `>=`  [关系比较]
`string` 不等价比较 使用字母顺序

```javascript
var a = 41
var b = '42'
var c = '43'
a < b //true => 两个值会强制转换成number
b < c //true
```

```javascript
var a = 42
var b = 'foo'
a > b //false
a < b //false
a == b //false => 42 == Nan or "42" == "foo"
// b 被转换成 Nan => Nan既不大于也不小于其他值
```
