# 第二章 进入JavaScript
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
`typeof a`并不是在询问[a的类型]，而是[当前a中值的类型]

### 对象
- 属性访问
  - 点号标记法 obj.a
  - 方括号标记法 obj["a"]
  - <img src="fg1 object.png">
- 数组
  - ```var arr = ['hello', 100, true]; typeof arr //'object'```
  - 一个数组就是一个object，使用数字索引的位置
  - <img src="fg2 array.png">
- 函数
  ```javascript
    function foo(){ 
        return 42
    }
    foo.bar = "hello world"

    typeof foo; // "function"
    typeof foo(); // "number"
    typeof foo.bar; // "string"
  ```

### 内建类型的方法
`String` - 对象包装器形式 => 原生类型; 
`string` => 基本类型
`Srting` 与 `string` 配成一对 => 一个为string的基本类型被当做对象使用时，JS自动将这个值【封箱】为它对应的对象包装器String

### 值的比较
【明确】强制转换
```javascript
var a = "42"
var b = Number(a)
console.log(a) // "42"
console.log(b) // 42 -- 数字
```

【隐含】强制转换
```javascript
var a ="42"
var b = a * 1
```

### Truthy 与 Falsy
非boolean => boolean

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


