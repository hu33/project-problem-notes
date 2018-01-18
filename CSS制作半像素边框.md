## CSS制作半像素边框



### 问题描述

如果想给某个元素设置个0.5px的边框，你会怎么做？是直接设置border-width为0.5？

```Css
border-width: 0.5px;
```

可是这样浏览器真的能给你显示成0.5px的边框吗？哈哈哒，想多啦~

border-width只能为自然数，其他类似的属性也是一样样的，小于1px的线浏览器都直接显示成1px（当然，0px就没有了哈）。[效果](http://jsbin.com/geqefik/edit?html,css,output)

所以，常规方式是不行哒，但是咱可以用伪元素+缩放来巧妙地实现~~

### 实现方式

大概思路就是用伪元素给该元素做个1px的框，这个框的大小是该元素的两倍，然后再缩小0.5倍，这样0.5px像素的框就出来啦~~具体步骤如下：

1. 给目标元素设个定位参考，然后伪元素才能根据这个定位进行绝对定位

   ```CSS
   span {
     position: relative;  /* 只要不是默认值static就好了，原因我想你们应该知道 */
   }
   ```

2. 给目标加个伪元素，并设置绝对定位

   ```css
   span:after {
     position: absolute;
     top: 0;
     left: 0;
   }
   ```

3. 给伪元素加个1px的边框，并把高宽设为目标元素的两倍

   ```css
   border: 1px solid #000;
   width: 200%;
   height: 200%;
   ```

4. 然后缩小0.5倍，缩到目标元素的大小

   ```css
   transform-origin: 0 0;
   transform: scale(0.5);
   ```

然后就差不多啦\~~

完整代码如下：

```css
span {
  position: relative;
}
span:after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  border: 1px solid #000;
  width: 200%;
  height: 200%;
  transform-origin: 0 0;
  transform: scale(0.5);
}
```

具体效果看[这里](http://jsbin.com/geqefik/edit?html,css,output)~~

