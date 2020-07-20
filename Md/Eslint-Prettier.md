<h1 align="center">Eslint-Prettier</h1>

> 本文参考：[深入理解 ESLint](https://zhuanlan.zhihu.com/p/75531199)

## 代码统一

- 代码质量规则(code-quality rules)
  - no-unused-vars
  - no-extra-bind
  - ...
- 代码风格规则(code-formatting rules)
  - max-len
  - keyword-spacing
  - ...

## Eslint

作用

- 代码质量问题（侧重）
- 代码风格问题

安装

- npm 全局安装并初始化

  ```powershell
  # 全局安装
  npm install -g eslint

  # 在安装目录文件夹下初始化package.json
  npm init -f

  # 初始化ESlint
  eslint --init
  ```

配置

- 在指定文件中使用注释配置

  ```js
  // 配置规则
  /* eslint eqeqeq: "error" */
  var num = 1
  num == '1'

  // 忽视规则
  /* eslint-disable */
  console.log('该注释放在文件顶部，整个文件都不会出现 lint 警告')

  /* eslint-enable */
  console.log('重新启用 lint 警告')

  /* eslint-disable eqeqeq */
  console.log('只禁止某一个或多个规则')

  /* eslint-disable-next-line */
  console.log('当前行禁止 lint 警告')
  ```

- 使用配置文件

  - JavaScript (eslintrc.js)
  - YAML (eslintrc.yaml)
  - JSON (eslintrc.json)
  - package.json -> eslintConfig

  ```js
  {
    // 解析器类型
    "parse": "esprima", // espima(默认), babel-eslint, @typescript-eslint/parse

    // 解析器配置参数
    "parseOptions": {
      "sourceType": "script", // 代码类型：script(默认), module
      "ecamVersion": 6, // es 版本号，默认为5，也可以是用年份，比如2015 (同6)
      // es 特性配置
      "ecmaFeatures": {
        "globalReturn": true, // 允许在全局作用域下使用 return 语句
        "impliedStrict": true, // 启用全局 strict mode
        "jsx": true // 启用 JSX
      }
    },

    // 声明全局变量
    "globals": {
      // 声明 jQuery 对象为全局变量
      "$": false // true表示该变量为 writeable，而 false 表示 readonly
    },

    // 对环境定义全局变量预设
    "env": {
      "amd": true,
      "commonjs": true,
      "jquery": true,
      "browser": true,
      "es6": true,
      "node": true
    },

    // 规则配置
    "rules": {
      // 数组形式配置规则细节
      "quote": [
        "error", // off 或 0 为关闭规则，warn 或 1 为警告，error 或 2 为报错
        "single",
        {
          "avoidEscape": true
        }
      ]
    },

    // 扩展规则
    "extends": [
      "eslint:recommended", // 官方扩展，"eslint:recommended"/"eslint:all"
      "plugin:react/recommended", // plugin为插件类扩展，需先npm安装
      "plugin:vue/essential",
      "eslint-config-standard" // npm官方包，可省略eslint-config- 开头
    ],

    // 插件
    // 先安装，npm install --save-dev eslint-plugin-vue eslint-plugin-react
    "plugins": [
      "eslint-plugin-react",
      "vue" // 可省略 eslint-plugin- 开头
    ]
  }
  ```

## prettier

作用

- 代码风格问题

安装

- prettier 安装

  `npm install -D prettier`

- eslint 插件安装

  `npm install -D eslint-plugin-prettier`

配合 ESLint

- 在.eslintrc.js 文件中配置错误提示

  ```js
  {
    "plugins": ["prettier"],
    "rules": {
      "prettier/prettier": "error"
    }
  }
  ```

- 修改 webpack 配置调用 ESLint 的 autofix 功能，保存自动修复错误提示的地方

  ```js
  const path = require('path')
  module.exports = {
    module: {
      rules: [
        {
          test: /\.(js|vue)$/,
          loader: 'eslint-loader',
          enforce: 'pre',
          include: [path.join(__dirname, 'src')],
          options: {
            fix: true,
          },
        },
      ],
    },
  }
  ```

- 解决和 ESLint 插件的冲突，安装依赖包并在.eslintrc.js 文件配置

  `npm i -D eslint-config-prettier`

  ```js
  {
    extends: [
      'standard',
      "prettier",
    ],
  }

  {
    "extends": ["plugin:prettier/recommended"] // 简化上面的写法
  }
  ```
