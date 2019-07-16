---
title: Leo用sublime记事for mac
date: 2015-06-01 20:32:07
tags: mac
---

记录 Leo 的IDE 一些开发配置

<!-- more -->

### 包管管理神器

最近Package Control好像被墙了, 我的另一台电脑老是上不去, 具体不太清清楚, 翻墙中

安装过程: 使用快捷键 control + ` 或者菜单栏选择View > Show Console

如果安装失败留意提示，到官方复制最新的代码

### Sublime Text3在控制台输入

```
import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open()os.path.join( ipp, pf), 'wb' ).write(by)    
```

### Sublime Text2在控制台输入

```
import urllib2,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')
```

##### 打开包管理神器shift + cmd + p, 然后输入install package

#### 配置快捷键的时候，写super是command，写alt是option，写ctrl是control，写shift是shift

------

### 插件：

#### emmet

前端神器, 减少大量的工作量, 使用方法可以参考Emmet：HTML/CSS代码快速编写神器或者官方文档

#### Bracket Highlighter

这是用来做括号匹配高亮的，可以在包管理器中安装。Sublime Text 2自带的括号匹配只有小小的一横线，太不显眼了，这个可以让高亮显示在行号那里, 非常清晰

#### HTML-CSS-JS Prettify

漂亮代码的，按command+shift+h