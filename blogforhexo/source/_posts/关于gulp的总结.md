---
title: 关于gulp的总结
date: 2015-08-31 22:32:37
tags: gulp
---

gulp 就像subime 这种编辑器一样，轻量级，自己想需要什么功能，就折腾什么插件进来。
fis 就像webstorm这种ide一样，有一些基础要求，比如说fis必须以项目的形式运行，内置了集成了大部分常用插件，拥有一套比较强大的的解决方案

<!-- more -->

gulp轻量级，你的项目可能由于历史原因，或者其他原因，fis的一些基础要求可能和你项目有冲突。比如你可能只想处理整个项目中的一个模块，或者你不太想在本地开发使用绝对路径，或者你的项目和程序员分工页面模板（jsp，php等）和前端资源不在同一个资源位置。这个时候你更适合使用gulp来定制自己的解决方案。

但是gulp使用者来说，并不是每个人都有非常强的处理错误能力，如果遇到插件bug（当然这种情况很少见），需要联系作者，这个是一件非常棘手的事情。但是这种风险是存在的

fis相对来说因为有专门的QQ群天天为用户答疑解惑收集bug处理bug，压根就不用担心太多问题。另外fis的一些解决方案确实是目前前端优化里面会需要真实考虑面对的。接触fis会让你对整个前端的优化和加载管理有更深入的了解，当然如果你已经了解很透彻了。我相信对于选择gulp 和fis这种困惑应该也不会存在


#### 首先要确保pc上装有node，然后在global环境和项目文件中都要安装gulp
npm install gulp -g   (全局)
npm install gulp --save-dev (项目环境)

#### 在项目中install需要的gulp插件，一般需要：
npm install gulp-util gulp-imagemin gulp-ruby-sass gulp-minify-css gulp-minify-html gulp-jshint gulp-uglify gulp-rename gulp-concat gulp-clean gulp-livereload tiny-lr --save-dev

#### 在项目的根目录新建gulpfile.js，require需要的module
var gulp = require('gulp'),
    minifycss = require('gulp-minify-css'),
    concat = require('gulp-concat'),
    uglify = require('gulp-uglify'),
    rename = require('gulp-rename'),
    del = require('del');
	
#### 压缩css
gulp.task('minifycss', function() {
    return gulp.src('src/*.css')      //压缩的文件
        .pipe(gulp.dest('minified/css'))   //输出文件夹
        .pipe(minifycss());   //执行压缩
});


#### 压缩js
gulp.task('minifyjs', function() {
    return gulp.src('src/*.js')
        .pipe(concat('main.js'))    //合并所有js到main.js
        .pipe(gulp.dest('minified/js'))    //输出main.js到文件夹
        .pipe(rename({suffix: '.min'}))   //rename压缩后的文件名
        .pipe(uglify())    //压缩
        .pipe(gulp.dest('minified/js'));  //输出
});


#### 执行压缩前，先删除文件夹里的内容
gulp.task('clean', function(cb) {
    del(['minified/css', 'minified/js'], cb)
});
7


#### 默认命令，在cmd中输入gulp后，执行的就是这个命令
gulp.task('default', ['clean'], function() {
    gulp.start('minifycss', 'minifyjs');
});


然后只要cmd中执行，gulp即可


#### 附上项目中的gulpfile代码：
```
// 引入 gulp及组件
var gulp    = require('gulp'),                 //基础库
    imagemin = require('gulp-imagemin'),       //图片压缩
    // sass = require('gulp-ruby-sass'),          //sass    使用前要安装哦
    minifycss = require('gulp-minify-css'),    //css压缩
    jshint = require('gulp-jshint'),           //js检查
    uglify  = require('gulp-uglify'),          //js压缩
	//jsmin = require('gulp-jsmin'),
    rename = require('gulp-rename'),           //重命名
    concat  = require('gulp-concat'),          //合并文件
    clean = require('gulp-clean'),             //清空文件夹    （安装时报了错。可能网络暂时不行，迟下装）
    //tinylr = require('tiny-lr'),               //livereload
    //server = tinylr(),
    minifyhtml = require('gulp-minify-html'),   // 压缩html
    port = 35729;
    //livereload = require('gulp-livereload');   //livereload


​	
// src: 指定源文件    Dst : 处理后文件的路径
​	
// HTML处理
gulp.task('html', function() {
​    var htmlSrc = './report_src/*.html',
​        htmlDst = './report/';

    gulp.src(htmlSrc)
        //.pipe(livereload(server))
        .pipe(minifyhtml())
        .pipe(gulp.dest(htmlDst));
});


// css处理
gulp.task('css', function() {
    var cssSrc = './report_src/css/*.css',
        cssDst = './report/css';

    gulp.src(cssSrc)
        
    	.pipe(minifycss())
        .pipe(gulp.dest(cssDst));
        //.pipe(livereload(server))
        //.pipe(gulp.dest(cssDst));
});


// 图片处理
gulp.task('images', function(){
    var imgSrc = './report_src/images/**/*',
        imgDst = './report/images';
		
    gulp.src(imgSrc)
        // .pipe(imagemin())
        //.pipe(livereload(server))
        .pipe(gulp.dest(imgDst));
})


// js处理
gulp.task('js', function () {
    var jsSrc = './report_src/js/*.js',
        jsDst ='./report/js';

    gulp.src(jsSrc)
         //.pipe(jshint())
         //.pipe(jshint.reporter('HE_HE_DA'))
        // .pipe(concat('main.js'))
        // .pipe(gulp.dest(jsDst))
        // .pipe(rename({ suffix: '.min' }))
        .pipe(uglify())
        // .pipe(livereload(server))
        .pipe(gulp.dest(jsDst));
});


// 清空图片、样式、js、html
gulp.task('clean', function() {
    gulp.src(['./report/css', './report/js', './report/images', './report/*.html'], {read: false})
        .pipe(clean());
});

// 清空html
gulp.task('cleanhtml', function() {
    gulp.src(['./report/*.html'], {read: false})
        .pipe(clean());
});

// 清空js
gulp.task('cleanjs', function() {
    gulp.src(['./report/js'], {read: false})
        .pipe(clean());
});

// 清空css
gulp.task('cleancss', function() {
    gulp.src(['./report/css'], {read: false})
        .pipe(clean());
});

// 清空images
gulp.task('cleanimages', function() {
    gulp.src(['./report/images'], {read: false})
        .pipe(clean());
});


// 默认任务  清空图片、样式、js并重建 运行语句 gulp
gulp.task('default', ['clean'], function(){
    gulp.start('html','css','images','js');
});


// 监听任务 运行语句 gulp watch
gulp.task('watch',function(){

    server.listen(port, function(err){
        if (err) {
            return console.log(err);
        }
    
        // 监听html
        gulp.watch('./report_src/*.html', function(event){
            gulp.run('html');
        })
    
        // 监听css
        gulp.watch('./report_src/css/*.css', function(){
            gulp.run('css');
        });
    
        // 监听images
        gulp.watch('./report_src/images/**/*', function(){
            gulp.run('images');
        });
    
        // 监听js
        gulp.watch('./report_src/js/*.js', function(){
            gulp.run('js');
        });
    
    });
});
```