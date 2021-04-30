<h1 align="center">JS</h1>

- [\<script\>](#script)
  - [概念](#概念)
  - [属性](#属性)
- [语法](#语法)
  - [标识符](#标识符)
  - [注释](#注释)
- [数据类型](#数据类型)
  - [数据检测](#数据检测)
  - [Undefined/Null](#undefinednull)
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

### Undefined/Null

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
- Object 对象的方法
  ```js
  // Object.is(value1, value2)
  // 判断 value1 和 value2 是否为同一个值，返回布尔值
  Object.is(undefined, null) // false
  Object.is(0, +0) // true
  Object.is(0, -0) // false
  Object.is(NaN, NaN) // true

  // Object.assign(target, source1, source2, ...)
  // 将各个源对象的全部属性复制并覆盖到目标对象的属性中，返回目标对象
  // 后一个源对象相同属性会覆盖前一个的，源对象不变，目标对象改变
  Object.assign({a: 1}, {a: 11, b: 22}, {a: 0}) // {a: 0, b: 22}

  // Object.create(protoObject)
  // 创建一个原型为 protoObject 的对象，返回新对象
  const obj = Object.create({job: 'programmer'})
  console.log(obj) // {}
  console.log(obj.__proto__) // {job: "programmer"}

  // Object.defineProperty(obj, prop, option)
  // 为对象定义或修改一个属性，返回该对象，原对象改变
  const obj = {name: 'bob'}
  Object.defineProperty(obj, job, {
    value: 'programmer',
    configurable: true,
    enumerable: true,
    writable: true,
    // get() { return anotherValue },
    // set(newValue) { value = newValue }
  })

  // Object.defineProperties(obj, {prop1: option1, prop2: option2})
  // 为对象定义或修改多个属性，返回该对象，原对象改变
  const obj = {name: 'bob'}
  Object.defineProperties(obj, {
    job: {
      value: 'programmer',
      writable: true
    },
    age: {
      value: 22,
      enumerable: true
    }
  })

  // Object.entries(obj)
  // 返回对象各个键值对构成的二维数组
  Object.entries({
    name: 'bob',
    age: 18
  }) // [["name", "bob"], ["age", 18]]

  // Object.fromEntries(iterable)
  // Object.entries() 的逆操作，返回 iterable 构建的新对象
  Object.fromEntries([
    ['name', 'bob'],
    ['age', 18]
  ]) // {name: "bob", age: 18}

  // Object.keys(obj)
  // 返回对象各个键构成的数组
  Object.keys({name: 'bob', age: 18}) // ["name", "age"]

  // Object.values(obj)
  // 返回对象各个值构成的数组
  Object.values({name: 'bob', age: 18}) // ["bob", 18]

  // Object.freeze(obj)
  // 冻结对象，使属性均不能增删改，返回冻结后的对象，原对象改变
  const obj = {name: 'bob'}
  Object.freeze(obj) // {name: "bob"}
  obj.age = 18
  delete obj.name
  console.log(obj) // {name: "bob"}

  // Object.isFrozen(obj)
  // 判断一个对象是否被冻结，返回布尔值
  const obj = {name: 'bob'}
  Object.freeze(obj)
  Object.isFrozen(obj) // true

  // Object.preventExtensions(obj)
  // 使对象属性不能增只能删改，返回原对象，原对象改变
  const obj = {name: 'bob', age: 18}
  Object.preventExtensions(obj) // {name: "bob", age: 18}
  obj.name = 'mike'
  obj.job = 'student'
  delete obj.age
  console.log(obj) // {name: "mike"}

  // Object.isExtensible(obj)
  // 判断一个对象是否可以扩展，返回布尔值
  const obj = {name: 'bob'}
  Object.preventExtensions(obj)
  Object.isExtensible(obj) // false

  // Object.seal(obj)
  // 封闭对象，使属性不能增删，不能修改属性描述符，返回原对象，原对象改变
  const obj = {name: 'bob', age: 18}
  Object.seal(obj)
  obj.job = 'student'
  obj.name = 'mike'
  delete obj.age
  console.log(obj) // {name: "mike", age: 18}

  // Object.isSealed(obj)
  // 判断一个对象是否被封闭，返回布尔值
  const obj = {name: 'bob'}
  Object.seal(obj)
  Object.isSealed(obj) // true
  
  ```

### Array

- 数组字面量定义数组时，不会调用 Array 构造函数
- Array 对象的方法
  ```js
  // Array.isArray(value)
  // 判断 value 是否是数组，返回布尔值
  Array.isArray([]) // true

  // Array.from(arrayLike[, mapCallback])
  // 将一个伪数组或可迭代对象浅拷贝为一个数组，返回新数组
  // 若传入回调函数，则成员经过函数处理再拷贝
  Array.from('hello') // ['h', 'e', 'l', 'l', 'o']
  Array.from([1, 2, 3], x => x * 2) // [2, 4, 6]

  // Array.of(element1, element2, ...)
  // 根据参数创建一个新数组，返回新数组
  Array.of('x', 1, 2) // ['x', 1, 2]
  ```
- Array实例对象的方法
  ```js
  // .concat(arr1, arr2, ...)
  // 将一个数组和多个数组合并，返回新数组，原数组不变
  [1, 1].concat([2, 2], [1, 1]) // [1, 1, 2, 2, 1, 1]
    
  // .slice(start = 0, end = this.length)
  // 将数组从 start 到 end 索引之间的成员截取，返回截取的新数组，原数组不变
  [1, 2, 3, 4, 5].slice(1, 4) // [2, 3, 4]

  // .copyWithin(target[, start = 0[, end = this.length]])
  // 将 start 到 end 索引之间的成员复制到 target 索引位置并覆盖原成员
  // 返回新数组，原数组改变
  [1, 2, 3, 4, 5].copyWithin(1, 3, 4) // [1, 4, 3, 4, 5]

  // .fill(element[, start = 0[, end = this.length]])
  // 将 element 填充到 start 到 end 索引之间的成员并覆盖
  // 返回新数组，原数组改变
  [1, 2, 3].fill('x', 1) // [1, 'x', 'x']
  
  // .reverse()
  // 将数组全部颠倒，返回新数组，原数组改变
  [1, 2, 3, 4].reverse() // [4, 3, 2, 1]

  // .splice(start[, deleteCount[, element1, element, ...]])
  // 数组从 start 索引位置开始删除 deleteCount 个成员并在该位置添加新成员
  // 返回被删除成员组成的数组，无删除则返回空数组，原数组改变
  [1, 2, 3, 4, 5].splice(1, 2, 'a') // [2, 3]
  // 原数组变为：[1, 'a', 4, 5]

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

  // .some(callback(element[, index[, array]]))
  // 测试所有成员是否至少有一个符合函数条件，返回布尔值，原数组不变
  // 空数组测试返回 false
  [1, 2, 3].some(ele => ele === 2) // true

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

  // .reduce(callback(accumulator, element[, index[, array]])[,init])
  // 将数组成员逐个经过函数处理，上个函数结果作为下个函数累计器，返回最终结果
  // init 值传入时，该值作为首个函数的 accumulator，
  // init 值为空时，数组首个成员代替作为首个函数的 accumulator，不作为 element
  // 空数组不传 init 值则报错，原数组不变
  [1, 2, 3].reduce((acc, ele) => acc + ele * 2) // 11
  [1, 2, 3].reduce((acc, ele) => acc + ele * 2, 0) // 12

  // .reduceRight()
  // 语法同 .reduce，执行顺序改为数组从后向前
  [1, 2, 3].reduceRight((acc, ele) => acc + ele * 2) // 9
  [1, 2, 3].reduceRight((acc, ele) => acc + ele * 2, 0) // 12

  // .flat([depth = 1])
  // 根据 depth 值将多维度数组扁平化，返回新数组，原数组不变
  [1, 2, [3, [4, 5]]].flat() // [1, 2, 3, [4, 5]]
  [1, 2, [3, [4, 5]]].flat(2) // [1, 2, 3, 4, 5]

  // .flatMap(callback(element[, index[, array]]))
  // 对原数组执行 .map()，再执行 .flat(1)，返回新数组，原数组不变
  [1, 2].flatMap(ele => [ele, ele * 10]) // [1, 10, 2, 20]

  // .includes(value[, start = 0])
  // 从 start 索引开始查找数组是否包含 value 值，返回布尔值
  [1, 2, 3, 2, 5].includes(2, 3) // true

  // .indexOf(element[, start = 0])
  // 从 start 索引开始往后查找数组是否存在 element 成员，返回成员对应索引
  // 查找到第一个就返回，成员不存在则返回 -1
  [1, 2, 3, 4, 5].indexOf(4, 3) // 3

  // .lastIndexOf(element[, start = this.length - 1])
  // 从 start 索引开始往前查找数组是否存在 element 成员，返回成员对应索引
  // 查找到第一个就返回，成员不存在则返回 -1
  [1, 2, 3, 2, 4, 2].lastIndexOf(2, 4) // 3

  // .join([separator])
  // 将数组所有成员用 separator 连接成字符串，返回新字符串，原数组不变
  // separator 不传则用逗号 , 连接
  ['a', 'b', 'c'].join() // "a,b,c"
  ['a', 'b', 'c'].join('') // "abc"

  // .push(element1, element2, ...)
  // 在数组尾部添加新成员，返回新数组的长度，原数组改变
  [1, 11].push(2, 22) // 4

  // .pop()
  // 将数组最后一个成员删除，返回删除的成员，原数组改变
  // 数组为空时，返回 undefined
  [1, 11, 2, 22].pop() // 22
  
  // .unshift(element1, element2, ...)
  // 在数组头部添加新成员，返回新数组的长度，原数组改变
  [2, 22].unshift(1, 11) // 4

  // .shift()
  // 将数组第一个成员删除，返回删除的成员，原数组改变
  // 数组为空时，返回 undefined
  [1, 11, 2, 22].shift() // 1

  // .entries()
  // 返回数组各个键值对构成的迭代器对象
  ['a', 'b', 'c'].entries() // Array Iterator

  // .keys()
  // 返回数组各个键构成的迭代器对象
  ['a', 'b', 'c'].keys() // Array Iterator

  // .values()
  // 返回数组各个值构成的迭代器对象
  ['a', 'b', 'c'].values() // Array Iterator

  // .sort([compareFunction(element1, element2)])
  // 对数组排序，原数组改变
  // 函数处理返回值小于0时，两成员位置不变，返回值大于0时，两成员位置互换
  [1, 3, 2, 4].sort((ele1, ele2) => ele2 - ele1) // [4, 3, 2, 1]

  // .toString()
  // 将数组转换成字符串，成员间用逗号连接，原数组不变
  [1, 2, 3, 4].toString() // "1,2,3,4"
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
