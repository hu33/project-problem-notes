## inline-block元素间的间距



### 间距问题

当给元素的`display`设置为`inline-block`时，可以让这些元素水平排列，然后这些元素之间会产生间距，效果如下：

![inline-block间隙1](http://ow7p6xhhi.bkt.clouddn.com/inline-block%E9%97%B4%E9%9A%991.png)

其实浏览器会对`inline`元素显示间距，而`inline-block`元素又保持了`inline`元素的特性，那自然也会产生间距咯。

![inline-block间隙2](http://ow7p6xhhi.bkt.clouddn.com/inline-block%E9%97%B4%E9%9A%992.png)

哪怕你设置`margin`为0，间隔还是会有的，那为啥`inline`元素间会有间隔呢？会不会是空白字符引起的呢？可以尝试一下~ [Demo](http://jsbin.com/hiladiv/1/edit?html,css,output)

![inline-block间隙3](http://ow7p6xhhi.bkt.clouddn.com/inline-block%E9%97%B4%E9%9A%993.png)

看起来确实是这个空白字符的问题，字体设的越大，间隙就越大。其实这个是个历史问题，西文排版时`inline`元素都会有个空白字符，专业点叫“空白字符压缩(`white space collapse`)”，仔细想想确实没啥毛病，人家单词中间都要有个空格的哇，要是`inline`之间不留个空格，那岂不是几个元素挨在一起了？但是对于不需留空格的中文来说，这就有点问题了。不过解决方法还是有很多种的~

### 解决方案

要解决`inline`元素或`inline-block`元素间的间隙问题，有很多种方式。

####删除元素间空白字符

其实只要把元素间的间隙删掉就不会有空白字符了。啥意思？我们写HTML代码是这样写的：

```css
<div class="inline-wrapper">
    <span class="inline-span">inline</span>
    <span class="inline-span">inline</span>
    <span class="inline-span">inline</span>
</div>
```

每个`span`和`span`之间都有空白字符，还有个换行，但是浏览器会将所有的空白符和换行符压缩成一个空白字符，也就是那个空格，如果没有把`span`元素紧密连接起来，这些元素间就不会有空白符啦。[Demo](http://jsbin.com/tuzokex/1/edit?html,output)

```css
<div class="inline-wrapper">
    <span class="inline-span">inline</span><span class="inline-span">inline</span><span class="inline-span">inline</span>
</div>
```

这种方式最直接，但对咱程序员很不友好啊，看着这种代码，既不好看又不好维护，一般情况下不可取~

#### 设置父元素font-size为0

我们已经知道间距是由空白字符导致的了，那咱把空白字符的字体大小设为0，间隔不就没有了嘛~~不过要注意的是，这些空白字符和`inline`元素是同级关系，所以应在父元素里设置`font-size`。[Demo](http://jsbin.com/bezedad/1/edit?html,css,output)

```css
.inline-wrapper {
  font-size: 0;
}
.inline-span {
  font-size: 14px;
}
```

这种方式也是阔以哒，但是这有个小问题，父元素设置了`font-size`为0后，所有的`inline`元素里面的字体大小也为0了，所以`inline`子元素都得重新设置`font-size`。

####设置letter-spacing

其实知道了问题出现的原因，解决方案就容易出来了，正如我师父说的，能找到原因的bug都不是问题~~既然间隔是1个空白字符，那咱可以想方设法把字符搞掉咯。CSS有个属性是`letter-spacing`，用来设置字符间距，我们把空白字符的字符间距设为负值，再把`inline`元素的`letter-spacing`设为0就好啦。[Demo](http://jsbin.com/worosed/1/edit?html,css,output)

```css
.inline-wrapper {
  letter-spacing: -10px;
}
.inline-span {
  letter-spacing: 0;
}
```

### 总结

还有一些消除间隔的方案，容我再试验完再更~

`inline-block`的间隔和`inline`的解决方案是一样样的，貌似用的比较多的还是设`font-size`为0了，不过具体的还是看情况吧。