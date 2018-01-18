## Chrome最小显示12px字体



### 问题描述

`Chrome`上默认最小字体是`12px`，这就意味着你想设置某个元素(可能是个上标或下标)的`font-size`为`9px`，结果显示出来的是`12px`，有那么一点恼火是吧？

曾经是可以使用`-webkit-text-size-adjust: none`来禁止网页调整字体大小，但是`Chrome27`之后就将该属性禁用了（该属性[具体变更集](https://trac.webkit.org/changeset/145168/webkit)）。这个禁用是有道理的，因为该属性很容易被滥用：很多开发者会直接将该属性设置为全局属性，然后当用户放大或缩小页面时（按住Ctrl滚动鼠标滚轮可缩放网页），文字却维持定义的大小而不放缩，给用户带来的不太友好的体验。

### 解决方案

现在一般是使用`transform: scale(0.82)`来实现小于`12px`的字体显示。比如要显示`8px`的字体，那么可以将该元素加上以下属性：

```css
font-size: 12px;
-webkit-transform: scale(0.667);
-moz-transform: scale(0.667);
-ms-transform: scale(0.667);
-o-transform: scale(0.667);
transform: scale(0.667);
```

只要缩小到需要尺寸的比例即好。需要注意的是，这个方案需要该元素表现为`inline-block`。

具体效果[戳这儿](http://jsbin.com/wiroweh/1/edit?html,css,output)~

上述代码可以使用`sass`的`@mixin`抽象一下（`@mixin`相关内容可以看[这里](https://github.com/hu33/project-problem-notes/blob/master/sass%20-%20%E4%BD%BF%E7%94%A8mixin%E6%88%96%E5%8D%A0%E4%BD%8D%E7%AC%A6%E5%A4%8D%E7%94%A8CSS%E4%BB%A3%E7%A0%81.md)），通过传参数来得到想要的字体。

```css
@mixin webkit-font-size($size: 10) {
    font-size: 12px;
    -webkit-transform: scale($size / 12);
    -moz-transform: scale($size / 12);
    -ms-transform: scale($size / 12);
    -o-transform: scale($size / 12);
    transform: scale($size / 12);
}
```

