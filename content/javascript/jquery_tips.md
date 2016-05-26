---
title: "jQuery 小技巧"
layout: page
date: 2016-05-25 00:00
---

[TOC]

### 回到顶部按钮
通过使用 jQuery 中的 animate 和 scrollTop 方法，你无需插件便可创建一个简单地回到顶部动画：
```javascript
// Back to top
$('a.top').click(function (e) {
  e.preventDefault();
  $(document.body).animate({scrollTop: 0}, 800);
});
```
将 scrollTop 的值改为你想要 scrollbar 停止的地方。然后你要做的就是，设置在 800 毫秒内回到顶部。

### 预加载图片
如果你的页面使用了大量不能初始可见的图片（例如绑定在 hover 上），预加载它们是十分有用的：

```javascript
$.preloadImages = function () {
  for (var i = 0; i < arguments.length; i++) {
    $('<img>').attr('src', arguments[i]);
  }
};

$.preloadImages('img/hover-on.png', 'img/hover-off.png');
```

### 检查图片是否加载完毕
有时你或许要检查图片是否完全加载完毕，才能在脚本中进行后续操作：
```javascript
$('img').load(function () {
  console.log('image load successful');
});
```
你也可以通过把 img 标签替换成 ID 或 class，来检查特定图片是否加载完成。

### 自动修复损坏的图片
如果你发现自己网站的图片链接挂了，一个一个替换很麻烦。这段简单的代码可以帮上大忙：

```javascript
$('img').on('error', function () {
  $(this).prop('src', 'img/broken.png');
});
```
即使你没有任何损坏的链接，增加这段代码也不会有什么影响。

### Hover 上的 Class 切换
如果用户的鼠标悬停在页面上某个可点击元素时，你想要改变这个元素的视觉表现。可以使用下面这段代码，当用户悬停时，为该元素增加一个 class；当用户鼠标离开后移除这个 class：
```javascript
$('.btn').hover(function () {
    $(this).addClass('hover');
  }, function () {
  $(this).removeClass('hover');
});
```
你仅需增加必须的 CSS。如果需要更简单的方式，还可以使用 toggleClass 方法：
```javascript
$('.btn').hover(function () {
  $(this).toggleClass('hover');
});
```
注意：CSS 或许是这个例子更快速的解决方式，但大家仍然值得知道这一点。

### 禁用 input 字段
有时你也许想让表单的提交按钮或其文本输入框变得不可用，直到用户执行了一个特定行为（例如确认 “我已经阅读该条款” 的复选框）。增加 disabled attribute 到你的 input，就可以实现自己想要的效果：
```javascript
$('input[type="submit"]').prop('disabled', true);
```
当你想把 disabled 的值改为 false 时，仅需在该 input 上再运行一次 prop 方法。
```javascript
$('input[type="submit"]').prop('disabled', false);
```

### 停止链接加载
有时你不想链接跳转到某个页面或重加载该页面，而希望可以做一些其他事情，比如触发其他脚本。下面的代码是禁止默认行为的一个小诀窍：
```javascript
$('a.no-link').click(function (e) {
  e.preventDefault();
});
```

### 淡入淡出/滑动开关
淡入淡出与滑动是我们经常使用 jQuery 做成的动画效果。或许你只是想在用户点击某物时展现一个元素，使用 fadeIn 和 slideDown 都很棒。但如果想让该元素在第一次点击时显现，第二次点击时消失，下面的代码可以很好地完成这个工作：

```javascript
// Fade
$('.btn').click(function () {
  $('.element').fadeToggle('slow');
});

// Toggle
$('.btn').click(function () {
  $('.element').slideToggle('slow');
});

```

### 简单的手风琴效果
```javascript
// Close all panels
$('#accordion').find('.content').hide();

// Accordion
$('#accordion').find('.accordion-header').click(function () {
  var next = $(this).next();
  next.slideToggle('fast');
  $('.content').not(next).slideUp('fast');
  return false;
});
```

### 使两个 Div 高度一样
有时你也许想让两个 div 拥有同样高度，不管它们里面有什么内容：
```javascript
$('.div').css('min-height', $('.main-div').height());
```
该例设置了 min-height，意味着它可以比主要 div 更大，但永远不能更小。但有一个更加灵活的方法是遍历一组元素的设置，然后将高度设为元素中的最高值：
```javascript
var $columns = $('.column');
var height = 0;
$columns.each(function () {
  if ($(this).height() &gt; height) {
    height = $(this).height();
  }
});
$columns.height(height);
```
如果你想让所有列都有相同高度：
```javascript
var $rows = $('.same-height-columns');
$rows.each(function () {
  $(this).find('.column').height($(this).height());
});
```

### 在新标签/窗口打开站外链接
在一个新标签或者新窗口中打开外置链接，并确保站内链接会在相同的标签或窗口中打开：

```javascript
$('a[href^="http"]').attr('target', '_blank');
$('a[href^="//"]').attr('target', '_blank');
$('a[href^="' + window.location.origin + '"]').attr('target', '_self');
```

注意：window.location.origin 在 IE 10 中不可用，该 issue 的修复方法。

### 通过文本找到元素
通过使用 jQuery 中的 contains() 选择器，你可以找到某个元素中的文本。如果文本不存在，该元素将会隐藏：
```javascript
var search = $('#search').val();
$('div:not(:contains("' + search + '"))').hide();
```

### 视觉改变触发
当用户焦点在另外一个标签上，或重新回到标签时，触发 JavaScript：
```javascript
$(document).on('visibilitychange', function (e) {
  if (e.target.visibilityState === "visible") {
    console.log('Tab is now in view!');
  } else if (e.target.visibilityState === "hidden") {
    console.log('Tab is now hidden!');
  }
});
```

### Ajax 调用的错误处理
当某次 Ajax 调用返回 404 或 500 错误，就会执行错误处理。但如果没有定义该处理，其他 jQuery 代码或许会停止工作。可以通过下面这段代码定义一个全局 Ajax 错误处理：

```javascript
$(document).ajaxError(function (e, xhr, settings, error) {
console.log(error);
});
```

### 插件链式调用
jQuery 支持链式调用插件，以减缓反复查询 DOM，并创建多个 jQuery 对象。看下面示例代码：
```javascript
$('#elem').show();
$('#elem').html('bla');
$('#elem').otherStuff();
```

上面这段代码，可以通过链式操作大大改进：
```javascript
$('#elem')
  .show()
  .html('bla')
  .otherStuff();
```
还有另外一种方法，把元素缓存在变量中（前缀是  $ ）：
```javascript
var $elem = $('#elem');
$elem.hide();
$elem.html('bla');
$elem.otherStuff();
```



参考来源: [博客在线-前端开发者都应知道的 jQuery 小技巧](http://web.jobbole.com/84028/)
