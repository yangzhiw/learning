内容如有错误望指正，点击fork,或留言

# 全局安装gulp
```
$ npm install gulp -g
```

# 作为项目依赖安装
```
$ npm install --save-dev gulp
```

# 在项目跟目录下创建 <font color="#c7254e">`gulpfile.js`</font>
```javascript
var gulp = require('gulp');

gulp.task('default', function() {
  // 将你的默认的任务代码放在这
});
```


# 运行gulp
```
$ gulp
```

# gulp方法
## gulp.src(globs[,options])
获取匹配的文件

### globs 
类型：<font color="#c7254e">`String`</font>或<font color="#c7254e">`Array`</font>

#### node-glob语法
+ \*：匹配该路径0个或多个任意字符
```javascript
//*:匹配路径中某部分:0个或多个字符
gulp.src('js/*.js');
```

+ ?: 匹配该路径中1个任意字符
```javascript
//?:匹配路径中某部分:1个字符
gulp.src('js/?.js');
```

+ [...]:匹配该路径段中在指定范围内字符(注意不能组合,只能是其中一个字符)
```javascript
//[]:匹配路径中某部分:指定的范围
//获取js目录下a开头,第二个字符为0-3之间(包括0和3)的js(a03.js不能被匹配到)
gulp.src('js/a[0-3].js');
```

+ \*(pattern|pattern|pattern): 匹配括号中多个模型的0个或多个或任意个的组合(注意|前后不能有空格)
```javascript
//*(pattern|pattern|pattern): 匹配路径中的某部分: 多个模型中的0个或多个.
//除了三个模型本身,如果是组合也可以,比如ab.js,但是仅仅包含某个模型是不行的,比如a4.js.
gulp.src('js/*(a|a1|b).js');
//获取js目录下a.js,a1.js,b.js,或者a,a1,b这几个字符的组合的js,比如ab.js
```

+  !(pattern|pattern|pattern) :匹配不包含任何模型
```javascript
//!(pattern|pattern|pattern): 匹配路径中的某部分: 不包含任何模型.
//带有a或者b的,都排除.需要注意的是,它并非是*(a|b)的取反
gulp.src('js/!(a|b).js');
//获取js目录下名字中不包含a,也不包含b的所有文件.
```

+  ?(pattern|pattern|pattern) : 匹配多个模型中的0个或任意1个(不可以组合.必须完全匹配)
```javascript
//?(pattern|pattern|pattern): 匹配路径中的某部分: 多个模型中的0个或1个.
//精确匹配模型,不可以组合.
gulp.src('js/?(a|a2|b).js');
//获取js目录下a.js,a2.js,b.js
```

+ +(pattern|pattern|pattern) : 匹配多个模型中的1个或多个(必须有一个,为空不匹配)
```javascript
//+(pattern|pattern|pattern): 匹配路径中的某部分: 多个模型中的1个或多个.
//可以是任意一个模型,也可以是他们的组合,比如ab.js
gulp.src('js/+(a|a1|b).js');
//获取js目录下a.js,a1.js,b.js,或者a,a1,b这几个字符的组合的js,比如ab.js
```

+ @(pattern|pat*|pat?erN) : 匹配多个模型中的任意1个(不匹配为空的情况)
```javascript
//@(pattern|pattern|pattern): 匹配路径中的某部分: 多个模型中的1个.
//精确匹配模型,不可以组合.和?的区别就是不可以为空.必须要是其中的一个.
gulp.src('js/@(a|a1|b).js');
```

+ \*\*:可以匹配任何内容,但\*\*不仅匹配路径中的某一段,而且可以匹配 'a/b/c' 这样带有'/'的内容,所以,它还可以匹配子文件夹下的文件.
```javascript
//**: 不是一个单独的路径中的某部分,而是可以带有'/',所以所有当前文件夹和子文件夹下都进行匹配
gulp.src('**/@(a|a1|b).js');
```

## gulp.pipe()
gulp.src()方法返回的对象可以被pipe到别的插件中
例如已经安装了gulp-htmlmin插件对象为<font color="#c7254e">`htmlmin`</font>
```javascript
//gulp-htmlmin插件如何使用，自行搜索
var htmlmin = require('gulp-htmlmin');//html压缩
gulp.task('html',function(){
    var options = {
        collapseWhitespace:true,
        collapseBooleanAttributes:true,
        removeComments:true,
        removeEmptyAttributes:true,
        removeScriptTypeAttributes:true,
        removeStyleLinkTypeAttributes:true,
        minifyJS:true,
        minifyCSS:true
    }
    return gulp.src('app/**/*.html')
               .pipe(htmlmin(options));
});
```

## gulp.dest(path[,options])
能被 pipe 进来，并且将会写文件。并且重新输出（emits）所有数据，因此你可以将它 pipe 到多个文件夹。如果某文件夹不存在，将会自动创建它。
```javascript
var htmlmin = require('gulp-htmlmin');//html压缩
gulp.task('html',function(){
    var options = {
        collapseWhitespace:true,
        collapseBooleanAttributes:true,
        removeComments:true,
        removeEmptyAttributes:true,
        removeScriptTypeAttributes:true,
        removeStyleLinkTypeAttributes:true,
        minifyJS:true,
        minifyCSS:true
    }
    return gulp.src('app/**/*.html')
               .pipe(htmlmin(options))
               .pipe(gulp.dest('dist'));
});
```

## gulp.watch(glob[, opts], tasks)
### globs 
类型：<font color="#c7254e">`String`</font>或<font color="#c7254e">`Array`</font>
一个 glob 字符串，或者一个包含多个 glob 字符串的数组，用来指定具体监控哪些文件的变动。
```javascript
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

## gulp.task(name[, deps], fn)
```javascript
gulp.task('somename', function() {
  // 做一些事
});
```

## gulp常用插件
+ gulp-htmlmin html压缩
+ gulp-uglify js压缩
+ gulp-jshint js语法检查
+ gulp-clean-css css压缩
+ gulp-rename 文件重命名
+ gulp-imagemin 图片压缩 
+ imagemin-pngquant 图片深度压缩
+ gulp-concat 文件合并
+ gulp-connect 服务
+ gulp-proxy-middleware 跨域
+ gulp-clean 清空多余文件

