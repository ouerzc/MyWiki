---
title: "javascript 函数节流"
layout: page
date: 2016-05-27 00:00
---


### throttle
函数调用的频度控制器，是连续执行时间间隔控制

《JavaScript高级程序设计》书中代码写法
```javascript
//方法定义
function throttle(method, context) {
    clearTimeout(methor.tId);
    method.tId = setTimeout(function(){
        method.call(context);
    }， 100);
}

//调用
window.onresize = function(){
    throttle(myFunc);
}
```

impress源码中代码写法
```javascript
//方法定义
var throttle = function(fn, delay){
 	var timer = null;
 	return function(){
 		var context = this, args = arguments;
 		clearTimeout(timer);
 		timer = setTimeout(function(){
 			fn.apply(context, args);
 		}, delay);
 	};
 };

 //调用
 window.onresize = throttle(myFunc, 100);
```

[css88博客](http://www.css88.com/archives/4648)中写法
```javascript
/*
* 频率控制 返回函数连续调用时，fn 执行频率限定为每多少时间执行一次
* @param fn {function}  需要调用的函数
* @param delay  {number}    延迟时间，单位毫秒
* @param immediate  {bool} 给 immediate参数传递false 绑定的函数先执行，而不是delay后后执行。
* @return {function}实际调用函数
*/
var throttle = function (fn,delay, immediate, debounce) {
   var curr = +new Date(),//当前事件
       last_call = 0,
       last_exec = 0,
       timer = null,
       diff, //时间差
       context,//上下文
       args,
       exec = function () {
           last_exec = curr;
           fn.apply(context, args);
       };
   return function () {
       curr= +new Date();
       context = this,
       args = arguments,
       diff = curr - (debounce ? last_call : last_exec) - delay;
       clearTimeout(timer);
       if (debounce) {
           if (immediate) {
               timer = setTimeout(exec, delay);
           } else if (diff >= 0) {
               exec();
           }
       } else {
           if (diff >= 0) {
               exec();
           } else if (immediate) {
               timer = setTimeout(exec, -diff);
           }
       }
       last_call = curr;
   }
};

```

### debounce
空闲时间必须大于或等于 一定值的时候，才会执行调用方法

[css88博客](http://www.css88.com/archives/4648)中写法
```javascript
/*
* 空闲控制 返回函数连续调用时，空闲时间必须大于或等于 delay，fn 才会执行
* @param fn {function}  要调用的函数
* @param delay   {number}    空闲时间
* @param immediate  {bool} 给 immediate参数传递false 绑定的函数先执行，而不是delay后后执行。
* @return {function}实际调用函数
*/

var debounce = function (fn, delay, immediate) {
   return throttle(fn, delay, immediate, true);
};
```
[alloyteam博客](http://www.alloyteam.com/2012/11/javascript-throttle/)中写法
```javascript
//方法定义
var throttleV2 = function(fn, delay, mustRunDelay){
 	var timer = null;
 	var t_start;
 	return function(){
 		var context = this, args = arguments, t_curr = +new Date();
 		clearTimeout(timer);
 		if(!t_start){
 			t_start = t_curr;
 		}
 		if(t_curr - t_start >= mustRunDelay){
 			fn.apply(context, args);
 			t_start = t_curr;
 		}
 		else {
 			timer = setTimeout(function(){
 				fn.apply(context, args);
 			}, delay);
 		}
 	};
 };

 //执行
 window.onresize = throttleV2(myFunc, 50, 100);
```
