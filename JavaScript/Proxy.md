Proxy 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。

## 术语

- handler:  包含捕捉器（trap）的占位符对象，可译为处理器对象。
- traps:  提供属性访问的方法。这类似于操作系统中捕获器的概念。
- target:  被 Proxy 代理虚拟化的对象。它常被作为代理的存储后端。根据目标验证关于对象不可扩展性或不可配置属性的不变量（保持不变的语义）。

## 语法

```const p = new Proxy(target, handler)```

Proxy() 构造函数用于创建 Proxy 对象。

!>  Proxy() 只能通过 new 关键字来调用。如果不使用 new 关键字调用，则会抛出 TypeError。

## 参数

- target: 要使用 Proxy 包装的目标对象，Proxy 会对目标（target）对象进行包装。它可以是任何类型的对象，包括原生的数组、函数甚至是另一个代理对象。
- handler: 一个通常以函数作为属性的对象，各属性中的函数分别定义了在执行各种操作时代理 p 的行为。

## 描述

可以使用 Proxy() 构造函数来创建一个新的 Proxy 对象。构造函数接收两个必须的参数：

- target 是要创建代理的对象
- handler 是定义了代理的自定义行为的对象

一个空的处理器（handler）将会创建一个与被代理对象行为几乎完全相同的代理对象。通过在 handler 对象上定义一组函数，你可以自定义被代理对象的一些特定行为。例如，通过定义 get() 你就可以自定义被代理对象的属性访问器。

## 处理器函数

处理器函数有时候也被称为劫持（trap），这是由于它们会对底层被代理对象的调用进行劫持。

- handler.apply(): 函数调用劫持。
- handler.construct(): new 运算符劫持。
- handler.defineProperty(): Object.defineProperty 调用劫持。
- handler.deleteProperty(): delete 运算符劫持。
- handler.get(): 获取属性值劫持。
- handler.getOwnPropertyDescriptor(): Object.getOwnPropertyDescriptor 调用劫持。
- handler.getPrototypeOf(): Object.getPrototypeOf 调用劫持。
- handler.has(): in 运算符劫持。
- handler.isExtensible(): Object.isExtensible 调用劫持。
- handler.ownKeys(): Object.getOwnPropertyNames 和 Object.getOwnPropertySymbols 调用劫持。
- handler.preventExtensions(): Object.preventExtensions 调用劫持。
- handler.set(): 设置属性值劫持。
- handler.setPrototypeOf(): Object.setPrototypeOf 调用劫持

## 与`Object.defineProperty`比较

### `Object.defineProperty`

#### 功能

- 属性拦截：只能拦截对象的已有属性。
- 只针对单个属性：需要逐个定义对象的每个属性。

#### 用法

- Object.defineProperty 允许你直接在一个对象上定义新的属性或修改现有属性，并返回这个对象。可以设置属性的 getter 和 setter 来拦截对属性的访问和修改。

#### 优势

- 性能开销：在处理简单对象时，性能可能优于 Proxy。
- 浏览器支持：几乎所有现代浏览器和 IE9+ 都支持。
- 对于简单的读取和写入操作，Object.defineProperty 通常性能较好，因为它只在特定属性上拦截操作。

#### 局限性

- 无法拦截属性的添加和删除：只能拦截对象已有属性的访问和修改。
- 数组操作：无法拦截数组的索引操作，如通过 push、pop 等方法修改数组。
- 代码复杂性：对于嵌套对象，需要递归遍历和设置每个属性，代码较为复杂。
- 初始化时需要递归遍历对象的所有属性，性能开销较大，特别是对于深层嵌套的对象。

### Proxy

#### 功能

- 全面拦截：可以拦截几乎所有对对象的操作，包括属性的添加、删除、读取、写入等。
- 单一接口：通过单个 Proxy 对象，可以拦截和处理所有类型的操作，而不需要针对每个属性单独处理。
- 动态处理：Proxy 能够在属性被访问或修改时动态处理这些操作，因此即使是后来添加的属性也能被正确拦截。

#### 用法

- Proxy 对象用于定义基本操作（例如属性查找、赋值、枚举、函数调用等）的自定义行为。创建一个 Proxy 对象需要一个目标对象和一个处理器对象。

#### 优势

- 广泛的拦截能力：可以拦截对象的创建、属性的读取和写入、函数调用等几乎所有操作。
- 对数组友好：可以拦截数组的索引操作和方法调用。
- 简化代码：无需递归遍历对象的所有属性，可以在一个地方集中处理所有拦截逻辑。
- 如果应用涉及大量的动态属性添加和删除，或复杂的嵌套对象结构，Proxy 的性能和代码简洁性更具优势。
- 初始化时无需递归遍历对象的所有属性，性能优于 Object.defineProperty，特别是在处理复杂对象和数组时。
- 可以直接拦截对象属性的添加和删除操作，无需额外的处理逻辑，整体性能更好。
- 尽管 Proxy 在某些简单操作上可能稍逊于 Object.defineProperty，但其在处理复杂场景时的优势，使得它成为现代框架（如 Vue 3）实现响应式系统的首选。

