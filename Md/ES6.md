<h1 align="center">ES6</h1>

- [变量定义](#变量定义)
  - [let](#let)
  - [const](#const)
- [解构赋值](#解构赋值)
  - [数组解构赋值](#数组解构赋值)
  - [对象解构赋值](#对象解构赋值)
  - [字符串解构赋值](#字符串解构赋值)
  - [数值和布尔值解构赋值](#数值和布尔值解构赋值)
  - [函数参数使用解构赋值](#函数参数使用解构赋值)
  - [解构赋值常见用法](#解构赋值常见用法)
- [函数扩展](#函数扩展)
  - [箭头函数](#箭头函数)
- [Symbol](#symbol)
  - [Symbol 概述](#symbol-概述)
  - [Symbol 用法](#symbol-用法)
- [Set](#set)
  - [Set 概述](#set-概述)
  - [Set 实例属性和方法](#set-实例属性和方法)
  - [Set 用法](#set-用法)
- [Map](#map)
  - [Map 概述](#map-概述)
  - [Map 实例属性和方法](#map-实例属性和方法)
  - [Map 数据转换](#map-数据转换)

## 变量定义

### let

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
    let str2
  }
  ```

- 相同块级作用域内不能重复声明变量
- let 定义的变量无法用 window 对象调用

### const

- 声明一个只读的常量，并且声明的同时必须赋值
- 块级作用域内有效
- 不存在变量提升
- 存在暂时性死区
- 相同块级作用域内不能重复声明变量

## 解构赋值

### 数组解构赋值

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

### 对象解构赋值

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

  let { str1, str1: str2, str1: str3 } = { str1: 'hey man' ,str2: 'hi man'}
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

### 字符串解构赋值

- 转换为类似数组的对象

  ```js
  let [a, b, c, d, e] = 'hello'
  console.log(a) // "h"
  console.log(b) // "e"
  console.log(c) // "l"
  console.log(d) // "l"
  console.log(e) // "o"

  let { length: len } = 'hello'
  console.log(len) // 5
  ```

### 数值和布尔值解构赋值

- 转换为包装对象

  ```js
  let { toString: s } = 123
  console.log(s === Number.prototype.toString) // true

  let { toString: s } = true
  console.log(s === Boolean.prototype.toString) // true
  ```

### 函数参数使用解构赋值

- 可指定默认值

  ```js
  let arr = [
    [1, 3],
    [5, 7],
  ].map(([a, b]) => a * b)
  console.log(arr) // [3, 35]

  let arr = [{ x: 3 }, { y: 9 }].map(({ x = 4 }) => x * 2)
  console.log(arr) //

  let fn = ({ x, y } = { x: 1, y: 1 }) => {
    console.log([x, y])
  }
  fn({ x: 3 }) // [3, undefined]
  fn({}) // [undefined, undefined]
  fn() // [1, 1]
  ```

### 解构赋值常见用法

- 交换变量

  ```js
  let [a, b, c] = ([1, 2, 3][(a, b)] = [b, a])
  console.log(a, b)[(a, b, c)] = [c, a, b] // 2 1
  console.log(a, b, c) // 3 2 1
  ```

- 复制对象

  ```js
  let person1 = {
    name: 'mike',
    age: 18,
  }

  let person2 = { ...person1 }
  person2.age = 20

  console.log(person1.age) // 18
  ```

- 函数返回多个值

  ```js
  let fn1 = () => {
    return [1, 2, 3]
  }
  let [a, b, c] = fn1()

  let fn2 = () => {
    return { x: 5, y: 6 }
  }
  let { x, y } = fn2()
  ```

- 函数参数的传值
- 提取 JSON 数据结构赋值给多个变量
- 函数对象参数的默认值
- 调取模块对象的方法

## 函数扩展

### 箭头函数

- 定义
  
  ```js
  let fn1 = () => {
    console.log('hello world')
  }

  fn1() // "hello world"
  ```

- 箭头函数没有 argument 属性，也没有 caller 和 callee 属性
- 箭头函数内的 this 指向函数定义时所在对象，而非调用时所在对象
- 箭头函数没有 prototype 属性，不可以用作构造函数

  ```js
  let People = () => {
    this.name = 'bob'
  }

  console.log(Fn.prototype) // undefined
  let people1 = new People() // Error: People is not a constuctor
  ```

- 箭头函数有 constructor 属性，向上存在原型链

  ```js
  let fn2 = () => { console.log('hi') }

  console.log(fn2.contructor) // Function()
  ```

## Symbol

### Symbol 概述

- 定义：第七种原始数据类型，值具有唯一性

  ```js
  let sym1 = Symbol()
  console.log(sym1) // Symbol()
  console.log(typeof sym1) // "symbol"
  // 可以显示转换为字符串
  console.log(String(sym1)) // "Symbol()"
  // 可以转为布尔值
  console.log(Boolean(sym1)) // true
  ```

- 添加描述与 Symbol.prototype.description

  ```js
  let sym1 = Symbol('test')
  console.log(sym1) // Symbol(test)
  console.log(sym1.toString()) // "Symbol(test)"
  console.log(sym1.description) // "test"
  ```

### Symbol 用法

- 作为对象属性名

  ```js
  let name = Symbol()
  let age = Symbol()
  let person = {
    [name]: 'bob',
  }
  person[age] = 20
  ```

## Set

### Set 概述

- 定义：一种类似数组的数据结构，成员的值唯一

  ```js
  const set1 = new Set()
  console.log(typeof set1) // "object"

  const arr = [1, 2, 2, 3, 3, 4, 5]
  arr.forEach((item) => {
    set1.add(item)
  })
  console.log(set1) // Set(5) {1, 2, 3, 4, 5}
  ```

- Set()构造函数传参初始化，参数需有 iterable 接口

  ```js
  const set1 = new Set([1, 2, 3, 4, 4, 4])
  console.log(set1) // Set(4) {1, 2, 3, 4}

  const set2 = new Set('hello')
  console.log(set2) // Set(4) {'h', 'e', 'l', 'o'}

  const set2 = new Set([NaN, NaN, {}, {}])
  console.log(set2) // Set(3) {NaN, {}, {}}
  ```

### Set 实例属性和方法

- size 属性：返回实例的成员数

  ```js
  let set1 = new Set([1, 2, 3, 4, 4, 4])
  console.log(set1.size) // 4
  ```

- 操作成员方法

  ```js
  let set1 = new Set([1, 2, 3, 4])

  // 添加成员
  set1.add(5)
  console.log([...set1]) // [1, 2, 3, 4, 5]

  // 删除成员
  set1.delete(2)
  console.log([...set1]) // [1, 3, 4, 5]

  // 查找成员
  console.log(set1.has(3)) // true
  console.log(set1.has(2)) // false

  // 清空成员
  set1.clear()
  console.log(set1) // Set(0) {}
  ```

- 遍历成员方法

  ```js
  let set1 = new Set([1, 2, 3])

  console.log(typeof set1.keys()) // "object"
  console.log(typeof set1.values()) // "object"
  console.log(typeof set1.entries()) // "object"

  // 使用 for...of 可遍历返回的遍历器对象
  // Set 结构的 keys() 和 values() 返回相同
  for (let item of set1.keys()) {
    console.log(item)
  }
  // 1
  // 2
  // 3

  for (let item of set1.entries()) {
    console.log(item)
  }
  // [1, 1]
  // [2, 2]
  // [3, 3]

  // 直接遍历 Set 数据，等同于 set.values()
  for (let item of set1) {
    console.log(item)
  }
  // 1
  // 2
  // 3

  // forEach() 方法遍历，注意参数顺序
  set1.forEach((value, key) => {
    console.log(`${key}:${value}`)
  })
  // 1:1
  // 2:2
  // 3:3
  ```

### Set 用法

- 数组或字符串去重

  ```js
  let arr = [1, 2, 2, 3, 4, 4]
  let arr1 = [...new Set(arr)] // 数组去重

  // Array.from()方法可以将Set数据转换为数组
  let arr2 = Array.from(new Set(arr)) // 数组去重

  let str = 'hello'
  let str1 = [...new Set(str)].join('') // 字符串去重
  ```

- 实现并集，交集，差集

  ```js
  let set1 = new Set([1, 2, 3])
  let set2 = new Set([3, 4, 5])

  let setUnion = new Set([...set1, ...set2])
  console.log([...setUnion]) // [1, 2, 3, 4, 5]

  let setIntersect = new Set([...set1].filter((item) => set2.has(item)))
  console.log([...setIntersect]) // [3]

  let setDifference = new Set([...set1].filter((item) => !set2.has(item)))
  console.log([...setDifference]) // [1, 2]
  ```

## Map

### Map 概述

- 定义：一种类似对象的数据结构，键值对的键可以是各种类型的值

  ```js
  let map1 = new Map()
  console.log(typeof map1) // "object"

  let obj = { person: 'Bob' }
  map1.set(obj, 'teacher')
  map1.set(obj, 'student')
  map1.get(obj) // "student"
  map1.get(Obj) // undefined
  ```

- 引用类型的键保存的是指针

  ```js
  let map2 = new Map()
  map2.set(['hi'], 123)
  map2.get(['hi']) // undefined

  map2.set(null, 111)
  map2.set(undefined, 222)
  map2.get(null) // 111

  map2.set(NaN, 333)
  map2.get(NaN) // 333
  ```

- Map()构造函数传参初始化，参数需有 iterable 接口，且参数的每个成员为双元素数组

  ```js
  let obj1 = {
    person1: 'bob',
  }
  let obj2 = {
    person2: 'mike',
  }
  let map3 = new Map([
    [obj1, teacher],
    [obj2, student],
  ])

  map3.get(obj1) // "teacher"
  map3.get(obj2) // "student"

  obj1 = 'hello'
  map3.get(obj1) // undefined
  obj2.person2 = 'jack'
  map3.get(obj2) // "student"
  ```

### Map 实例属性和方法

- size 属性：返回实例的成员数

  ```js
  let map1 = new Map()
  map1.set('name', 'bob')
  map1.set('job', 'student')
  console.log(map1.size) // 2
  ```

- 操作键值方法

  ```js
  let map1 = new Map()

  // 设置键值
  map1.set('person', 'bob').set(undefined, 'error')

  // 获取对应键的值
  map1.get(undefined) // 'error'

  // 查找是否包含键
  map1.has(null) // false
  map1.has(undefined) // true

  // 删除键
  map1.delete(undefined) // true
  map1.delete('name') // false

  // 清空键值
  map1.clear()
  console.log(map1.size) // 0
  ```

- 遍历键值方法

  ```js
  let map1 = new Map([
    [{ name: 'mike' }, 'teacher'],
    [{ name: 'bob' }, 'student'],
  ])

  for (let key of map1.keys()) {
    console.log(key)
  }
  // { name: 'mike' }
  // { name: 'bob' }

  for (let value of map1.values()) {
    console.log(value)
  }
  // "teacher"
  // "student"

  for (let item of map1.entries()) {
    console.log(item)
  }
  // [{ name: 'mike' }, 'teacher']
  // [{ name: 'bob' }, 'student']

  for (let [key, value] of map1.entries()) {
    console.log(key)
    console.log(value)
  }
  // { name: 'mike' }
  // 'teacher'
  // { name: 'bob' }
  // 'student'

  // 直接遍历 Map 数据，等同于 map.entries()
  for (let [key, value] of map1) {
    console.log(key, value)
  }
  // { name: 'mike' } 'teacher'
  // { name: 'bob' } 'student'

  // forEach() 方法遍历，注意参数顺序
  map1.forEach((value, key, map) => {
    console.log(key, value)
    console.log(map)
  })
  // { name: 'mike' } teacher
  // Map(2) { { name: 'mike' } => teacher, { name: 'bob' } => student }
  // { name: 'bob' } student
  // Map(2) { { name: 'mike' } => teacher, { name: 'bob' } => student }
  ```

### Map 数据转换

- 与数组互转

  ```js
  let map1 = new Map([
    ['bob', 18], 
    ['mike', 20]
  ])
  let map2 = new Map(
    [...map1].filter((key, value) => {
      return value === 18
    })
  )
  console.log(map2) // Map(1) { 'bob' => 18 }
  ```

- 与对象互转，键必须为字符串

  ```js
  let map1 = new Map([
    ['bob', 18],
    ['mike', 20]
  ])

  let obj = {}
  for (let [key, value] of map1) {
    obj[key] = value
  }

  let map2 = new Map(Object.entries(obj))
  ```
