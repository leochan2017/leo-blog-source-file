---
title: 关于JavaScript命名空间
date: 2015-05-07 19:32:07
tags: javascript
---

javascript本身没有定义名称空间，但是我们可以根据javascript的对象本身的一些特性来模拟出名称空间来


<!-- more -->

```
var leoJsPackage = leoJsPackage || {};

leoJsPackage.util = leoJsPackage.util || {};

leoJsPackage.util.common = {

raiseError : function(message){

throw new Error(message);
}, showMessage : function(info){

alert(info);
} }
```

我们建立了一个leoJsPackage对象，leoJsPackage对象有个util属性，同样util也是一个对象，而common对象则为util的一个属性，当我们想要访问raiseError或者showMessage两个函数时，必须加上leoJsPackage.util.common前缀才可以

如：

```
leoJsPackage.util.common.raiseError('Sorry ,Error')

leoJsPackage.util.common.showMessage('hello world')
```