---
title: gulp
date: 2020-04-15 19:39:06
tags:
    -nodejs
categories:Nodejs 
---

## 第三方模块gulp
它可以项目上线，html css js文件压缩合并
语法转换，es6 less
公共文件抽离
修改文件浏览器自动刷新
etc...

使用
+ 根目录下有gulpfile.js
+ src目录存放源代码文件
+ dist目录放置构建后的文件
+ 在gulpfile.js中编写任务
+ 命令行执行gulp任务

gulp提供的方法
+ gulp.src():获取任务要处理的文件  不仅可以直接穿路径字符串也可以穿数组数组里是路径字符串
+ gulp.dest():输出文件
+ gulp.task():建立gulp任务
+ gulp.watch(): 监控文件的变化

具体使用
```
const gulp = require('gulp');
gulp.task('first',(done)=>{    //first是这个任务的名字，task建立任务
    gulp.src('./src/css/base.css')   // 通过src获取到文件
    .pipe(gulp.dest('./dist/css'))   //pipe是管道就是获取之后的文件放进管道之后由dest输出书去，总的来说这个处理就是复制的操作
    done();    //新版的gulp中用done返回结束因为新版不再接受同步函数
})
```

gulp还提供了命令行工具去执行它gulp-cli
gulp指令执行
gulp + 任务名执行任务 

### gulp插件
在npmjs中查找怎么用
+ gulp-htmlmin html文件压缩
+ gulp-csso 压缩css
+ gulp-babel js语法转化
+ gulp-less less语法转化
+ gulp-uglify 压缩混淆hs
+ gulp-file-include 公共文件部分引入  
  - 公共部分放在某一个html中
  - 任务中用.pipe(fileinclude())来进行公共部分引入
  - 公共文件该引用的部分用@@include('common_path')来引用  
+ browsersync 浏览器实时同步

使用:1.下载2.引用3.调用

```
const gulp = require('gulp')
gulp.task('first',()=>{
    console.log('人生中的第一个gulp');
    gulp.src('./src/css/base.css').pipe(gulp.dest('./dist/css'));
})

////抽取html公共代码并html文件压缩

const htmlmin = require('gulp-htmlmin')
const fileinclude = require('gulp-file-include')
gulp.task('htmlmin',(done)=>{
    gulp.src('./src/*.html').pipe(fileinclude()).pipe(htmlmin({ collapseWhitespace: true })).pipe(gulp.dest('./dist'))
    done()
})


//less语法转换
//css代码压缩
const less = require('gulp-less')
const csso = require('gulp-csso')
gulp.task('cssmin',(done)=>{
    gulp.src(['./src/css/*.less','./src/css/*.css']).pipe(less()).pipe(csso()).pipe(gulp.dest('./dist/css'))
    done();
})

//es6代码转换
//代码压缩
const uglify = require('gulp-uglify');
const babel = require('gulp-babel')
gulp.task('jsmin',(done)=>{
    gulp.src('./src/js/*.js').pipe(babel({
        //他可以判断当前代码的运行环境将代码转换为当前运行环境所支持的代码
        presets: ['@babel/env']
    })).pipe(uglify())
    .pipe(gulp.dest('./dist/js'))
    done();
})

//复制文件夹
gulp.task('copy',(done)=>{
    gulp.src('./src/images/*').pipe(gulp.dest('./dist/images'));
    gulp.src('./src/lib/*').pipe(gulp.dest('./dist/lib'))
    done();
})

//建立总任务使得所有分任务也一起执行
//构建任务
gulp.task('default',gulp.series('htmlmin','cssmin','jsmin','copy'));  //新版的gulp如果要执行其他的任务要将任务名放在gulp.series()中

```

### node_modules文件夹的问题
1.文件夹过多过碎将项目整体拷贝传输效率低 
2.复杂的模块依赖关系需要被记录，确保模块的版本当前一致，否则项目会运行报错
### package.json
所以package.json文件可以很好的记录
npm init -y 可以生成，-y代表不填写任何东西所有的写成默认值
npm install 可以在根目录中自动下载package.json中的依赖模块(dependencies和devdependencies)
npm install --production 会下载dependencies依赖模块，因为是运营环境
npm install gulp --save-dev 会将gulp放在package.json中的devDependencies中用作区分，因为gulp是开发阶段中的模块线上运行不需要依赖此模块
npm在下载依赖模块中npm会同时生成package-lock.json文件记录模块依赖关系，会锁定包的版本，加快下载速度
package.json文件中的script是记录别名的比如 "bulid":"nodemon app.js" 那么执npm run build相当于执行了nodemon app.js
### require
require模块时会执行模块里面的代码
require()参数省略了js后缀会先找同名的js文件没有的话会找同名文件夹下的jindex.js,再没有的话就找同名文件夹下的package.json文件，找json中的main制定的入口js文件，
require如果即省略了路径又省略了后缀那么会当作系统模块来查找，在node_module文件夹中找，没有找同名文件夹中index没有找同名文件夹中的package.json中的main入口js文件


