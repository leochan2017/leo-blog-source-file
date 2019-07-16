---
title: 抛弃jquery
date: 2015-09-03 19:32:07
tags: javascript
---

### ES5前，HTML5前流行的jQuery

jQuery之所以流行，因为他做了一点事，并没有做许多事。

他做的，是解决DOM操作，事件，AJAX，浏览器兼容等核心的事。

已经足够了，多了就是累赘，占流量，拖速度。

他只编写20%的代码，解决你80%的问题。至于剩下的20%的各个业务场景下都不同的功能，交给你去解决。

这就是低侵入。

<!-- more -->

### 为什么我离开了jQuery的怀抱？

- 浏览器把他淘汰，现代浏览器原生API已经足够好用。
- 体积大，特别不适合于移动端项目。于是有了很多精简jq的项目，如 Zepto Underscore Lodash。（如果用zepto，还多出一个“点击穿透”的问题）
- 依赖标签，大量的DOM操作使得钩子依赖标签，维护上有困难。如果要改到页面结构，会导致依赖标签的选择器那块js跟着大改。
- 兼容性，jq每个新版本都不能兼容早期的版本，如selector。这会影响到一些插件和代码
- 插件冲突，同一个页面上使用多个插件，很容易碰到冲突现象。如同时依赖相同事件或者selector。

------

### 框架的兴起

因不满 jQuery 写出来的“意大利面条”式代码，于是有了一些框架，最早的MVC框架例如Backbone，火了没多久。

这些MVC框架很多的都在做“绑定事件”和“更新视图”这些事情。

于是有了MVVM框架（Angular，React，VUE）。

------

### 我们真的还有必直接去操作DOM？

本质上，jQuery 仅仅是方便操作DOM的工具库，现在有这么多热门的MVVM框架，你还用操心VIEW吗？

这时候jQuery最大的优势已经显得鸡肋了。

------

### 一些部分常用的jQuery操作，用原生实现也不麻烦

#### 选择DOM元素

jq中有很多出色的选择器。在原生中，可以用querySelectorAll来模拟这个功能。

```
var $ = document.querySelectorAll.bind(document);
```

不同的是，querySelectorAll返回的是一个像数组（有数组索引和length属性）但不是数组的NodeList对象，可以将NodeList转换成数组。

```
var myList = Array.prototype.slice.call(myNodeList);
```

#### 尾部追加

```
// jq:
$(parent).append($(child));

// 原生:
parent.appendChild(child);
```

#### 头部插入

```
// jq:
$(parent).prepend($(child));

// 原生:
parent.inserBefore(child, parent.childrenNodes[0]);
```

#### 删除元素

```
// jq:
$(child).remove();

// 原生:
child.parentNode.removeChild(child);
```

*为了防止内存泄露，jq在删除DOM元素前会对元素上已绑定的事件进行清理操作，再用原生JS实现的时候也要注意到这点* #### 事件监听 可以通过给元素挂载addEventListener来模拟jq的on方法。

```
Element.prototype.on = Element.prototype.addEventListener;
```

也可以在NodeList里加上这个方法:

```
NodeList.prototype.on = function(e, fn) {
    []['forEach'].call(this, function(el) {
        el.on(e, fn);
    });

    return this;
}
```

#### 事件触发

jq的trigger则需要单独挂载

```
Element.prototype.trigger = function (type, data) {
    var e = document.createEvent('HTMLEvents');

　　e.initEvent(type, true, true);
　　
　　e.data = data || {};
　　
　　e.eventName = type;
　　
　　e.target = this;
　　
　　this.dispatchEvent(e);
　　
　　return this;
};
```

同理，NodeList对象也可以:

```
NodeList.prototype.trigger = function (e) {
　[]['forEach'].call(this, function (el) {
　　　　el['trigger'](e);
　});

　return this;
};
```

#### document.ready

现在都把js放在页面底部加载，所以document.ready已经不需要了。因为等到运行的时候DOM已经生成号了。

#### 读写元素的属性

```
// jq:
$('#image').attr('src', 'http://www.leojs.com/logo.png');

// 原生:
$('#image').src = 'http://www.leojs.com/logo.png';
```

*PS：input元素的this.value返回的是输入的值，a元素的this.href返回的是绝对URL。如需要的到准确值，可以用 this.getAttribute(‘value’) 和 this.getAttribute(‘href’)*

#### addClass

```
// jq:
$('body').addClass('body-style');

// 原生:
document.body.className = 'body-style';

或者

document.body.className += 'body-style';

// HTML5还可以用classList对象 （注意IE 9不支持）:
document.body.classList.add('body-style');

document.body.classList.remove('body-style');

document.body.classList.toggle('body-style');

document.body.classList.contains('body-style');
```

#### CSS

```
// jq:
$('#el').css('color', 'red');

// 原生:
el.style.color = 'red';

或者

el.style.cssText += 'color: red';
```

#### 元素上存储数据

```
// jq:
$('#el').data('userName', 'leo');

// HTML5（注意IE 10不支持，且只能保存字符串）:
el.dataset.userName = JSON.stringify(userName);

el.dataset.score = score;
```

#### AJAX

个人感觉链式更好用

