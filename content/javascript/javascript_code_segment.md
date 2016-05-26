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
