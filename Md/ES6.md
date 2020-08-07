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

- 模式匹配
