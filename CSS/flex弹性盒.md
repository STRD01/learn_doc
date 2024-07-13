CSS 的弹性盒（Flexbox）布局模块是一种一维布局模型，用于在容器中分配和对齐空间。Flexbox 布局模块旨在提供更高效和灵活的方式来排列子元素，特别是在动态或不确定的尺寸环境下。下面是对 Flexbox 的详细介绍。

## 基本概念

- Flex 容器（Flex Container）

  - Flex 容器是使用 `display: flex` 或 `display: inline-flex` 属性定义的容器。所有的直接子元素自动成为 Flex 项目。

- Flex 项目（Flex Item）

  - Flex 项目是 Flex 容器的直接子元素。它们的排列和布局由容器的 Flexbox 规则控制。

### Flex 容器属性

1. display

定义 Flex 容器。

```css
.container {
  display: flex; /* 或者 inline-flex */
}
```

2. flex-direction

定义主轴方向（主轴是 Flex 容器的主排列方向）。

- row（默认）：从左到右。
- row-reverse：从右到左。
- column：从上到下。
- column-reverse：从下到上。

```css
.container {
  flex-direction: row; /* row, row-reverse, column, column-reverse */
}
```

3. flex-wrap

控制 Flex 项目是否换行。

- nowrap（默认）：不换行。
- wrap：换行，第一行在上方。
- wrap-reverse：换行，第一行在下方。

```css
.container {
  flex-wrap: nowrap; /* nowrap, wrap, wrap-reverse */
}
```

4. flex-flow

flex-direction 和 flex-wrap 的简写形式。

```css
.container {
  flex-flow: row wrap; /* flex-direction | flex-wrap */
}
```

5. justify-content

定义主轴上的对齐方式。

- flex-start（默认）：项目从主轴起点对齐。
- flex-end：项目从主轴终点对齐。
- center：项目在主轴居中对齐。
- space-between：项目之间的间距相等，首尾项目与容器边缘对齐。
- space-around：项目之间的间距相等，每个项目两侧的间距相等。
- space-evenly：项目之间的间距相等，包括首尾项目。

```css
.container {
  justify-content: flex-start; /* flex-start, flex-end, center, space-between, space-around, space-evenly */
}

```

6. align-items

定义交叉轴上的对齐方式。

- stretch（默认）：项目在交叉轴上拉伸以适应容器。
- flex-start：项目在交叉轴起点对齐。
- flex-end：项目在交叉轴终点对齐。
- center：项目在交叉轴居中对齐。
- baseline：项目的第一行文字基线对齐。

```css
.container {
  align-items: stretch; /* stretch, flex-start, flex-end, center, baseline */
}

```

7. align-content

定义多行内容的对齐方式（当项目不止一行时）。

- stretch（默认）：行在交叉轴上拉伸以适应容器。
- flex-start：行在交叉轴起点对齐。
- flex-end：行在交叉轴终点对齐。
- center：行在交叉轴居中对齐。
- space-between：行之间的间距相等，首尾行与容器边缘对齐。
- space-around：行之间的间距相等，每个行两侧的间距相等。
- space-evenly：行之间的间距相等，包括首尾行。

```css
.container {
  align-content: stretch; /* stretch, flex-start, flex-end, center, space-between, space-around, space-evenly */
}
```

### Flex 项目属性

1. order

定义项目的排列顺序。数值越小，排列越靠前，默认值为 0。

```css
.item {
  order: 1; /* 任意整数 */
}
```

2. flex-grow

定义项目在 flex 容器中分配剩余空间的相对比例，负值无效，默认为 0（即如果存在剩余空间，也不放大）。

剩余空间是 flex 容器的大小减去所有 flex 项的大小加起来的大小。如果所有的兄弟项目都有相同的 flex-grow 系数，那么所有的项目将剩余空间按相同比例分配，否则将根据不同的 flex-grow 定义的比例进行分配。

```css
.item {
  flex-grow: 1; /* 任意非负数 */
}
```

3. flex-shrink

指定了 flex 元素的收缩规则。flex 元素仅在默认宽度之和大于容器的时候才会发生收缩，其收缩的大小是依据 flex-shrink 的值。默认为 1（即如果空间不足，项目会缩小）。

```css
.item {
  flex-shrink: 1; /* 任意非负数 */
}
```

4. flex-basis

指定了 flex 元素在主轴方向上的初始大小。如果不使用 box-sizing 改变盒模型的话，那么这个属性就决定了 flex 元素的内容盒（content-box）的尺寸。可以为任意长度单位。

```css
.item {
  flex-basis: auto; /* auto, 任意长度单位 */
}
```

5. flex

flex-grow、flex-shrink 和 flex-basis 的简写。

```css
.item {
  flex: 1 1 auto; /* flex-grow | flex-shrink | flex-basis */
}
```

6. align-self

允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性。默认值为 auto，继承父元素的 align-items 属性。

```css
.item {
  align-self: auto; /* auto, flex-start, flex-end, center, baseline, stretch */
}
```

以下是一个 Flexbox 布局的完整示例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    .container {
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      align-content: flex-start;
      height: 300px;
      border: 1px solid black;
    }
    .item {
      flex: 1 1 100px;
      margin: 10px;
      background-color: lightblue;
      text-align: center;
    }
  </style>
  <title>Flexbox Example</title>
</head>
<body>
  <div class="container">
    <div class="item">Item 1</div>
    <div class="item">Item 2</div>
    <div class="item">Item 3</div>
    <div class="item">Item 4</div>
    <div class="item">Item 5</div>
  </div>
</body>
</html>
```

在这个示例中，.container 是一个 Flex 容器，.item 是 Flex 项目。通过设置不同的 Flexbox 属性，可以控制项目在容器中的排列、对齐和分配空间的方式。Flexbox 布局非常灵活且强大，适用于创建各种复杂的布局和响应式设计。