#### 局限性

- 浏览器支持：较新，较老的浏览器（如 IE11）不支持，需要 polyfill。
- 性能开销：在某些情况下可能比 Object.defineProperty 有更高的性能开销。
- 对于读取和写入操作，由于可以拦截所有操作，性能开销会比 Object.defineProperty 高一些，但现代 JavaScript 引擎对 Proxy 已进行了大量优化，通常在可接受范围内。

## 使用场景

常用于对象属性读取、设置劫持等，进行依赖收集或数据校验。或是做一些更底层的操作。

1. 数据校验

通过代理，可以轻松地验证向一个对象的传值是否符合需求，使用到了 set handler 的作用。

```js
let validator = {
  set: function (obj, prop, value) {
    if (prop === "age") {
      if (!Number.isInteger(value)) {
        throw new TypeError("The age is not an integer");
      }
      if (value > 200) {
        throw new RangeError("The age seems invalid");
      }
    }
    // The default behavior to store the value
    obj[prop] = value;
    // 表示成功
    return true;
  },
};
let person = new Proxy({}, validator);
person.age = 100;
console.log(person.age);
// 100
person.age = "young";
// 抛出异常：Uncaught TypeError: The age is not an integer
person.age = 300;
// 抛出异常：Uncaught RangeError: The age seems invalid
```

2. 扩展构造函数

通过代理，可以轻松地通过一个新构造函数来扩展一个已有的构造函数。使用了construct和apply。

```js
function extend(sup, base) {
  // 获取 base.prototype 上的 "constructor" 属性的描述符
  var descriptor = Object.getOwnPropertyDescriptor(
    base.prototype,
    "constructor",
  );

  // 将 base.prototype 设置为继承自 sup.prototype
  base.prototype = Object.create(sup.prototype);

  // 定义一个 handler 对象，用于拦截构造函数和方法调用
  var handler = {
    construct: function (target, args) {
      // 创建一个继承自 base.prototype 的新对象
      var obj = Object.create(base.prototype);
      // 使用 apply 调用 target（即 base）构造函数，传入新对象和参数
      this.apply(target, obj, args);
      // 返回新对象
      return obj;
    },
    apply: function (target, that, args) {
      // 调用 sup（即父类）的构造函数，传入当前对象和参数
      sup.apply(that, args);
      // 调用 base（即子类）的构造函数，传入当前对象和参数
      base.apply(that, args);
    },
  };

  // 创建一个代理对象，用 handler 拦截 base 的调用
  var proxy = new Proxy(base, handler);

  // 将 proxy 赋值给 descriptor.value
  descriptor.value = proxy;
  // 将修改后的 descriptor 定义回 base.prototype 上的 "constructor" 属性
  Object.defineProperty(base.prototype, "constructor", descriptor);

  // 返回代理对象
  return proxy;
}

// 定义 Person 构造函数
var Person = function (name) {
  this.name = name;
};

// 使用 extend 函数使 Boy 继承自 Person
var Boy = extend(Person, function (name, age) {
  this.age = age;
});

// 为 Boy.prototype 添加属性
Boy.prototype.sex = "M";

// 创建 Boy 的实例
var Peter = new Boy("Peter", 13);
console.log(Peter.sex); // "M"
console.log(Peter.name); // "Peter"
console.log(Peter.age); // 13
```

3. 值修正及附加属性

通过代理，以下products代理会计算传值并根据需要转换为数组。这个代理对象同时支持一个叫做 latestBrowser的附加属性，这个属性可以同时作为 getter 和 setter。

```js
let products = new Proxy(
  {
    browsers: ["Internet Explorer", "Netscape"],
  },
  {
    get: function (obj, prop) {
      // 附加一个属性
      if (prop === "latestBrowser") {
        return obj.browsers[obj.browsers.length - 1];
      }
      // 默认行为是返回属性值
      return obj[prop];
    },
    set: function (obj, prop, value) {
      // 附加属性
      if (prop === "latestBrowser") {
        obj.browsers.push(value);
        return;
      }
      // 如果不是数组，则进行转换
      if (typeof value === "string") {
        value = [value];
      }
      // 默认行为是保存属性值
      obj[prop] = value;
      // 表示成功
      return true;
    },
  },
);

console.log(products.browsers); // ['Internet Explorer', 'Netscape']
products.browsers = "Firefox"; // 如果不小心传入了一个字符串
console.log(products.browsers); // ['Firefox'] <- 也没问题，得到的依旧是一个数组

products.latestBrowser = "Chrome";
console.log(products.browsers); // ['Firefox', 'Chrome']
console.log(products.latestBrowser); // 'Chrome'
```


