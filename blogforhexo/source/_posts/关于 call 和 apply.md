---
title: 关于 call 和 apply
date: 2015-03-28 19:32:07
tags: javascript
---

call和apply通常用来修改函数的上下文，函数中的this指针将被替换为call或者apply的第一个参数

<!-- more -->

```
var jack = { name : "jack", age : 26 }

var leo = { name : "leo", age : 24 }
```

### 定义一个全局的函数对象

```
function getName(){ return this.name + '的年龄是：' + this.age; }
```

这时我们设置getName的上下文为jack
此时的this为jack console.log(getName.call(jack)); //结果：jack的年龄是：26

这时我们设置getName的上下文为leo
此时的this为leo console.log(getName.call(leo)); //结果：jack的年龄是：24

另外看看apply

```
console.log(getName.call(jack)); console.log(getName.call(leo));
```



是跟上面一样的结果

### 定义一个animal类

```
function Cat(){

this.name = 'Cat';
}
```

### 创建两个类对象

```
var ani = new Animal();

var cat1 = new Cat();
```

### 通过call或apply方法，将原本属于ani对象的showName方法交给当前对象cat1来使用

```
ani.showName.call(cat1);//输出：Cat

//ani.showName.apply([cat])
```