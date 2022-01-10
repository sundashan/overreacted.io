---
title: 代码规范
date: 2021-11-04 18:14:39
spoiler: 省略.
cta: 'js'
---

最近也没啥好写的，凑个数，嘻嘻

### 1. eslint
安装
```jsx
yarn add eslint
```
新建文件 .eslintrc
```jsx
{
  // A wrapper around the Babel parser that makes it compatible with ESLint     default Esprima
  "parser": "@typescript-eslint/parser", // babel-eslint
  "parserOptions": {
    "sourceType": "module",
    "ecmaVersion": 6, //For ES6 syntax
    "ecmaFeatures": {
      "jsx": true,
      "tsx": true,
      "experimentalObjectRestSpread": true,
      "legacyDecorators": true
    }
  },
  "env": {
    "es6": true, //for new ES6 global variables（该选项会自动设置 ecmaVersion 解析器选项为 6）
    "browser": true,
    "commonjs": true
  },
  "globals": {
    "axios": false, //全局变量不能被重写
    "process": false
  },
  "plugins": [
    "@typescript-eslint",
     "react",
     "react-hooks"
  ],
  "extends": [
    "eslint:recommended", // 此属性应用后，rules中的no-XXX规则很多都默认开启了。
    "plugin:@typescript-eslint/recommended"
    // "eslint-config-ali/react"
  ],
  "rules": {
    "no-undef": "error", // 禁用未声明的变量
    "no-unreachable": "warn", // 禁止在 return、throw、continue 和 break 语句后出现不可达代码 (no-unreachable)
    "no-unused-vars": [ // 禁止未使用过的变量
      "warn",
      {
        "vars": "all",
        "args": "after-used",
        "ignoreRestSiblings": true
      }
    ],
    "indent": ["warn", 2], // 强制使用一致的缩进
    // "indent": [1, 2],
    "linebreak-style": [ // 强制使用一致的换行风格
      "warn",
      "unix"
    ],
    "quotes": [ //强制使用一致的反勾号、双引号或单引号
      "warn",
      "single"
    ],
    "semi": [ // 要求或禁止使用分号代替 ASI
      "off",
      "always"
    ],
    "no-empty": [ // 禁止出现空语句块
      "warn",
      {
        "allowEmptyCatch": true
      }
    ],
    "no-alert": "warn",
    "no-console": "off",
    "no-multiple-empty-lines": [ // 禁止出现多行空行
      "warn",
      {
        "max": 1
      }
    ], //最多1个空行
    "@typescript-eslint/explicit-module-boundary-types": "off", // ????
    "no-use-before-define": "off", // 禁止在变量定义之前使用它们
   "@typescript-eslint/no-use-before-define": ["error"],
    "react-hooks/rules-of-hooks": "off",
    // React & JSX
    "react/prop-types": "off", // 防止在 React 组件定义中缺少 props 验证
    // Our transforms set this automatically dom属性检测
    // "react/jsx-boolean-value": [2, "always"],
    "react/jsx-no-undef": "error", // Disallow undeclared variables in JSX
    // We don"t care to do this
    // "react/jsx-sort-prop-types": OFF,
    // "react/jsx-space-before-closing": 2,
    "react/jsx-tag-spacing": "warn", // 验证 JSX 左括号和右括号内和周围的空格
    // 排除检测React 因为代码可能没有引用他 正常检查会有语法错误
    "react/jsx-uses-react": "error", // Prevent React to be marked as unused
    // "react/jsx-uses-axios": 2,
    // "react/no-is-mounted": OFF,
    // This isn"t useful in our test code
    "react/react-in-jsx-scope": "error", // Prevent missing React when using JSX
    // jsx 空标签  标签没有关闭
    "react/self-closing-comp": "error", // Prevent extra closing tags for components without children
    // We don"t care to do this
    "react/jsx-wrap-multilines": [ // Prevent missing parentheses around multilines JSX
      "error",
      {
        "declaration": false,
        "assignment": false
      }
    ],
    // 判断虚拟dom 有没有使用
    "react/jsx-uses-vars": [ // Prevent variables used in JSX to be marked as unused
      "error"
    ],
    "react/forbid-prop-types": "off", // Forbid certain propTypes
  }
}
```

### 2. prettier
代码格式化工具
安装
```jsx
yarn add --dev --exact prettier
```
新建文件 .prettierrc.js
```jsx
module.exports = {
  bracketSpacing: true,
  singleQuote: true,
  jsxBracketSameLine: false,
  printWidth: 80,
  parser: 'babel',
  overrides: [
    {
      files: '.prettierrc',
      options: {
        trailingComma: 'es5'
      }
    }
  ]
};
```
prettier也可与lint-staged挂钩，在package.json中进行设置，例如：
```jsx
{
  "lint-staged": {
    "**/*": "prettier --write --ignore-unknown"
  }
}
```

注意：
vscode--setting.json文件中
"editor.defaultFormatter": "esbenp.prettier-vscode"
设置里面设置此属性，Ctrl+s保存会自动格式文件，不够eslint和prettier的规则要设置好，不然冲突了
### 3. lint-staged
对暂存的git文件运行linters的工具。
安装
yarn add lint-staged
配置
在package.json中进行设置，例如：
```jsx
"lint-staged": {
    "src/**/*.ts?(x)": [
        "tslint --fix",
        "git add"
    ],
    "src/**/*.js?(x)": [
        "eslint --fix",
        "git add"
    ]
}
```

### 4. tslint
tslint 已废弃，从TSLint迁移到eslint，现使用typescript-eslint

### 5. 其他
vscode默认配置
项目里面配置.vscode，里面包括extensions.json，settings.json
```jsx
{
    "editor.formatOnSave": true, // 自动保存格式化代码
    "javascript.format.enable": true, // 格式化js时冲突，需要关闭默认hs格式化程序
    "stylelint.enable": true,
    "editor.codeActionsOnSave": { // vscode保存自动按照eslint规则格式化代码
        "source.fixAll.eslint": true,
        "source.fixAll.stylelint": true,
    },
    "editor.tabSize": 2, // tab键2个空格
    "prettier.tabWidth": 2, // prettier tab键2个空格
    "js/ts.implicitProjectConfig.experimentalDecorators": true, // 装饰器的语法是ES7的实验性语法，在VSCode中需要打开experimentalDecorators的设置方可消除报错提示。
    "eslint.validate": [ // 这个参数是老版本的定义校验的类型，逐步会被替代,添加XX支持,看项目是从什么写的，如果是vue，那么["javascript", "vue", "html"],
      "typescript",
      "typescriptreact",
    ],
  }
  
```

### 6. 参考链接：
https://github.com/typescript-eslint/typescript-eslint
https://github.com/typescript-eslint/typescript-eslint/tree/master/packages/eslint-plugin/docs/rules