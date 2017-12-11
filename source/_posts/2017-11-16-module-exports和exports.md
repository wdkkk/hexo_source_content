---
title: module.exports和exports
date: 2017-11-16 20:24:30
tags:
---

```javascript
var name = 'rainbow';
exports.name = name;
exports.sayName = function(){
  console.log(name);
}
// 给 exports 赋值相当于给 module.exports 这个空对象添加了两个属性，相当于：
var name = 'rainbow';
module.exports.name = name;
module.exports.sayName = function(){
  console.log(name);
}
```
<!--more-->
在使用require时，得到的始终是module.exports（也可以说导出的始终是module.exports），而module.exports=exports   exports.xxx相当于给exports挂属性，也就是对module.exports挂属性，也就是对require得到的模块挂属性，而使用module.exports =xxx 就是直接让这个模块等于xxx