```
/**
 * [可链式调用的ajax]
 * @param  {[object]} options [初始化参数对象，传入后会替换掉原参数，一般不用传]
 * 
 * 链式调用方式：ajax().before([Function]).get|post(url,data).always([Function]).succ([Function]).fail([Function])
 * 
 * 注：always、succ、fail可连续调用多次，即succ().succ().succ()...
 */
leo.ajax = function(options) {
    // 默认初始化参数对象，可悲options替换
    var _options = {
        async: true, // 是否异步
        contentType: 'application/json; charset=utf-8', // POST时，head编码方式，默认json
        jsonForce: true // 是否强制要求返回格式为json
    };
    // 覆盖默认参数对象
    if (Object.prototype.toString.call(options) == '[object Object]') {
        for (var pname in options) {
            _options[pname] = options[pname];
        }
    }
    // 生成xhr对象，懒函数
    var createXHR = function() {
        try {
            xhr = new XMLHttpRequest(); // 直接创建
            createXHR = function() {
                return new XMLHttpRequest()
            };
        } catch (e) {
            try {
                xhr = new ActiveXObject('Msxml2.XMLHTTP'); // IE高版本创建XMLHTTP
                createXHR = function() {
                    return new ActiveXObject('Msxml2.XMLHTTP')
                };
            } catch (e) {
                try {
                    xhr = new ActiveXObject('Microsoft.XMLHTTP'); // IE低版本创建XMLHTTP
                    createXHR = function() {
                        return new ActiveXObject('Microsoft.XMLHTTP')
                    };
                } catch (e) {
                    xhr = null;
                    createXHR = function() {
                        return null
                    };
                }
            }
        }
        return xhr;
    }
    var xhr = createXHR();
    var doAjax = function(url, data, method) {
        // 开始执行ajax
        xhr.onreadystatechange = function() {
            if (xhr.readyState == 4) {
                // 执行always里面的函数
                for (var i = 0, l = main.alwaysCallbacks.length; i < l; i++) {
                    main.alwaysCallbacks[i](xhr.responseText, xhr);
                }
                // 返回结果转换为json
                var resJson;
                try {
                    resJson = JSON.parse(xhr.responseText || null);
                } catch (e) {
                    resJson = undefined;
                }
                var status = xhr.status;
                if (status < 200 || (status >= 300 && status != 304) || (_options.jsonForce && typeof(resJson) === 'undefined')) {
                    // 执行errCallbacks里面的函数
                    for (var i = 0, l = main.errCallbacks.length; i < l; i++) {
                        main.errCallbacks[i](xhr.responseText, xhr);
                    }
                } else {
                    // 执行sucCallbacks里面的函数
                    for (var i = 0, l = main.sucCallbacks.length; i < l; i++) {
                        main.sucCallbacks[i](resJson, xhr);
                    }
                }
            }
        }
        xhr.open(method, url, _options.async);
        // 如果是post要改变头
        method === 'POST' && xhr.setRequestHeader('Content-Type', _options.contentType);
        xhr.send(data || null);
    }

    var main = {
        sucCallbacks: [],
        errCallbacks: [function(err) { console.error('responseText:' + err) }],
        alwaysCallbacks: [],
        options: _options,
        get: function(url, data) {
            doAjax(url, data, 'GET');
            return main;
        },
        post: function(url, data) {
            doAjax(url, data, 'POST');
            return main;
        }
    };

    /**
     * 设置多个请求头
     * @param  {object} headers
     */
    main.headers = function(headers) {
        if (Object.prototype.toString.call(headers) === '[object Object]') {
            for (var name in headers) {
                console.log(name + ' ' + headers[name]);
                console.log(xhr)
                xhr.setRequestHeader(name, headers[name]);
            }
        }
    };

    /**
     * 设置前置处理方法
     * @param  {Function} callback
     */
    main.before = function(callback) {
        typeof(callback) === 'function' && callback(xhr);
        return main;
    };

    /**
     * 分别是成功、失败、完成的回调函数
     * @param  {Function} callback [description]
     */
    main.succ = function(callback) {
        typeof(callback) === 'function' && main.sucCallbacks.push(callback);
        return main;
    };

    main.fail = function(callback) {
        typeof(callback) === 'function' && main.errCallbacks.push(callback);
        return main;
    };

    main.always = function(callback) {
        typeof(callback) === 'function' && main.alwaysCallbacks.push(callback);
        return main;
    };

    return main;
}
```

#### 动画

```
// jq:
$foo.animata('slow', { x: '+=5px' });

// 原生:
foo.classList.add('slow');

// 现在的CSS 3动画做的比DOM强大，所以建议用CSS把动画效果写出来，然后通过操作class来展示动画
```

如果需要回调函数 ，CSS 3也是有对应的事件

```
el.addEventListener("webkitTransitionEnd", transitionEnded);

el.addEventListener("transitionend", transitionEnded);
```

*PS:这里有更多更详细的jq代码替代总结：http://youmightnotneedjquery.com/*

------

### 最后

更多可以降低后期维护成本，多人协作成本，系统拓展成本等的开发解决方案正在来临。

jQuery已经退出历史舞台，而他已经成为了规范的一部分。

你不再需要引入jQuery，因为他已经在浏览器中。

生而不有，为而不恃，功成而弗居；夫唯弗居，是以不去。

