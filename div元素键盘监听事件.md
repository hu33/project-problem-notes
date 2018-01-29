## div元素监听键盘事件

最近做表情联想时，踩了几个键盘监听事件的坑，总结一下~

### 坑一：如何让div元素监听键盘

不知道你们试没试过，直接给`div`绑定如`keypress`、`keyup`、`keydown`这种键盘事件是不管用的，

要想让某个`div`元素监听键盘事件，首先得让它获得焦点吧？不然人家都没有玉玺，哪来的皇权？

```javascript
div.focus();    //div指的是那个想监听键盘事件的div~
```

巴特，你以为`div`元素像`input`元素一样直接`focus`就能聚焦了？too young too naive~~像`form`中`input`、`button`、`textarea`这种可输入的元素和a这种链接元素是默认可获得焦点的，但是`span`、`div`这种元素可不行哇。因为他们缺少一个重要的属性：`tabindex`，给他们添加了`tabindex`属性后，就能指定他们可触焦了。

所以要想让`div`监听键盘事件，应该酱紫写：

```javascript
//需要给这个元素添加tabindex属性
div.setAttribute('tabindex', '0');
div.focus();
```

给它绑定键盘事件，在合适的时候让它获得焦点，然后就能监听到键盘事件啦~~

#### 关于tabindex

`tabindex`属性用于管理键盘焦点，决定元素能否被选中，且指定了按下tab键时元素被选中的顺序。

像`form`表单元素及链接元素是默认可获得焦点的，所以即使不显式地设置其`tabindex`，按下`tab`键时，也能获得焦点，直接触发`onfocus`事件，而`span`这种元素就不行了，哪怕你点击这个`span`也木有用。[试一试~](http://jsbin.com/husuxid/2/edit?html,output)

如果想让`span`也能获取焦点，可以为这个`span`设置个`tabindex`，这样它也具有自动聚焦的能力啦~[试一试~](http://jsbin.com/husuxid/4/edit?html,output)

`tabindex`可以设置为0、负值、正值：

- `tabindex=0`：表示在自然`tab`键顺序中插入一个元素，可以通过调用其`focus`方法聚焦该元素，也能通过tab键聚焦该元素。BTW，`tab`顺序就是你按tab键时元素获得焦点的顺序，默认就是在DOM中的位置顺序。
- `tabindex=负值`：表示在自然`tab`键顺序中移除该元素，哟呵，移除了就不能通过`tab`键使其聚焦啦，不过还是可以调用`focus`方法聚焦的啦。这里的负值基本上就是-1啦，虽然你想设-5，-6，-7也没问题，但是没意义哇，就好像数组的`indexof`，若无匹配就返回-1，也不返回-2对吧？[试一试~](http://jsbin.com/husuxid/5/edit?html,output)
- `tabindex=正值`：显式地指定该元素在`tab`键顺序中的位置。其实`tabindex`就是`tab`+`index`嘛，`index`不就表示索引咯，咱对`index`的理解不就是越小越靠前嘛，`tabindex`也是哇(不包括0)。[试一试~](http://jsbin.com/kayaciz/1/edit?html,css,output)

关于`tabindex`更多使用场景，请移步[这里](https://developers.google.com/web/fundamentals/accessibility/focus/using-tabindex?hl=zh-cn)~

### 坑二：Mac和win的快捷键不一致

在`Mac`中，`cmd+a`默认是全选的快捷键，在`Windows`中，全选的快捷键是`ctrl+a`。我想做的交互是，表情联想面板出现时，不管焦点是否在表情联想面板上，全选的时候都选中输入框中的内容，如下图所示：

![div键盘事件监听2](http://ow7p6xhhi.bkt.clouddn.com/div%E9%94%AE%E7%9B%98%E4%BA%8B%E4%BB%B6%E7%9B%91%E5%90%AC2.gif)

但是实际上，当`div`获取焦点后，`cmd+a`时是酱紫的：

![div键盘事件监听1](http://ow7p6xhhi.bkt.clouddn.com/div%E9%94%AE%E7%9B%98%E4%BA%8B%E4%BB%B6%E7%9B%91%E5%90%AC1.png)

诶？这不是我想要的效果哇，我应该要监听一下`cmd+a`的键盘事件，然后实现我想要的效果~但是`cmd+a`是`Mac`的全选快捷键，而在`Windows`上，对应的快捷键是`ctrl+a`，所以需要先判断一下操作系统，再重写对应的全选键盘事件。

`window`的`navigator`对象中，有个`platform`字段，声明了运行浏览器的操作系统和（或）硬件平台，咱可以用它来判断当前浏览器运行在哪个操作系统中：

```javascript
var isWin = (navigator.platform == "Win32") || (navigator.platform == "Windows");
var isMac = (navigator.platform == "Mac68K") || (navigator.platform == "MacPPC") || (navigator.platform == "Macintosh") || (navigator.platform == "MacIntel");
```

所以核心代码可以这样写：

```javascript
switch (true) {
    case e.keyCode === KEY_A:    //KEY_A = 65
        if ((isMac && e.metaKey) || (isWin && e.ctrlKey)) {
            //your code
        }
        break;
}
```

完整的keycode[在此](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/keyCode)~