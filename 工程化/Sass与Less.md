CSS 预编译器是扩展 CSS 功能的工具，通过引入变量、嵌套、函数等高级特性，使 CSS 的编写更加灵活和模块化。Sass 和 Less 是两种流行的 CSS 预编译器，广泛应用于前端开发中。

## Sass

Sass（Syntactically Awesome Stylesheets）是一种增强版的 CSS，它引入了变量、嵌套、混合（mixin）、继承等特性。Sass 有两种语法：SCSS 和缩进语法。

### SCSS 语法

SCSS 语法类似于 CSS，并且是 CSS3 的超集。这意味着所有有效的 CSS 代码都是有效的 SCSS 代码。

```scss
$primary-color: #333;

body {
  font: 100% Helvetica, sans-serif;
  color: $primary-color;
}

nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { 
    display: inline-block; 
  }

  a {
    color: $primary-color;
    text-decoration: none;
  }
}
```

### 缩进语法

缩进语法没有大括号和分号，而是依赖缩进来表示嵌套关系。

```sass
$primary-color: #333

body
  font: 100% Helvetica, sans-serif
  color: $primary-color

nav
  ul
    margin: 0
    padding: 0
    list-style: none
  
  li 
    display: inline-block

  a
    color: $primary-color
    text-decoration: none
```

## Sass 的主要特性

1. 变量（Variables）：

允许存储值并在整个样式表中重用。

```scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

2. 嵌套（Nesting）：

允许在 CSS 规则内嵌套子规则，使代码更具结构性和可读性。

```scss
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    color: blue;
    text-decoration: none;
  }
}
```

3. 部分文件（Partials）：

通过 @import 指令可以将多个 Sass 文件组合成一个文件，便于模块化开发。

```scss
// _reset.scss
html, body, ul, ol {
  margin: 0;
  padding: 0;
}

// main.scss
@import 'reset';
body {
  font: 100% Helvetica, sans-serif;
}
```

4. 混合（Mixins）：

允许定义可重用的样式块，并在需要时进行包含。

```scss
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  border-radius: $radius;
}

.box { @include border-radius(10px); }
```

5. 继承（Inheritance）：

允许一个选择器继承另一个选择器的样式。

```scss
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}
```

## Less

Less 是另一个流行的 CSS 预编译器，与 Sass 类似，提供了变量、嵌套、混合、函数等特性。Less 的语法与 CSS 相似，所有合法的 CSS 代码也是合法的 Less 代码。

### Less 语法示例

```less
@primary-color: #4D926F;

#header {
  color: @primary-color;
}

h2 {
  color: @primary-color;
}
```

### Less 的主要特性

1. 变量（Variables）：

允许定义变量并在样式表中重用。

```less
@font-stack: Helvetica, sans-serif;
@primary-color: #333;

body {
  font-family: @font-stack;
  color: @primary-color;
}
```

2. 嵌套（Nesting）：

允许在选择器内嵌套子选择器，类似于 HTML 结构。

```less
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    color: @primary-color;
    text-decoration: none;
  }
}
```

3. 混合（Mixins）：

允许定义可重用的样式块。

```less
.border-radius (@radius: 5px) {
  -webkit-border-radius: @radius;
  -moz-border-radius: @radius;
  border-radius: @radius;
}

.box { .border-radius(10px); }
```

4. 运算（Operations）：

支持在样式中进行数学运算。

```less
@base: 5%;
@width: @base * 2;  // 10%
@height: @base + 100px;  // 105px

.box {
  width: @width;
  height: @height;
}
```

5. 函数（Functions）：

提供内置函数，用于颜色操作、字符串处理等。

```less
@base-color: #111;
@light-color: lighten(@base-color, 20%);
@dark-color: darken(@base-color, 20%);

.light { color: @light-color; }
.dark { color: @dark-color; }
```

## Sass 与 Less 的对比
|特性|	Sass|	Less|
|-|-|-|
|语法|	SCSS（类 CSS）和缩进语法	|类 CSS|
|变量声明|	$variable: value;|	@variable: value;|
|混合（Mixins）|	@mixin name { ... } @include name;|	.name { ... } .name;|
|函数|	@function name { ... }|	使用 mixins 实现|
|条件语句|	@if, @else if, @else|	when|
|循环|	@for, @each, @while|	不直接支持（需使用插件）|
|部分文件|	@import 'file';|	@import 'file';|
|运算支持|	完整支持|	完整支持|
|浏览器支持|	通过 Sass 编译器预处理生成标准 CSS	|通过 Less 编译器预处理生成标准 CSS|
|工具支持|	广泛支持（如 Dart Sass, Node Sass）|	广泛支持（如 Less.js, lessc）|


Sass 和 Less 都是强大的 CSS 预编译器，提供了许多高级特性，使 CSS 编写更加高效和模块化。选择使用哪一个主要取决于团队的习惯和项目的需求。Sass 提供了更强大的功能和更灵活的语法，适合复杂项目；Less 语法简单，与 CSS 更加相似，适合快速上手。无论选择哪一种预编译器，都可以极大提高 CSS 开发的效率和代码的可维护性。
