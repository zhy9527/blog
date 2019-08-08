title: 'gulp使用记录'
tags: gulp
categories: js
date: 2019-04-18 17:15:37
---
## gulp介绍
gulp是一个前端自动化构建工具，前端开发者可以使用它来处理常见任务:
* 搭建web服务器
* 文件保存时自动重载浏览器
* 使用css预处理器如Sass、LESS
* 优化资源，比如压缩CSS、JavaScript、压缩图片、合并文件
* 资源文件加版本号，刷新加载最新资源，保证资源文件实时更新

## gulp入门指南
> 这部分内容来源于[中文官方文档](https://www.gulpjs.com.cn/docs/getting-started/)
1. 全局安装 gulp：
```
$ npm install --global gulp
```
2. 作为项目的开发依赖（devDependencies）安装，这里为了举例安装gulp和gulp-clean，gulp-clean可以用来清空指定文件夹：
```
$ npm install --save-dev gulp gulp-clean
```
3. 在项目根目录下创建一个名为 gulpfile.js 的文件，[点击可以查看gulp-clean使用方法](http://npm.taobao.org/package/gulp-clean)：
``` javascript
var gulp = require('gulp')、
    clean = require('gulp-clean') //清空文件夹,这里
gulp.task('default', function() {
  // 将你的默认的任务代码放在这
});
//清除dist文件夹
gulp.task('clean',function(){
    return gulp.src('dist', {read: false})
        .pipe(clean());
})
```
4. 运行gulp：
```
$ gulp
```
5. 默认的名为 default 的任务（task）将会被运行，在这里，这个任务并未做任何事情。
想要单独执行特定的任务（task），请输入 `gulp <task> <othertask>`：
```
$ gulp clean
```
接下来介绍下gulp相关的API，gulp的API有`.src`、`.watch`、`.dest`、`.task`。

## gulp API 文档
这里只简单介绍用法，和自己理解，[官方文档](https://www.gulpjs.com.cn/docs/api/)讲解的更为详细，如果想深入学习，请移步官方文档
* **`gulp.src(globs[, options])`**
	匹配想要处理的文件，例如：
	``` javascript
	gulp.src('client/js/*.js') // 对clent/js下面的所有js文件进行处理
	```
* **`gulp.dest(path[, options])`**
	dest就是要把处理完的文件放到指定位置，例如：
	``` javascript
	gulp.src('client/js/*.js') // 对clent/js下面的所有js文件进行处理
	    .pipe(gulp.dest("dist/js/")); // 把处理完的文件放到`dist/js/`这个目录下面
	```
	因为有同事问过pipe方法，这里顺便提下。可以给他理解成一个管道，文件通过管道流入，可能有很多管道相连，每个管道处理不同的事物，a管道负责压缩js，b管道负责修改文件名为jquery.min.js，最后处理完就是压缩后的文件名为jquery.min.js的文件
	关于pipe可以[点击这里查看stream.pipe()](http://nodejs.cn/api/stream.html#stream_event_pipe)，具体不在阐述。
* **`gulp.task(name[, deps], fn)`**
	task定义任务，你要做什么，就是用它定义，例如要定义一个压缩js的任务，代码如下
	``` javascript
	var gulp = require('gulp'),	// gulp基础库
	    uglify = require('gulp-uglify') //压缩Js
	// 压缩JS
	gulp.task('uglifyJs',function(){  // 定义名为uglifyJs的任务，用来压缩js
	    gulp.src("src/content/js/**/*.js")  // 需要压缩的文件是src目录下
	        .pipe(uglify())  // 压缩js
	        .pipe(gulp.dest("dist/content/js/"));  // 压缩完成后放到dist目录下
	})
	```
	如果想执行这个任务，在终端输入以下命令：
	```
	$ gulp uglifyJs
	```
* **`gulp.watch(glob[, opts], tasks)`**
	监听文件变化，如下代码/js/目录下的文件有任何变化，都会执行function
	``` javascript
	gulp.watch('js/**/*.js', function(event) {
	    console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
	});
	```
## 常用插件及说明
```
gulp-sass             // SCSS编译
gulp-sourcemaps       // SCSS地图
gulp-clean            // 清空文件夹
gulp-clean-css        // 压缩CSS
gulp-uglify           // 压缩Js
gulp-concat           // 合并文件
gulp-rev              // 文件加版本号
gulp-rev-collector    // 替换html中资源文件
gulp-minify-html      // 压缩html
gulp-replace          // 替换html中的内容
gulp-modify-css-urls  // 修改css中url路径
gulp-rename           // 重命名文件
```
## 实战
代码是自己项目中，根据需求配置的，有点乱，如有不明白的，欢迎留言
``` javascript
'use strict';
var gulp = require('gulp'), //基础库
    scss = require('gulp-sass'), //SCSS编译
    sourcemaps = require('gulp-sourcemaps'), //SCSS地图
    clean = require('gulp-clean'), //清空文件夹
    cleanCss = require('gulp-clean-css'), //压缩CSS
    uglify = require('gulp-uglify'), //压缩Js
    concat = require('gulp-concat'), //合并文件
    rev = require('gulp-rev'), //文件加版本号
    revCollector = require('gulp-rev-collector'), //替换html文件
    minifyHTML   = require('gulp-minify-html'), //压缩html
    replace = require('gulp-replace'), //替换html内容
    // browserSync = require('browser-sync').create(), //
    // reload = browserSync.reload, //
    modifyCssUrls = require('gulp-modify-css-urls'), //修改css中url路径
    rename = require('gulp-rename'); //重命名文件
var jsSrc = [ // 压缩js按照次序合并
        'src/content/js/base_dev/config.js',
        'src/content/js/base_dev/ajax.js',
        'src/content/js/base_dev/cookie.js',
        'src/content/js/base_dev/jiao.js',
        'src/content/js/base_dev/log.js',
        'src/content/js/base_dev/second-page.js',
        'src/content/js/base_dev/storage.js',
        'src/content/js/base_dev/string.js',
        'src/content/js/base_dev/template-helper.js',
        'src/content/js/base_dev/tools.js',
        'src/content/js/base_dev/common.js',
        'src/content/js/base_dev/tongji.js',
        // 'src/content/js/base_dev/wechat.js'
    ],
    revSrc = [ // 加版本号的资源
        'dist/**',
        '!dist/**/*.html',
        '!dist/content/swiper/**',
        '!dist/content/mobiscroll/**',
        '!dist/content/font/**',
        '!dist/**/*.json',
        '!dist/**/*.ico',
        '!dist/**/*.map',
    ];
// 将你的默认的任务代码放在这
gulp.task('default',['clean','copyDist','cleanAvail','rev','cleanCss','uglifyJs','revCollector'], function() {
});
// 编译Scss
gulp.task('scss',function(){
    return gulp.src('src/content/scss/*.scss')
    .pipe(sourcemaps.init())
    .pipe(scss().on('error',scss.logError))
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('src/content/css/'));
})
// 合并js
gulp.task('concatJs', function() {
    return gulp.src(jsSrc)
    .pipe(concat('global.js'))    //合并所有js到global.js
    .pipe(gulp.dest('src/content/js/base/'))    //输出main.js到文件夹
});
// 监听任务
gulp.task('watch',function(){
    var watcher = gulp.watch(['src/content/scss/**','src/index.html','src/content/js/base_dev/*.js'], ['scss','concatJs']);
    watcher.on('change', function(event) {
      console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
    });
})
//清除文件夹
gulp.task('clean',function(){
    return gulp.src('dist', {read: false})
        .pipe(clean());
})
// 压缩CSS
gulp.task('cleanCss',['rev'],function(){
    gulp.src("dist/content/css/*.css")
        // .pipe(rename({
        //     dirname: "../dist/content/css/",
        //     basename: "shu",
        //     prefix: "",
        //     suffix: ".min",
        //     extname: ".css"
        // }))
        .pipe(cleanCss())
    .pipe(gulp.dest("dist/content/css/"));
})
// 压缩CSS
gulp.task('cleanCssToV2',function(){
    gulp.src("src/content/css/*.css")
        // 替换css文件中的images路径
        .pipe(modifyCssUrls({
            modify: function (url, filePath) {
                if (url.indexOf('../images') != -1) {
                    return url.replace('../images','/images/v3')
                }
                if (url.indexOf('../font') != -1) {
                    return url.replace('../font','../../fonts/v3')
                }
            },
        }))
        .pipe(rename({
            dirname: "../../wap_v2/css/v3",
            prefix: "",
            suffix: ".min",
            extname: ".css"
        }))
        .pipe(cleanCss())
    .pipe(gulp.dest('src'));
})
// 压缩JS
gulp.task('uglifyJs',['rev'],function(){
    gulp.src("dist/content/js/**/*.js")
        .pipe(uglify())
    .pipe(gulp.dest("dist/content/js/"));
})
// 文件加版本号
gulp.task('rev',['cleanAvail'],function(){
    return gulp.src(revSrc)
        .pipe(rev())
        .pipe(gulp.dest('dist'))
        .pipe(rev.manifest())
        .pipe(gulp.dest('dist'));
})
// 替换html中的文件路径，并压缩html
gulp.task('revCollector', ['rev'], function () {
    return gulp.src(['dist/rev-manifest.json', 'dist/**/*.html'])
        .pipe( revCollector() )
        .pipe( minifyHTML({
                empty:true,
                spare:true
            }) )
        .pipe( gulp.dest('dist') );
});
// 复制dist文件夹
gulp.task('copyDist',['clean'],function(){
    return gulp.src('src/**')
        .pipe(gulp.dest('dist'))
})
// 清除线上不用文件，scss，base-dev等
gulp.task('cleanAvail',['copyDist'],function(){
    return gulp.src(['dist/content/scss','dist/content/js/base_dev'])
        .pipe(clean());
})
// 构建
gulp.task('build',['clean','copyDist','cleanAvail','cleanCss','rev','revCollector'],function(){
})
// 静态服务器
// gulp.task('browser-sync', function() {
//     browserSync.init({
//         server: {
//             baseDir: "src/index.html"
//         }
//     });
// });
// 代理
// gulp.task('browser-sync', function() {
//     browserSync.init({
//         proxy: "你的域名或IP"
//     });
// });
// 生成对应文件到v2
gulp.task('fileToV2',['cssToV2','jsToV2','imgToV2','htmlToV2'],function(){
})
gulp.task('cssToV2',function(){
    gulp.src("dist/content/css/*.css")
        // 替换css文件中的images路径
        .pipe(modifyCssUrls({
            modify: function (url, filePath) {
                if (url.indexOf('../images') != -1) {
                    return url.replace('../images','/images/v3')
                }
                if (url.indexOf('../font') != -1) {
                    return url.replace('../font','../../fonts/v3')
                }
            },
        }))
    .pipe(gulp.dest('../wap_v2/css/v3'));
})
gulp.task('jsToV2',function(){
    gulp.src(["!dist/content/js/cycle/**","!dist/content/js/index/**","dist/content/js/**/*.js"])
    .pipe(gulp.dest('../wap_v2/js/v3'));
})
gulp.task('imgToV2',function(){
    gulp.src("dist/content/images/**")
    .pipe(gulp.dest('../wap_v2/images/v3'));
})
gulp.task('htmlToV2',function(){
    gulp.src("dist/**/*.html")
    // 替换css文件中的images路径
    .pipe(replace('../content/css', '/css/v3'))
    .pipe(replace('../content/js', '/js/v3'))
    .pipe(replace('../content/images', '/images/v3'))
    .pipe(replace('../content/mobiscroll', '/js/v3/mobiscroll'))
    .pipe(replace('../content/swiper', '/js/v3/swiper'))
    .pipe(gulp.dest('../wap_v2/statics'));
})
```