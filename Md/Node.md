<h1 align="center">Node</h1>

- [nodemon 安装](#nodemon-安装)
- [模块化与 CommonJS](#模块化与-commonjs)
  - [exports 与 module.exports](#exports-与-moduleexports)
  - [require](#require)
  - [模块缓存](#模块缓存)

## nodemon 安装

> 使用 nodemon 启动的服务可热重载

- 全局安装 nodemon

  `npm i nodemon -g`

## 模块化与 CommonJS

### exports 与 module.exports

- exports 是模块中保存 module.exports 指引的变量
- exports 在模块中重新赋值会切断和 module.exports 的联系

### require

- 加载规则
  
  ```js
  // 绝对路径加载
  const test = require('/test.js')

  // 相对路径加载
  const test = require('./test.js')

  // 加载 node 模块，从局部到全局的node_modules 中加载
  const test = require('test.js')
  const test = require('test') // .js 可省略
  const _math = require('mathjs/lib/browser/math.js') // npm i mathjs
  ```

- 目录加载规则

  ```js
  // 加载 mathjs 目录下 package.json 文件内 main 字段指定文件
  // 若无 main 字段或无 package.json 文件则加载 mathjs 目录下的 index.js
  const _math = require('mathjs') 
  ```

- 加载机制

  ```js
  // 加载模块的值是模块输出值的拷贝
  // 加载拷贝的值后，模块内部的变化不会影响该拷贝值
  
  // moduleA.js
  let num = 1
  function counter() {
    num++
  }
  module.exports = {
    num: num,
    counter: counter
  }

  // index.js
  let num = require(./moduleA.js).num
  const counter = require(./moduleA.js).counter

  console.log(num) // 1
  counter()
  console.log(num) // 1
  ```

### 模块缓存

  - 加载一个模块后会将该模块缓存，下次加载该模块会取出缓存
  - 删除模块缓存
    ```js
    // 删除指定模块缓存，如 mathjs 模块
    delete require.cache['mathjs']

    // 删除所有模块缓存
    Object.keys(require.cache).forEach((key) => {
      delete require.cache[key]
    })
    ```

