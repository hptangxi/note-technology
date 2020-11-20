# Node.js 学习笔记

<!-- TOC -->

- [Node.js 学习笔记](#nodejs-学习笔记)
  - [模块的导出](#模块的导出)
  - [nodemon](#nodemon)
  - [nrm](#nrm)
  - [Gulp](#gulp)
      - [作用](#作用)
      - [使用](#使用)
      - [方法](#方法)
      - [插件](#插件)
  - [创建web服务器](#创建web服务器)
  - [异步函数](#异步函数)
  - [全局对象 global](#全局对象-global)
  - [数据库 MongoDB](#数据库-mongodb)
      - [下载安装](#下载安装)
      - [启动和关闭](#启动和关闭)
      - [数据库连接](#数据库连接)
      - [创建数据库](#创建数据库)
      - [创建集合和文档](#创建集合和文档)
      - [数据库导入数据](#数据库导入数据)
      - [查询文档](#查询文档)
      - [删除文档](#删除文档)

<!-- /TOC -->

## 模块的导出

`exports.a = a` 与 `module.exports.a = a` 都指向同一个对象，
`exports = {}` 无效，
`module.exports = {}` 优先级最高，将覆盖上面的写法。

## nodemon

监听文件保存，自动执行修改文件的命令行工具

## nrm

`nrm ls` 查看npm地址列表

`nrm use taobao` 指定npm使用淘宝地址

## Gulp

基于node平台开发的前端构建工具，将机械化操作编写成任务。

#### 作用

- 项目上线，HTML、CSS、JS文件压缩合并
- 语法转换（es6，less...）
- 公共文件抽离
- 修改文件浏览器自动刷新

#### 使用

- 使用`npm install gulp`下载gulp库文件
- 在项目根目录下建立gulpfile.js文件
- 重构项目的文件夹结构，src目录放置源代码文件，dist目录放置构建后文件
- 在gulpfile.js文件中编写任务
- 在命令行工具中执行gulp任务

#### 方法

- `gulp.src()` 获取任务要处理的文件
- `gulp.dest()` 输出文件
- `gulp.task()` 建立gulp任务
- `gulp.watch()` 监控文件的变化

```javascript
const gulp = require('gulp')
// 使用gulp.task()方法建立任务，参数为任务名和回调函数
gulp.task('first', () => {
  // 获取要处理的文件
  gulp.src('./src/css/base.css')
  // 将处理后的文件输出到dist目录
  .pipe(gulp.dest('./dist/css'))
})
```

#### 插件

- gulp-htmlmin: html文件压缩
- gulp-csso：压缩css
- gulp-babel：JavaScript语法转化
- gulp-less：less语法转化
- gulp-uglify：压缩混淆JavaScript
- gulp-file-include：公共文件包含
- browsersync：浏览器实时同步

## 创建web服务器

```javascript
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

## 异步函数

```javascript
const fs = require('fs')
const { promisify } = require('util')
const readFile = promisify(fs.readFile)

async function run () {
  let r1 = await readFile('./1.txt', 'utf8')
  let r2 = await readFile('./2.txt', 'utf8')
  let r3 = await readFile('./3.txt', 'utf8')
  console.log(r1, r2, r3)
}

run()
```

## 全局对象 global

在浏览器中全局对象是window，在Node中全局对象是global

global有以下方法（global可以省略）：

- console.log()
- setTimeout()
- clearTimeout()
- setInterval()
- clearInterval()

## 数据库 MongoDB

术语|解释说明
:-:|:-:
database|数据库，mongoDB数据库软件可以建立多个数据库
collection|集合，一组数据的集合，可以理解为JS的数组
document|文档，一条具体的数据，可以理解为JS的对象
field|字段，文档中的属性名称，可以理解为JS的对象属性

#### 下载安装

同时下载安装MongoDB和MongoDB Compass，使用Node.js操作MongoDB数据库需要依赖Node的第三方包mongoose，在项目中`npm install mongoose`

#### 启动和关闭

- 启动MongoDB：`net start mongodb`
- 关闭MongoDB：`net stop mongodb`

#### 数据库连接

使用mongoose提供的connect方法连接数据库

```javascript
mongoose.connect('mongodb://localhost/databaseName')
  .then(() => console.log('数据库连接成功'))
  .catch(err => console.log('数据库连接失败'), err)
```

#### 创建数据库

在MongoDB中不需要显示创建数据库，如果正在使用的数据库不存在，会自动创建

#### 创建集合和文档

```javascript
// 创建集合规则
const userSchema = new mongoose.Schema({
  name: String,
  age: Number,
  email: String,
  password: String,
  hobbies: [String]
})
// 使用规则创建集合
const User = mongoose.model('User', userSchema)

// 插入数据库的方法有两种
// 方法一：
const user = new User({
  name: 'Sylvia',
  age: 30,
  email: hptangxi@yeah.net,
  password: '0123456789',
  hobbies: ['music', 'coder', 'write']
})
user.save()

// 方法二：
User.create({
  name: 'Sylvia',
  age: 30,
  email: hptangxi@yeah.net,
  password: '0123456789',
  hobbies: ['music', 'coder', 'write']
}).then(doc => {
  // 当前插入的文档
  console.log(doc)
}).catch(err => {
  // 错误对象
  console.log(err)
})
```

#### 数据库导入数据

`mongoimport -d 数据库名称 -c 集合名称 --jsonArray -file 要导入的数据库文件`

#### 查询文档

```javascript
// 根据条件查找文档（条件为空则查找全部文档）
User.find().then(res => {}) // 返回数组
User.findOne({name: 'Sylvia'}) // 返回匹配的第一条数据

// 匹配大于 小于
User.find({age: {$gt: 20, $lt: 50}})

// 匹配包含
User.find({hobbies: {$in: ['coder']}})

// 选择要查询的字段
User.find().select('name email -_id') // 不包含_id

// 将数据按照年龄排序
User.find().sort('age') // 升序
User.find().sort('-age') // 降序

// skip 跳过多少条数据 limit 限制查询数量
User.find().skip(2).limit(2)
```

#### 删除文档

```javascript
// 删除单个
User.findOneAndDelete({_id: 'XXX'}).then(res => {}) // 返回被删除的文档

// 删除多个
User.deleteMany({})
```