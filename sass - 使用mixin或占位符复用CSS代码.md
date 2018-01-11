## sass - 使用混合宏(@mixin)和占位符(%placeholder)复用CSS代码



### 使用场景

项目中有几处地方需要自定义滚动条样式，各处的自定义滚动条的样式代码都是一样的，所以希望将其抽离出来，减少项目中重复代码。

### 使用方式

这里只说两种方式：混合宏及占位符。

####混合宏(%mixin)：

定义一个`mixin`，将公共代码放里面：

```css
/*自定义滚动条*/
@mixin scroll-bar {
    /*里面是样式代码*/
}
```

然后在需要用到这段代码的地方使用如下方式引用：

```css
.smiley-panel {
    @include scroll-bar; 
}
```

#### 占位符(%placeholder)：

定义一个占位符scroll-bar

```Css
%scroll-bar {
    /*里面是样式代码*/
}
```

然后在需要用到这段代码的地方使用如下方式引用：

```css
.smiley-panel {
    @extend scroll-bar; 
}
```

### @include和@extend的区别

从功能上看，选择器占位符`%placeholder`和**具有同参数**的`@mixin`是一样的，也就是说在浏览器中渲染出来的效果是没差的。但是其实他们编译出来的CSS却大不相同。

举个栗子:

- 用@mixin和@include实现CSS代码复用：

  ```css
  @mixin icon {
    transition: background-color ease .2s;
    margin: 0 .5em;
  }

  .error-icon {
    @include icon;
    color: red;
  }

  .info-icon {
    @include icon;
    color: blue;
  }
  ```

  编译后的CSS是酱的：

  ```css
  .error-icon {
    transition: background-color ease .2s;
    margin: 0 .5em;
    color: red;
  }

  .info-icon {
    transition: background-color ease .2s;
    margin: 0 .5em;
    color: blue;
  }
  ```

- 用%placeholder和@extend实现CSS代码复用：

  ```css
  %icon {
    transition: background-color ease .2s;
    margin: 0 .5em;
  }

  .error-icon {
    @extend %icon;
    color: red;
  }

  .info-icon {
    @extend %icon;
    color: blue;
  }
  ```

  编译后的CSS是酱的：

  ```css
  .error-icon, .info-icon {
    transition: background-color ease .2s;
    margin: 0 .5em;
  }

  .error-icon {
    color: red;
  }

  .info-icon {
    color: blue;
  }
  ```

有没有看出区别？对呀，编译出来的CSS代码不一样：`@extend`在合并选择器时会非常聪明地合并重复代码，避免不必要的重复；而`@include`并不会合并重复代码，所以编译出来的CSS样式会有重复。

### 带参数的@mixin

上面讲了`@extend`和`@include`的区别后，会觉得`@mixin`略逊一筹，但是`@mixin`可以传参数，这就为她扳回一局啦。借用一下官方文档中的例子(借用的是指定默认值参数的例子)：

```CSS
@mixin sexy-border($color, $width: 1in) {
  border: {
    color: $color;
    width: $width;
    style: dashed;
  }
}
p { @include sexy-border(blue); }
h1 { @include sexy-border(blue, 2in); }
```

这个会编译成：

```css
p {
  border-color: blue;
  border-width: 1in;
  border-style: dashed; }

h1 {
  border-color: blue;
  border-width: 2in;
  border-style: dashed; }
```

PS: [@mixin更多操作](http://sass.bootcss.com/docs/sass-reference/#mixins)

### @extend的限制

`@extend`有个特(mao)点(bing)，Sass无法将`@media`区块之外的CSS规则应用于其内部的选择器，如果在`@media`中使用`@extend`，则只能扩展出现在同一个指令块中的选择器。还是借用一下官方文档的例子吧，哈哈哈：

正确方式：

```CSS
/*这样是OK的*/
@media print {
  .error {
    border: 1px #f00;
    background-color: #fdd;
  }
  .seriousError {
    @extend .error;
    border-width: 3px;
  }
}
```

错误姿势：

```CSS
/*这样会报错*/
.error {
  border: 1px #f00;
  background-color: #fdd;
}
@media print {
  .seriousError {
    // INVALID EXTEND: .error is used outside of the "@media print" directive
    @extend .error;
    border-width: 3px;
  }
}
```

使用错误姿势就会编译报错啦：`Extend directives may only be used in rules`。来，有图有真相：

![extend media](http://ow7p6xhhi.bkt.clouddn.com/extend%20media.png)

PS: [@extend更多操作](http://sass.bootcss.com/docs/sass-reference/#extend)

### 总结

稍作总结一下，其实`@mixin`和`%placeholder`都很强大的，好好使用的话可以让代码非常漂亮简洁，当然这里只是稍微讲了他们的一部分常用使用方法。更多的用法可以参考[官方文档](http://sass.bootcss.com/docs/sass-reference/)啦。平常在考虑使用`@mixin`或`%placeholder`时，可以先评估一下自己的需求再确定使用哪个比较合适。





