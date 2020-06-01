<h1 align="center">JS</h1>

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
  alert(undefined == null) // true
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
- 浮点数值计算会产生舍入误差
  ```js
  alert(0.1 + 0.2 == 0.3) // false
  ```
-
