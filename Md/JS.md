<h1 align="center">JS</h1>

## \<script\>

概念

- 从上而下顺序解析
- 后一组 script 标签可使用前一组的内容，反之不行
- 解析 script 标签内容会阻塞 DOM 加载及渲染

属性

- async：异步加载，只对外部脚本有效，不同异步脚本标签无法保证顺序解析
- defer：DOM 加载并渲染后执行，只对外部脚本有效
- src：外部脚本源
- type：指定脚本内容 MIME 类型，默认 text/javascript
- charset：设置字符集，少用

## 语法

标识符

- 首字符：字母 或 \_ 或 \$（不能数字）
- 其它字符：字母 或 \_ 或 \$ 或 数字
- 大小写敏感

注释

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

数据检测

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

Undefined / Null

- undefined 派生自 null

  ```js
  undefined == null // true
  undefined === null // false
  ```

Boolean

- 转换为 true
  - 非零数字
  - 非空字符串
  - 非 null 对象
- 转换为 false
  - 0 和 NaN
  - ""空字符串
  - null
  - undefined

Number

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

String

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
