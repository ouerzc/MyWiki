---
title: "javascript 代码片段"
layout: page
date: 2016-05-25 00:00
---

[TOC]

### 调用函数或类,函数内部自动new ###
```javascript
var Person = function(name){
  if(!(this instanceof Person)){
    return new Person(name);
  }
  this.name = name;
}

var p = new Person('laoyao');
alert(p.name);

var p = Person('xxx');
alert(p.name);
```

### 用 JavaScript 模拟触发内置事件
内置的事件也可以被 JavaScript 模拟触发，比如下面函数模拟触发单击事件：

```javascript
function simulateClick() {
  var event = new MouseEvent('click', {
    'view': window,
    'bubbles': true,
    'cancelable': true
  });
  var cb = document.getElementById('checkbox');
  var canceled = !cb.dispatchEvent(event);
  if (canceled) {
    // A handler called preventDefault.
    alert("canceled");
  } else {
    // None of the handlers called preventDefault.
    alert("not canceled");
  }
}
```
参考链接: [创建和触发 events](https://developer.mozilla.org/zh-CN/docs/Web/Guide/Events/Creating_and_triggering_events)
