---
title: work note
date: 2016-09-14 10:34:46
tags: note
---

　　首行缩进
    
##### 1、arguments.callee
 &#160; &#160; &#160; &#160;才进公司时被安排看了几个月的代码，第一次看到这货的时候我竟然是懵逼的(不得不说当时图样，不懂的太多，当然现在才发觉不懂的更多)，当时只知道arguments（它是指向实参对象的引用，实参对象是一个类数组对象，这么官方的话当然是百度的，反正就是能获取函数传入的参数）。不懂于是马上百度之(我遇到问题之所以首选百度，是因为来得快，使用google要翻墙，即使墙后依然可能会很慢，当然也有百度解决不了问题的时候，而且情况还不少，想以后专门写一篇个人解决问题调试bug的文章)，百度出来的结果一目了然：返回当前正在执行的function对象。在函数内部有两个特殊的对象：arguments和this，其中arguments是一个数组类对象，包含传入函数的所有参数；而this则表示的是函数在执行时所处的作用域。callee是arguments对象的一个属性，指向的就是拥有这个arguments的函数，这样一看就很容易理解使用它的代码了。

常使用场景：在定时器内调用自身或函数内部递归调用自身
注意事项：严格模式下（"user strict"）不能使用

<!--more-->

##### 2、css处理文本超过div宽度自动换行
```
word-wrap: break-word（会对长单词另起一行）
word-break: break-all（强制断句不换行）
```
##### 3、阻止事件捕获/冒泡
```
$("#div").on("touchmove",function(event){
    var e=window.event||event;
if(e.stopPropagation){
    e.stopPropagation();
}else{
    window.event.cancelBubble=true;
	}
})
```
之前遇到一个问题，手机页面里有一个div，div的超出内容是scroll的，当滚动div的内容不想页面跟随滚动，后面仔细一想其实就是阻止事件捕获，以上是兼容写法。

##### 4、节流两种方式简单实现
```
var  debounce=function (action, idle) {//在idle时间内只执行action一次
    var i = null;
    return function () {
        clearTimeout(i);
        i = setTimeout(function () {
            action.apply(this, arguments);
        }, idle)
    }
};
var  throttle= function(action, delay){//每delay时间执行action一次
    var last = 0;
    return function(){
        var curr = +new Date();
        if (curr - last > delay){
            action.apply(this, arguments);
            last = curr;
        }
    }
};
```

##### 5、网站整体变成灰白色
```
<html style="filter: progid:DXImageTransform.Microsoft.BasicImage(grayscale=1);-webkitfiltegrayscale(100%);"/>
```
这段代码的使用场景我想不言而喻。

##### 6、网页禁止右键：
```
oncontextmenu="return false;"
document.oncontextmenu=function(a){return !1}
```
插播一条，jQuery事件显示按了哪个键：event.which

##### 7、DOM文档加载顺序
```
 * 解析HTML结构。
 * 加载外部脚本和样式表文件。
 * 解析并执行脚本代码。
 * 构造HTML DOM模型。//ready
 * 加载图片等外部文件。
 * 页面加载完毕。//load
```
##### 8、BFC（清除浮动）
如果要把BFC(块级格式化上下文)讲清楚，恐怕得单独写一章，我还不一定说得清楚，而且网上好的文章有很多，此处只记录其中的一种作用-清除浮动，也是我们项目中所用的比较兼容的写法。
```
.clearfix:before,
.clearfix:after { /*直接copy的项目中的源码，好像只写after就行了吧*/
    content: " ";
    display: table;
}
.clearfix:after {
    clear: both;
}
.clearfix {
    *zoom: 1;
}
```
##### 9、ie各个版本hash

![ie 各个版本hash](/images/ie_hash.png)

##### 总结