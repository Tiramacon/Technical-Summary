<h1 align="center">Node</h1>

- [nodemon 安装](#nodemon-安装)
- [模块化与 CommonJS](#模块化与-commonjs)
  - [exports 与 module.exports](#exports-与-moduleexports)
  - [require](#require)
  - [模块缓存](#模块缓存)
- [fs 模块](#fs-模块)
  - [文件读写](#文件读写)
  - [文件删除和改名](#文件删除和改名)
  - [文件存在判断](#文件存在判断)
  - [目录新建、读取和删除](#目录新建读取和删除)
  - [获取目录或文件具体信息](#获取目录或文件具体信息)
  - [文件流式读写](#文件流式读写)
- [http 模块](#http-模块)
  - [http 服务构建](#http-服务构建)

## nodemon 安装

> 使用 nodemon 启动的服务可热重载

- 全局安装 nodemon

  `npm i nodemon -g`

## 模块化与 CommonJS

### exports 与 module.exports

- exports 是模块中保存 module.exports 指引的变量
- exports 在模块中重新赋值会切断对 module.exports 的指引

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

## fs 模块

> const fs = require('fs')

### 文件读写

- 读取文件数据
  ```js
  // 异步读取
  fs.readFile('test.txt', (err, data) => {
    if (err) throw err
    console.log(`读取到数据：${data}`) // 读取为 Buffer 实例
  })
  fs.readFile('test.txt','utf8', (err, data) => {
    if (err) throw err
    console.log(`读取到数据：${data}`) // 读取为 utf8 编码数据
  })
  // 同步读取
  let data = fs.readFileSync('test.txt')
  ```

- 写入文件数据
  ```js
  // 异步写入
  fs.writeFile('test.txt', 'hello', (err) => {
    if (err) throw err
    console.log('写入数据成功')
  })
  // 同步写入
  fs.writeFileSync('test.txt', 'hello')
  ```

### 文件删除和改名

- 文件删改
  ```js
  // 异步删除
  fs.unlink('test.txt', (err) => {
    if (err) throw err
    console.log('文件删除成功')
  })
  // 同步删除
  fs.unlinkSync('test.txt')
  ```

- 文件改名
  ```js
  // 异步改名
  fs.rename('test.txt', 'newTest.txt', (err) => {
    if (err) throw err
    console.log('文件改名成功')
  })
  // 同步改名
  fs.renameSync('test.txt', 'newTest.txt')
  ```

### 文件存在判断

- 判断文件存在
  ```js
  // 异步判断
  fs.exists('test.txt', (exists) => {
    console.log(`文件是否存在：${exists}`)
  }) 
  // 同步判断
  if (fs.exists('test.txt')) {
    console.log('文件存在')
  }
  ```

### 目录新建、读取和删除

- 新建目录
  ```js
  // 异步新建
  fs.mkdir('newDir', (err) => {
    if (err) throw err
  })
  // 同步新建
  fs.mkdirSync('newDir')
  ```

- 读取目录
  ```js
  // 异步读取
  fs.readdir('newDir', (err, files) => {
    // 获取包含子文件或子目录的数组
    if (err) throw err
    files.forEach((item) => {
      console.log(item)
    })
  })
  // 同步读取
  let files = fs.readdirSync('newDir')
  files.forEach((item) => {
    console.log(item)
  })
  ```

- 删除目录
  ```js
  // 异步删除
  fs.rmdir('oldDir', (err) => {
    if (err) throw err
  })
  // 同步删除
  fs.rmdirSync('oldDir')
  ```

### 获取目录或文件具体信息

- 获取具体信息
  ```js
  // 异步获取
  fs.stat('test.txt', (err, stat) => {
    if (err) throw err
    console.log(stat) // stat 对象，包含信息属性或方法
  })
  // 同步获取
  const stat = fs.statSync('test.txt')
  ```

- 通过 stat 对象方法判断是文件还是目录
  ```js
  fs.readdir('test/', (err, files) => {
    if (err) throw err
    files.forEach((item) => {
      const stat = fs.stat(`test/${item}`)
      if (stat.isDirectory()) {
        console.log(`test文件夹包含子目录：${item}`)
      } else if (stat.isFile()) {
        console.log(`test文件夹包含子文件：${item}`)
      }
    })
  })
  ```

### 文件流式读写

- 流式读取
  ```js
  // 创建流
  let rs = fs.createReadStream('test.txt')
  // 流数据事件
  rs.on('data', (data) => {
    console.log(`这是一段流数据：${data}`)
  })
  // 流结束事件
  rs.on('end', () => {
    console.log('流式读取完成')
  })
  // 流报错事件
  rs.on('error', (err) => {
    console.log(err)
  })
  ```

- 流式写入
  ```js
  // 创建流
  let ws = fs.createWriteStream('newTest.txt')
  // 流写入
  ws.write('something')
  // 流结束
  ws.end()
  ```

## http 模块

> const http = require('http')

### http 服务构建

