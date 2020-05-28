---
layout: post
title: '代码规范'
date: 2020-05-09
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: engineering
---

> `eslint + prettier`

&emsp;&emsp;统一代码规范的好处有如下几条：
* 避免初级错误
* 统一团队风格
* 养成良好的编码习惯

### `eslint`
&emsp;&emsp;由于`javascript`是动态的弱类型语言，没有编译过程，于是一些错误可能无法发现。`eslint`能在开发的过程中给出提示。给与代码质量和代码格式的提示
#### 使用
1. 安装
  ```text
  $ npm install -D eslint

  ```
2. 初始化配置文件（支持`.eslintrc.js`、`.eslintrc.yaml`、`.eslintrc.json`）
  ```text
  $ ./node_modules/.bin/eslint --init
  ```
3. 配置（`.eslintrc.json`为例）
  ```json
  {
    "rules": {
      "semi": ["error", "always"],
      "quotes": ["error", "double"]
    }
  }
  ```

### `prettier`
&emsp;&emsp;规范代码格式，自动格式化代码
#### 使用
1. 安装
  ```text
  $ npm i -D prettier

  ```
2. 配置文件
  ```js
    module.exports = {
      singleQuote: true,
      trailingComma: 'none',
      jsxSingleQuote: true,
      tabWidth: 2,
      jsxBracketSameLine: false,
      printWidth: 120,
      bracketSpacing: true,
      arrowParens: 'always',
      compileOnSave: false,
      semi: true,
      tslintIntegration: true
    }

  ```
3. 配合`eslint`（在不符合`prettier`的格式给出提示）
   ```text
    $ npm install --D eslint-plugin-prettier
   ```
   ```json
    {
       "extends": ["plugin:@typescript-eslint/recommended", "plugin:prettier/recommended"],
      // ...
    }
    // 等同于
    {
      "plugins": ["prettier"],
      "rules": {
        "prettier/prettier": ["error", {
          "endOfLine":"auto"  // 结尾是 \n \r \n\r auto，有关Delete 'CR'的问题
        }]
      }
    }
   ```
### 结合`vscode`使用
1. 下载`prettier`插件
2. 配置`setting.json`
   ```json
    {
    // 自动修复
    "editor.codeActionsOnSave": {
        "source.fixAll.tslint": true,
        "source.fixAll.eslint": true
    },
    // 验证文件类型
    "eslint.validate": [  
        "vue",
        "html",
        "javascript",
        "typescript",
        "javascriptreact",
        "typescriptreact"
    ],
    }
   ```
### 参考
* <a style='color:#0A497B' href='https://juejin.im/post/5b27a326e51d45588a7dac57' target='_blank'>eslint +prettier</a>
* <a style='color:#0A497B' href='https://www.jianshu.com/p/dd07cca0a48e' target='_blank'>ESLint+Prettier代码规范实践</a>
* <a style='color:#0A497B' href='https://copyfuture.com/blogs-details/20200221154453353i9w32fbrpj93jb4' target='_blank'>Delete `␍`eslint(prettier/prettier) 错误的解决方案</a>
