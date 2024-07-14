ESLint 和 Prettier 是两种流行的代码风格检查和格式化工具，广泛应用于 JavaScript 项目中。它们帮助开发者保持代码一致性、提高代码质量，并减少代码审查中的人工工作。

## ESLint

ESLint 是一个可扩展的静态分析工具，用于识别和报告 JavaScript 代码中的模式，以确保代码遵循一定的风格和质量标准。ESLint 允许用户自定义规则、插件和配置文件。

### 安装和使用

1. 安装 ESLint：

```sh
npm install eslint --save-dev
```

2. 初始化 ESLint：

```sh
npx eslint --init
```

这将引导你通过一个问答过程来生成一个 ESLint 配置文件（例如 .eslintrc.json）。

3. 配置 ESLint：

ESLint 可以通过配置文件（如 .eslintrc.json）进行自定义。配置文件包括环境、全局变量、规则等。

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": "eslint:recommended",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "rules": {
    "no-unused-vars": "warn",
    "semi": ["error", "always"]
  }
}
```

4. 运行 ESLint：

```sh
npx eslint yourfile.js
```

### 主要功能

1. 规则（Rules）：

ESLint 提供了大量内置规则，用于检查代码的风格、最佳实践、错误等。开发者也可以创建自定义规则。

2. 插件（Plugins）：

ESLint 支持插件系统，可以通过插件扩展功能，如检查 React、Vue 代码，或整合 TypeScript。

```sh
npm install eslint-plugin-react --save-dev
```

```json
{
  "plugins": ["react"]
}
```

3. 共享配置（Shareable Configs）：

共享配置是一个 npm 包，导出一个 ESLint 配置对象，可以通过 extends 属性引入。

```sh
npm install eslint-config-airbnb --save-dev
```

```json
{
  "extends": "airbnb"
}
```

4. 自动修复（Auto-fixing）：

ESLint 可以自动修复一些常见的问题，使用 --fix 选项运行。

```sh
npx eslint yourfile.js --fix
```

## Prettier

Prettier 是一个代码格式化工具，专注于统一代码风格。它没有太多可配置项，旨在通过一致的代码格式减少代码审查中的讨论。

### 安装和使用

1. 安装 Prettier：

```sh
npm install prettier --save-dev
```

2. 创建配置文件：

Prettier 的配置文件通常命名为 .prettierrc，用于指定格式化选项。

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5"
}
```

3. 运行 Prettier：

```sh
npx prettier --write yourfile.js
```

### 主要功能

1. 统一的代码风格：

Prettier 强制统一的代码风格，消除代码审查中的风格争论。

2. 支持多种语言：

除了 JavaScript，Prettier 还支持 TypeScript、CSS、HTML、JSON、Markdown 等多种语言。

3. 与编辑器集成：

Prettier 可以与 VSCode、Sublime Text 等主流编辑器集成，提供即时的代码格式化功能。

4. 自动化集成：

可以与各种构建工具和 CI/CD 集成，确保代码在提交和发布前格式正确。

## ESLint 与 Prettier 的集成

ESLint 和 Prettier 可以一起使用，以确保代码既符合风格指南又保持一致的格式。

1. 安装集成插件

```sh
npm install eslint-config-prettier eslint-plugin-prettier --save-dev
```

配置文件示例

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:prettier/recommended"
  ],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

上述配置中：

- eslint-config-prettier 禁用所有可能与 Prettier 冲突的 ESLint 规则。
- eslint-plugin-prettier 将 Prettier 的格式化规则作为 ESLint 规则运行，并报告格式化问题。


ESLint 和 Prettier 是 JavaScript 开发中保持代码质量和一致性的强大工具。ESLint 通过静态分析来检查代码中的错误和风格问题，并可以通过插件和自定义规则进行扩展。Prettier 则专注于统一代码格式，通过最少的配置选项确保代码风格的一致性。将两者结合使用，可以充分利用它们各自的优势，提高代码质量和开发效率。
