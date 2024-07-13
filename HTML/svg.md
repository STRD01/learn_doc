SVG（Scalable Vector Graphics，可缩放矢量图形）是一种基于XML的图形格式，用于描述二维矢量图形。SVG 是 W3C 推荐的标准，广泛用于网页开发中，以其可伸缩性和分辨率独立性著称。

## SVG 的特点

- 可缩放性：SVG 图形是矢量图形，可以在任何分辨率下缩放到任意大小而不失真。
- 基于 XML：SVG 文件是文本文件，可以轻松编辑和压缩。
- 交互性：SVG 是一种基于 XML 的标记语言，每个图形元素都是 DOM 元素，可以使用标准的 DOM 事件和 CSS 样式进行动态操作和交互，容易添加交互效果。
- 可访问性：SVG 文件可以包含可访问性信息，如标题和描述。
- 静态或动态图形：适用于静态图形和复杂的交互图形，如图表和图标。
- 性能：由于 SVG 基于 DOM，每个元素都可以独立操作，适合于图形元素较少但需要频繁交互的场景。
- 文件大小：对于简单图形，SVG 文件通常比 Canvas 更小，因为它是基于矢量的描述语言。
- 加载时间：由于基于 DOM 的特性，可能会导致较长的初始解析和渲染时间，尤其是包含大量图形元素时。
- 适用场景：
  - 适合需要高质量渲染和互动的场景，如图表、图标、信息图和地图。
  - 适合绘制复杂的几何图形且需要在不同分辨率和设备上保持清晰。
  - 图形元素数量较少，且需要频繁的 DOM 操作和交互。
  - 需要对图形进行样式化（使用 CSS）。

## 基本用法

### 内联 SVG

内联 SVG 是将 SVG 代码直接嵌入到 HTML 文档中。内联 SVG 可以直接与 CSS 和 JavaScript 交互。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Inline SVG Example</title>
  <style>
    .red-fill {
      fill: red;
    }
  </style>
</head>
<body>
  <svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
    <circle cx="100" cy="100" r="50" class="red-fill" />
  </svg>
</body>
</html>
```

### 外部 SVG

外部 SVG 是将 SVG 文件存储在独立的文件中，并在 HTML 中引用。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>External SVG Example</title>
</head>
<body>
  <img src="image.svg" alt="Example SVG" width="200" height="200">
</body>
</html>
```

### 嵌入 SVG

使用 <object>、<iframe> 或 <embed> 标签嵌入 SVG 文件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Embed SVG Example</title>
</head>
<body>
  <object data="image.svg" type="image/svg+xml" width="200" height="200"></object>
</body>
</html>
```

## 基本图形元素

1. <rect>

矩形。

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <rect x="10" y="10" width="100" height="50" fill="blue" />
</svg>
```

2. <circle>

圆形。

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <circle cx="100" cy="100" r="50" fill="green" />
</svg>
```

3. <ellipse>

椭圆。

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <ellipse cx="100" cy="100" rx="50" ry="25" fill="yellow" />
</svg>
```

4. <line>

直线。

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <line x1="10" y1="10" x2="190" y2="190" stroke="black" stroke-width="2" />
</svg>
```

5. <polyline>

折线。

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <polyline points="10,10 50,60 90,10 130,60" fill="none" stroke="red" />
</svg>
```

6. <polygon>

多边形。

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <polygon points="50,10 90,60 10,60" fill="purple" />
</svg>
```

7. <path>

复杂路径。

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <path d="M10 10 H 90 V 90 H 10 L 10 10" fill="none" stroke="blue" />
</svg>
```

## 文本

可以使用 <text> 元素在 SVG 中添加文本。

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <text x="10" y="30" font-family="Arial" font-size="24" fill="black">Hello, SVG!</text>
</svg>
```

## 样式

### 使用 CSS

SVG 元素可以使用 CSS 进行样式化。

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <style>
    .my-rect {
      fill: orange;
      stroke: black;
      stroke-width: 2;
    }
  </style>
  <rect x="10" y="10" width="100" height="50" class="my-rect" />
</svg>
```

### 使用内联样式

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <rect x="10" y="10" width="100" height="50" style="fill:orange; stroke:black; stroke-width:2;" />
</svg>
```

## 动画

SVG 支持动画，可以使用 <animate>、<animateTransform> 等元素。

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <circle cx="50" cy="50" r="40" fill="green">
    <animate attributeName="cx" from="50" to="150" dur="2s" repeatCount="indefinite" />
  </circle>
</svg>
```

## 交互性

可以使用 JavaScript 操作 SVG 元素。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>SVG Interaction Example</title>
</head>
<body>
  <svg width="200" height="200" xmlns="http://www.w3.org/2000/svg" onclick="changeColor(evt)">
    <circle id="myCircle" cx="100" cy="100" r="50" fill="blue" />
  </svg>
  <script>
    function changeColor(evt) {
      const circle = document.getElementById('myCircle');
      circle.setAttribute('fill', 'red');
    }
  </script>
</body>
</html>
```

## 完整示例

以下是一个包含多种 SVG 元素和功能的完整示例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>SVG Example</title>
  <style>
    .highlight {
      fill: yellow;
      stroke: black;
      stroke-width: 2;
    }
  </style>
</head>
<body>
  <svg width="600" height="600" xmlns="http://www.w3.org/2000/svg">
    <!-- 矩形 -->
    <rect x="10" y="10" width="100" height="50" fill="blue" />

    <!-- 圆形 -->
    <circle cx="200" cy="100" r="50" fill="green" />

    <!-- 椭圆 -->
    <ellipse cx="400" cy="100" rx="50" ry="25" fill="purple" />

    <!-- 直线 -->
    <line x1="10" y1="150" x2="300" y2="150" stroke="black" stroke-width="2" />

    <!-- 折线 -->
    <polyline points="10,200 50,250 90,200 130,250" fill="none" stroke="red" />

    <!-- 多边形 -->
    <polygon points="250,200 290,250 210,250" class="highlight" />

    <!-- 路径 -->
    <path d="M10 300 H 90 V 350 H 10 L 10 300" fill="none" stroke="blue" />

    <!-- 文本 -->
    <text x="10" y="400" font-family="Arial" font-size="24" fill="black">Hello, SVG!</text>

    <!-- 动画 -->
    <circle cx="50" cy="450" r="40" fill="green">
      <animate attributeName="cx" from="50" to="150" dur="2s" repeatCount="indefinite" />
    </circle>

    <!-- 交互性 -->
    <circle id="interactiveCircle" cx="300" cy="450" r="50" fill="blue" onclick="changeColor(evt)" />
  </svg>

  <script>
    function changeColor(evt) {
      const circle = document.getElementById('interactiveCircle');
      circle.setAttribute('fill', 'red');
    }
  </script>
</body>
</html>
```

SVG 提供了强大的图形绘制和交互功能，是网页开发中处理图形和动画的利器。
