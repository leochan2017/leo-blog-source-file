---
title: 关于localStorage和sessionStorage
date: 2015-05-12 22:32:07
tags: javascript
---

...


<!-- more -->

1.localStorage和sessionStorage均只能存储字符串类型的对象

2.localStorage生命周期是永久

3.sessionStorage生命周期为当前窗口或标签页，关闭了，数据就没了

4.不同浏览器无法共享localStorage或sessionStorage中的信息。

5.相同浏览器的不同页面间可以共享相同的localStorage（页面属于相同域名和端口），但是不同页面或标签页间无法共享sessionStorage的信息。这里需要注意的是，页面及标签页仅指顶级窗口，如果一个标签页包含多个iframe标签且他们属于同源页面，那么他们之间是可以共享sessionStorage的。 同源的判断规则： [http://www.test.com](http://www.test.com/) [https://www.test.com](https://www.test.com/) （不同源，因为协议不同） [http://my.test.com（不同源，因为主机名不同）](http://my.test.xn--com(%2C)-zp7ib81bka094neal332hc47dnsya/) [http://www.test.com:8080（不同源，因为端口不同）](http://www.test.com:8080（不同源，因为端口不同）/)





### 数据存储

```
var arrDisplay = [0, 1, 1, 1];
//存储，IE6~7 cookie 其他浏览器HTML5本地存储

if (window.localStorage) {
    localStorage.setItem("menuTitle", arrDisplay);    
} else {
    Cookie.write("menuTitle", arrDisplay);    
}
```

\###数据读取

```
var strStoreDate = window.localStorage? localStorage.getItem("menuTitle"): Cookie.read("menuTitle");
```



需要注意的是：虽然我们存储的是数组，但是，实际上存储的的是数组字符化后的字符串(Cookie和localStorage都是)，因此，我们在处理strStoreDate的时候，一定要当作字符串处理，类似下面：

```
strStoreDate.split(",").each(function(display, index) {
    //根据存储的display触发相对应的动作
});
```



在HTML5中，本地存储是一个window的属性，判断浏览器是否支持：

```
if(window.localStorage){
 alert('This browser supports localStorage');
}else{
 alert('This browser does NOT support localStorage');
}
```



存储数据的方法就是直接给window.localStorage添加一个属性

例如：window.localStorage.a 或者 window.localStorage[“a”]。

它的读取、写、删除操作方法很简单，是以键值对的方式存在的，如下：

```
localStorage.a = 3;//设置a为"3"
localStorage["a"] = "sfsf";//设置a为"sfsf"，覆盖上面的值
localStorage.setItem("b","isaac");//设置b为"isaac"
var a1 = localStorage["a"];//获取a的值
var a2 = localStorage.a;//获取a的值
var b = localStorage.getItem("b");//获取b的值
localStorage.removeItem("c");//清除c的值
```



推荐使用的自然是getItem()和setItem()，清除键值对使用removeItem()。

如果希望一次性清除所有的键值对，可以使用clear()。另外，HTML5还提供了一个key()方法，可以在不知道有哪些键值的时候使用，如下：

```
var storage = window.localStorage;
function showStorage(){
 for(var i=0;i");
 }
}
```