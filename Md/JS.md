<h1 align="center">JS</h1>

- [\<script\>](#script)
  - [概念](#概念)
  - [属性](#属性)
- [语法](#语法)
  - [标识符](#标识符)
  - [注释](#注释)
- [数据类型](#数据类型)
  - [数据检测](#数据检测)
  - [Undefined / Null](#undefined--null)
  - [Boolean](#boolean)
  - [Number](#number)
  - [String](#string)
- [引用类型](#引用类型)
  - [Object](#object)
  - [Array](#array)
  - [Date](#date)
  - [RegExp](#regexp)
  - [Boolean, Number, String](#boolean-number-string)
  - [Global, Math](#global-math)
- [函数](#函数)
  - [this](#this)
  - [arguments](#arguments)
  - [call, apply, bind](#call-apply-bind)

## \<script\>

### 概念

- 从上而下顺序解析
- 后一组 script 标签可使用前一组的内容，反之不行
- 解析 script 标签内容会阻塞 DOM 加载及渲染

### 属性

- async：异步加载，只对外部脚本有效，不同异步脚本标签无法保证顺序解析
- defer：DOM 加载并渲染后执行，只对外部脚本有效
- src：外部脚本源
- type：指定脚本内容 MIME 类型，默认 text/javascript
- charset：设置字符集，少用

## 语法

### 标识符

- 首字符：字母 或 \_ 或 \$（不能数字）
- 其它字符：字母 或 \_ 或 \$ 或 数字
- 大小写敏感

### 注释

- 普通注释

  ```js
  // 单行注释

  /*
    多行注释
  */
  ```

- 未完成注释

  ```js
  // TODO 待实现或待完善功能
  ```

- 文件注释（全英文）

  ```js
  /*!
   * 概要说明 - 版本
   * 项目地址 | 开源协议
   */
  ```

- 文档注释

  ```js
  /**
   * 模块注释
   * @module 模块名
   *
   * 类注释
   * @class 类名
   * @constructor / @static
   * @param {参数类型} 参数名 参数说明
   *
   * 方法注释
   * @method 方法名
   * @for 所属类名
   * @param {参数类型} 参数名 参数说明
   * @return {返回值类型} 返回值说明
   */
  ```

## 数据类型

### 数据检测

- typeof

  ```js
  var val1
  var val2 = null
  var val3 = function () {}

  alert(val0) // 报错
  typeof val0 // "undefined"
  typeof val1 // "undefined"
  typeof val2 // "object"
  typeof val3 // "function"
  ```

- instanceof

  ```js
  var arr = [1, 2, 3]
  var fn = function () {}
  var obj = null
  var reg = /abc/g

  console.log(arr instanceof Array) // true
  console.log(fn instanceof Function) // true
  console.log(fn instanceof Object) // true
  console.log(obj instanceof Object) // false
  console.log(reg instanceof RegExp) // true
  ```

### Undefined / Null

- undefined 派生自 null

  ```js
  undefined == null // true
  undefined === null // false
  ```

### Boolean

- 转换为 true
  - 非零数字
  - 非空字符串
  - 非 null 对象
- 转换为 false
  - 0 和 NaN
  - ""空字符串
  - null
  - undefined

### Number

- +0 和 -0 相等

  ```js
  console.log(+0 === -0) // true
  ```

- 浮点数值计算会产生舍入误差

  ```js
  console.log(0.1 + 0.2 == 0.3) // false
  ```

- NaN 及 isNaN()

  ```js
  console.log(NaN == NaN) // false

  isNaN('10') // false 隐性转换为数值10
  isNaN(true) // false 隐性转换为数值1
  ```

- Number() 转换

  ```js
  Number(true) // 1
  Number(false) // 0
  Number(null) // 0
  Number(undefined) // NaN
  Number('') // 0
  Number('hello') // NaN
  Number('007') // 7
  Number('hello007') // NaN
  ```

- parseInt() 和 parseFloat() 转换

  ```js
  parseInt(true) // NaN
  parseInt(null) // NaN
  parseInt(undefined) // NaN
  parseInt('') // NaN
  parseInt('007hello888') // 7
  parseInt('007.88') // 7
  parseInt('070') // 70
  parseInt(070) // 56 (八进制)
  parseInt('070', 8) // 56 (八进制)
  parseInt('0xf') // 15 (十六进制)
  parseInt(0xf) // 15 (十六进制)
  parseInt('af', 16) // 15 (十六进制)

  parseFloat('007.88hello') // 7.88
  parseFloat('007.88.99') // 7.88
  parseFloat('0xf') // 0 (只解析十进制)
  ```

### String

- toString() 和 String()

  ```js
  var test
  var obj = null
  var boo = true
  var num = 10

  test.toString() // 报错
  obj.toString() // 报错
  boo.toString() // 'true'
  num.toString() // '10'
  num.toString(8) // '12' (根据八进制转换)
  num.toString(16) // 'a' (根据十六进制转换)

  String(test) // 'undefined'
  String(obj) // 'null'
  ```

## 引用类型

### Object

- 对象字面量定义对象时，不会调用 Object 构造函数

### Array

- 数组字面量定义数组时，不会调用 Array 构造函数
- Array对象的方法
  ```js
  // 判断是否是数组，返回布尔值
  Array.isArray([]) // true

  // 将一个伪数组或可迭代对象浅拷贝为一个数组，返回新数组
  Array.from('hello') // ['h', 'e', 'l', 'l', 'o']
  Array.from([1, 2, 3], x => x * 2) // [2, 4, 6]

  // 根据参数创建一个新数组，返回新数组
  Array.of('x', 1, 2) // ['x', 1, 2]
  ```
- Array实例对象的方法
  ```js
  // .concat(arr1, arr2, ...)
  // 将一个数组和多个数组合并，返回新数组，原数组不变
  [1, 1].concat([2, 2], [1, 1]) // [1, 1, 2, 2, 1, 1]

  // .copyWithin(target[, start = 0[, end = this.length]])
  // 将 start 到 end 索引之间的成员复制到 target 索引位置并覆盖原成员
  // 返回新数组，原数组改变
  [1, 2, 3, 4, 5].copyWithin(1, 3, 4) // [1, 3, 3, 4, 5]

  // .fill(element[, start = 0[, end = this.length]])
  // 将 element 填充到 start 到 end 索引之间的成员并覆盖
  // 返回新数组，原数组改变
  [1, 2, 3].fill('x', 1) // [1, 'x', 'x']

  // .forEach(callbcak(element[, index[, array]]))
  // 给所有成员执行一次函数，原数组不变
  [1, 2, 3].forEach(ele => ele > 2? console.log(ele): 0) // 3

  // .map(callback(element[, index[, array]]))
  // 给所有成员执行一次函数，返回由每次函数返回值组成的新数组，原数组不变
  // 函数无返回值时返回由 undefined 组成的等长新数组
  [1, 2, 3].map(ele => ele * 2) // [2, 4, 6]

  // .every(callback(element[, index[, array]]))
  // 测试所有成员是否都符合函数条件，返回布尔值，原数组不变
  [1, 2, 3].every(ele => ele < 8) // true

  // .filter(callback(element[, index[, array]]))
  // 测试所有成员是否符合函数条件，返回由符合条件成员组成的新数组，原数组不变
  // 都不符合时返回空数组 []
  [1, 2, 3, 4, 5].filter(ele => ele < 4) // [1, 2, 3]

  // .find(callback(element[, index[, array]]))
  // 逐个测试成员是否符合函数条件，返回第一个符合条件的成员，原数组不变
  // 都不符合时返回 undefined
  [1, 2, 3, 4, 5].find(ele => ele > 3) // 4

  // .findIndex(callback(element[, index[, array]]))
  // 逐个测试成员是否符合函数条件，返回第一个符合条件的成员索引，原数组不变
  // 都不符合时返回 -1
  [1, 2, 3, 4, 5].findIndex(ele => ele > 3) // 3

  // .flat([depth = 1])
  // 根据 depth 值将多维度数组扁平化
  [1, 2, [3, [4, 5]]].flat() // [1, 2, 3, [4, 5]]
  [1, 2, [3, [4, 5]]].flat(2) // [1, 2, 3, 4, 5]

  // .flatMap(callback(element[, index[, array]]))
  // 对原数组执行 .map()，再执行 .flat(1)，原数组不变
  [1, 2].flatMap(ele => [ele, ele * 10]) // [1, 10, 2, 20]

  // .entries()
  // 返回数组各个键值对构成的迭代器对象
  ['a', 'b', 'c'].entries() // Array Iterator

  // .keys()
  // 返回数组各个键构成的迭代器对象
  ['a', 'b', 'c'].keys() // Array Iterator

  // .values()
  // 返回数组各个值构成的迭代器对象
  ['a', 'b', 'c'].values() // Array Iterator


  ```

### Date

### RegExp

### Boolean, Number, String

### Global, Math

## 函数

### this

- 在普通模式下，函数执行上下文（this）默认指向 window
- 在普通模式下，将 this 指向 undefined 和 null 时，一样会指向 window
- 在严格模式下，this 默认指向 undefined
- 在严格模式下，将 this 指向 undefined 和 null 时，则 this 对应改变

### arguments

- 参数对象（伪数组）：可以索引访问成员，有 length，没有 Array 属性和方法
- arguments 的长度取决于调用函数时传入的参数数量，而非定义函数时的命名参数
- arguments 成员改变会反应到对应命名参数，前提是该命名参数在 arguments 长度范围内

  ```js
  function getSum(num1, num2, num3) {
    arguments[2] = 90
    let sum1 = num1 + num2 + num3
    num3 = 1
    let sum2 = num1 + num2 + num3
    console.log('sum1: ' + sum1)
    console.log('sum2: ' + sum2)
  }
  getSum(1, 1)
  // 'sum1: NaN'
  // 'sum2: 3'
  getSum(1, 1, 1)
  // 'sum1: 92'
  // 'sum2: 3'
  ```

### call, apply, bind

- 都是修改函数执行时的 this 指向
- call 和 bind 传入逐个参数，apply 传入参数数组
- call 和 apply 改变后执行，bind 改变后不执行但返回改变后的函数
