# less.js

## 变量

### less:

```
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```

### css:

```
#header {
  width: 10px;
  height: 20px;
}
```



## 混合（Mixins）

### less:

```
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

```
#menu a {
  color: #111;
  .bordered();
}

.post a {
  color: red;
  .bordered();
}
```



## 嵌套

### less:

```
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```

## css:

```
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
```



#### 补充：

重写为一个混合（mixin） (`&` 表示当前选择器的父级）

```
.clearfix {
  display: block;
  zoom: 1;

  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}
```



## @规则嵌套

### less:

```
.component {
  width: 300px;
  @media (min-width: 768px) {
    width: 600px;
    @media  (min-resolution: 192dpi) {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    width: 800px;
  }
}

```

### css:

```
.component {
  width: 300px;
}
@media (min-width: 768px) {
  .component {
    width: 600px;
  }
}
@media (min-width: 768px) and (min-resolution: 192dpi) {
  .component {
    background-image: url(/img/retina2x.png);
  }
}
@media (min-width: 1280px) {
  .component {
    width: 800px;
  }
}
```



## 运算：

### less:

算术运算符 `+`、`-`、`*`、`/` 可以对任何数字、颜色或变量进行运算。如果可能的话，算术运算符在加、减或比较之前会进行单位换算。计算的结果以最左侧操作数的单位类型为准。如果单位换算无效或失去意义，则忽略单位。无效的单位换算例如：px 到 cm 或 rad 到 % 的转换。

```
// 所有操作数被转换成相同的单位
@conversion-1: 5cm + 10mm; // 结果是 6cm
@conversion-2: 2 - 3cm - 5mm; // 结果是 -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // 结果是 4px

// example with variables
@base: 5%;
@filler: @base * 2; // 结果是 10%
@other: @base + @filler; // 结果是 15%
```

```
@base: 2cm * 3mm; // 结果是 6cm
```

```
@color: #224488 / 2; //结果是 #112244
background-color: #112244 + #111; // 结果是 #223355
```

单位类型：都是取最左边的单位

### 补充：

#### calc()特例

```
@var: 50vh/2;
width: calc(50% + (@var - 20px));  // 结果是 calc(50% + (25vh - 20px))
```



## 转义

### less:(old)

```
@min768: ~"(min-width: 768px)";
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
```

### css:

```
@media (min-width: 768px) {
  .element {
    font-size: 1.2rem;
  }
}
```

### less:(new)

```
@min768: (min-width: 768px);
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
```



## 函数

### less:

函数的用法非常简单。下面这个例子将介绍如何利用 percentage 函数将 0.5 转换为 50%，将颜色饱和度增加 5%，以及颜色亮度降低 25% 并且色相值增加 8 等用法：

```
@base: #f04615;
@width: 0.5;

.class {
  width: percentage(@width); // returns `50%`
  color: saturate(@base, 5%);
  background-color: spin(lighten(@base, 25%), 8);
}
```

[less函数]: https://less.bootcss.com/functions/

## 命名空间

```
#bundle() {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white;
    }
  }
  .tab { ... }
  .citation { ... }
}
```

```
#header a {
  color: orange;
  #bundle.button();  // 还可以书写为 #bundle > .button 形式
}
```

注意：如果不希望它们出现在输出的 CSS 中，例如 `#bundle .tab`，请将 `()` 附加到命名空间（例如 `#bundle()`）后面。



## 映射

### less:

```
#colors() {
  primary: blue;
  secondary: green;
}

.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}
```

### css：

```
.button {
  color: blue;
  border: 1px solid green;
}
```



## 作用域

Less 中的作用域与 CSS 中的作用域非常类似。首先在本地查找变量和混合（mixins），如果找不到，则从“父”级作用域继承。

### less:

```
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
```

等价于

```
@var: red;

#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```



## 注释

```
/* 一个块注释
 * style comment! */
@var: red;

// 这一行被注释掉了！
@var: white;
```



## 导入

“导入”的工作方式和你预期的一样。你可以导入一个 `.less` 文件，此文件中的所有变量就可以全部使用了。如果导入的文件是 `.less` 扩展名，则可以将扩展名省略掉

```
@import "library"; // library.less
@import "typo.css";
```