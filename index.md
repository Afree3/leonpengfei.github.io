# express框架

## 安装

```shell
npm i --save express
```

## hello world

```javascript
var express = require('express')

var app = express()

app.get('/', (req, res) =>{
    res.send('hello world.')
})

app.listen(3000, () =>{
    console.log('app server is running...')
})
```

## 基本路由

get:

```javascript
app.get('/', (req, res) =>{
    res.send('hello world.')
})
```

post:

```javascript
app.post('/', (req, res) => {
    res.send('Got a POST request.')
})
```

## 静态服务

```javascript
app.use(express.static('public'))
app.use(express.static('files'))
app.use('/static', express.static('public'))
app.use('/static', express.static(path.join(__dirna)))
```

## 配置 `atr-template` 模板引擎

[art-template Github 仓库](https://github.com/aui/art-template)

[art-template 官方网址](https://aui.github.io/art-template/zh-cn/index.html)

安装：

```shell
npm i art-template -S
npm i express-art-template -S
```

配置：

```javascript
// 当渲染以 .html 结尾的文件时，使用 art-template 模板引擎
// 在外部不用加载 art-template 但是也必须安装，因为 express-art-template 依赖了 art-template
app.engine('html', require('express-art-template'))
```

使用：

```javascript
// express 为 response 响应对象提供了一个方法：render，该方法只有在配置了模板引擎时才可用
app.get('/', (req, res) => {
    // 模板文件   {模板数据}
    // 模板文件不能写路径，因为express默认从项目的 views 目录中查找该模板文件
    // express 约定：所有的视图文件都存放在 views 目录中
    res.render('index.html', {
    	conments: comments
	})
})
app.get('/post', (req, res) => {
    res.render('post.html')
})
app.get('/pinglun', (req, res) => {
    var comment = req.query
    comments.unshift(comment)
    res.redirect('/')
})
```

修改默认的 `views` 视图渲染存储目录

```javascript
app.set('views', 目录路径)
```

## 在 express 中获取 GET 请求参数

express 内置了一个 API ，可以直接通过 `req.query` 来获取

```
req.qurey
```



## 解析表单 post 请求体（获取post 数据）

在express中没有内置获取表单POSTt请求体数据的API，我们需要使用一个第三方包：`body-parser`

安装：

```shell
npm i --save body-parser
```

配置：

```javascript
var express = require('express')
// 0. 引包
var bodyParser = require('body-parser')

var app = express()

// 配置 body-parse
// 加入这个配置后，在 req 请求对象上回多出来一个属性：body
// 我们就可以通过 req.body 来获取表单 POST 请求体数据了
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))

// parse application/json
app.use(bodyParser.json())
```

使用：

```javascript
app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  // 可以通过 req.body 来获取表单 POST 请求体数据
  res.end(JSON.stringify(req.body, null, 2))
})
```



