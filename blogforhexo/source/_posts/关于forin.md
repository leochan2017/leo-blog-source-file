---
title: 关于forin
date: 2015-03-26 19:32:07
tags: javascript
---

虽然for in可以用来遍历数组，但是不推荐使用，因为数组要是被添加了其他属性，那么for in就会将所有的元素和属性都遍历到。


<!-- more -->


```
var obj1 = ['four',4,5,6,7]

obj1.name = 'my name is obj2';

for (key in obj1){ console.log(key + ' : ' + obj1[key]); }
```

结果：

```
0 : four

1 : 4

2 : 5

3 : 6

4 : 7

name : my name is obj2
```

看到了吧，他会把obj1上的name属性也打印了

## 遍历数组

```
var arr = ['a',1,'b','c','d','e','f']

for (var i = 0 ;i < arr.length ; i++){ console.log(arr[i]); }
```

## 遍历对象

```
var obj = { 'one' : 1, 'two' : 2, 'three' :3 }

for (key in obj){ console.log(key + ' ：' + obj[key]); }
```