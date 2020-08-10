<h1 align="center">ES6</h1>

## 变量定义

let

- 块级作用域内有效

  ```js
  var fnArr = []
  for (var i = 0; i < 10; i++) {
    fnArr[i] = function () {
      console.log(i)
    }
  }
  fnArr[3]() // 10

  let fnArr = []
  for (let i = 0; i < 10; i++) {
    fnArr[i] = () => {
      console.log(i)
    }
  }
  fnArr[3]() // 3

  for (let i = 0; i < 3; i++) {
    // 循环体头部为父作用域
    let i = 'hello' // 循环体内部为子作用域
    console.log(i)
  }
  // hello * 3
  ```

- 不存在变量提升
- 存在暂时性死区：块级内声明的变量绑定块级，形成封闭作用域

  ```js
  let str1 = 'hello'
  let str2 = 'hi'
  if (true) {
    console.log(str1) // 'hello'
    console.log(str2) // Error 形成封闭作用域后，无法调用父作用域的str，此为死区
    let str
  }
  ```

- 相同块级作用域内不能重复声明变量

const

- 声明一个只读的常量，并且声明的同时必须赋值
- 块级作用域内有效
- 不存在变量提升
- 存在暂时性死区
- 相同块级作用域内不能重复声明变量

## 解构赋值

数组解构赋值

- 按数组维度和顺序模式匹配

  ```js
  let [a, [[b], c]] = [1, [[2, 3], 4]]
  console.log(a) // 1
  console.log(b) // 2
  console.log(c) // 4

  let [...n] = [11, 22, 33]
  console.log(n) // [11, 22, 33]

  let [x, y, ...z] = ['a']
  console.log(x) // "a"
  console.log(y) // undefined
  console.log(z) // []
  ```

- 指定默认值

  ```js
  let [x = 1] = [undefined]
  console.log(x) // 1

  let [x = 1] = [null]
  console.log(x) // null

  function fn1() {
    console.log('hello')
    return 'hi'
  }

  let [str = fn1()] = [] // 'hello'
  console.log(str) // 'hi'

  let [str1 = fn1()] = ['hey'] // 赋值则不执行默认值内容，无打印
  console.log(str1) // 'hey'
  ```

对象解构赋值

- 按对象属性名模式匹配

  ```js
  let { str: str1, num: num1 } = { str: 'string', num: 123 }
  console.log(str1) // "string"
  console.log(num1) // 123

  let { name, age } = { age: 20, name: 'bob' }
  console.log(name) // "bob"
  console.log(age) // 20

  const { log } = console
  log('hello') // "hello"

  let { str1, str1: str2, str1: str3 } = { str1: 'hey man' ,str2 = 'hi man'}
  console.log(str1) // "hey man"
  console.log(str2) // "hey man"
  console.log(str3) // "hey man"

  const obj1 = {};
  const obj2 = { foo: 'bar' };
  Object.setPrototypeOf(obj1, obj2);

  const { foo } = obj1; // 继承的属性也可以解构赋值
  foo // "bar"

  let x
  {x} = {x: 1} // error {}位于行首会判定为代码块执行

  let x
  ({x} = {x: 1}) // 正确写法
  ```

- 指定默认值

  ```js
  let { x, y = 2 } = { x: 1 }
  console.log(x) // 1
  console.log(y) // 2

  let { x = 1, x: y = 2 } = { x: 9 }
  console.log(x) // 9
  console.log(y) // 9
  ```

字符串解构赋值

- 转换为类似数组的对象

  ```js
  let [a, b, c, d, e] = 'hello'
  console.log(a) // "h"
  console.log(b) // "e"
  console.log(c) // "l"
  console.log(d) // "l"
  console.log(e) // "o"

  let length: len = 'hello'
  console.log(len) // 5
  ```

数值和布尔值解构赋值

- 转换为包装对象

  ```js
  let { toString: s } = 123
  console.log(s === Number.prototype.toString) // true

  let { toString: s } = true
  console.log(s === Boolean.prototype.toString) // true
  ```

函数参数使用解构赋值

- 可指定默认值
