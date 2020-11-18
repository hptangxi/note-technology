### 模块的导出

`exports.a = a` 与 `module.exports.a = a` 都指向同一个对象，
`exports = {}` 无效，
`module.exports = {}` 优先级最高，将覆盖上面的写法。

### 第三方模块

#### nodemon

监听文件保存，自动执行修改文件的命令行工具

#### nrm

`nrm ls` 查看npm地址列表

`nrm use taobao` 指定npm使用淘宝地址

#### Gulp

基于node平台开发的前端构建工具，将机械化操作编写成任务。

**作用：**
- 项目上线，HTML、CSS、JS文件压缩合并
- 语法转换（es6，less...）
- 公共文件抽离
- 修改文件浏览器自动刷新

**使用：**
1. 使用`npm install gulp`下载gulp库文件
2. 在项目根目录下建立gulpfile.js文件
3. 重构项目的文件夹结构，src目录放置源代码文件，dist目录放置构建后文件
4. 在gulpfile.js文件中编写任务
5. 在命令行工具中执行gulp任务

**方法：**
- `gulp.src()` 获取任务要处理的文件
- `gulp.dest()` 输出文件
- `gulp.task()` 建立gulp任务
- `gulp.watch()` 监控文件的变化

```
const gulp = require('gulp')
// 使用gulp.task()方法建立任务，参数为任务名和回调函数
gulp.task('first', () => {
  // 获取要处理的文件
  gulp.src('./src/css/base.css')
  // 将处理后的文件输出到dist目录
  .pipe(gulp.dest('./dist/css'))
})
```

**插件：**

- gulp-htmlmin: html文件压缩
- gulp-csso：压缩css
- gulp-babel：JavaScript语法转化
- gulp-less：less语法转化
- gulp-uglify：压缩混淆JavaScript
- gulp-file-include：公共文件包含
- browsersync：浏览器实时同步


### 服务器端

#### 创建web服务器

```
// 引用系统模块
const http = require('http')
// 创建web服务器
const app = http.createServer()
// 当客户端发送请求的时候
app.on('request', (req, res) => {
  // 响应
  res.end('<h1>hi, user</h1>')
})
// 监听3000端口
app.listen(3000)
console.log('服务器已启动，监听3000端口，请访问localhost:3000')
```