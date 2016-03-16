---
version: v0.1.0
category: 'API'
title: 目录
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/apis/v0.1.0/README.md'
permalink: /apis/v0.1.0/index.html
---

请注意您正在查看的文档是正确的版本，版本号在url中有所体现，如果错了，这里有所有版本的文档，[请切换](/apis/).

# API



## lor

```lua
local lor = require("lor.index")
```

lor对象: lor框架暴露出来的最重要对象，理论上通过调用该对象的方法和属性即可使用绝大部分lor框架提供的功能。



### lor()

```lua
local lor = require("lor.index")
local app = lor()
```

该方法是框架暴露出来的最重要的方法，用于创建一个app对象，之后大部分api将通过`app`对象展开.  
如无特别说明，下文中出现的`app`均指通过`lor()`方法创建的application对象.


### lor:Router()

生成一个`group router`对象，`group router`对象指一个`路由组`，用来在业务上聚合一组相关的路由，它具有`get`、`post`、`delete`、`put`等等HTTP方法，这些API与下文介绍的`app`上的`get`、`post`、`delete`、`put`等方法使用方式一致。








## app对象


### app:conf(key, value)

application配置项，目前主要用于html模板配置，提供一下四个配置项：

- app:conf("view enable", true), 开启模板功能，默认为关闭
- app:conf("view engine", "tmpl")， 配置模板引擎，当前lor只支持lua-resty-template，所以这个值暂时固定为"tmpl"
- app:conf("view ext", "html")，模板文件后缀，用户可自由配置
- app:conf("views", "./app/views")，模板文件所在路径


### app:use(path, middleware)

- app:use用于加载一个插件(middleware)
- 参数说明
	- path, 插件作用的路径，可以为空，也就是说app:use可以只有一个`middleware`参数，这时插件作用在所有path上
	- middleware，插件，格式为`function(req, res, next) end`, 对请求做预处理或者善后处理

- 示例1：

该实例加载了一个作用在所有路径上的插件，它的作用是在请求上注入了一个参数`inject_param`,这样后续匹配到的路由可以使用这个参数。

```lua
app:use(function(req, res, next)
    -- 注入一个参数
    req.params.inject_param = "from all path middleware" 
    next() -- 注意，如果不调用next()方法，请求到这里就截止了，不在匹配后面的路由
end)
```

	
- 示例2：

该实例类似于一个拦截器，app加载了一个作用在前缀为"/user/"路径上的插件。
插件的作用是判断传入的请求参数里是否含有已经被授权的token，没有则直接返回错误，如果有就放过，继续下个路由。

```lua
app:use("/user", function(req, res, next)
    if not req.params or req.params.token ~= "authorized token" then
    	-- 注意，这里没有调用next()方法，请求到这里就截止了，不在匹配后面的路由
    	res:status(403):send("not allowed reqeust") 
    else
    	next() -- 满足以上条件，那么继续匹配下一个路由
    end
end)
```



### app:erroruse(path, middleware)

- app:erroruse用于加载一个错误处理插件(middleware)
- 参数说明
	- path, 插件作用的路径，可以为空，也就是说app:erroruse可以只有一个`middleware`参数，这时插件作用在所有path上
	- middleware，插件，格式为`function(err, req, res, next) end`, 注意与`use` api不同的是这个function有4个参数.

- 示例1：

该实例加载了一个作用在所有路径上的插件，也就是说只要有地方发生了错误，并且没有显式地调用`response`对象的`输出`方法，则会路由到这个错误插件进行处理。

```lua
-- 统一错误处理插件
app:erroruse(function(err, req, res, next)
    -- err是错误对象，直接将err打到response，生产环境请勿这样做
    res:status(500):send(err)
end)
```

	
- 示例2：

该实例加载了一个作用在前缀为"/user/"路径上的错误插件，也就是以"/user/"开始的请求处理过程中如果发生了错误就会被路由到这里做处理.

```lua
app:erroruse("/user", function(err, req, res, next)
    -- ...
end)
```


### app:run()

启动lor项目，开始接受请求并处理。



### app:get(path, fn)

- 目前lor支持的HTTP方法有get、post、head、options、put、patch、delete、trace，这些方法都可以用类似于app:get(path, fn)的形式调用。
- 参数说明
    - path，有两种格式，
        - 纯字符串形式，如"/user/1","/foo/bar"
        - 含有变量的uri， 如"/user/:id"，那么"/user/123"解析后将生成一个变量req.params.id，值为123
    - fn，对应这个uri的请求处理匿名函数，格式为function(req, res, next) end， 也可将它理解为一个通用的lor插件

- 示例:

```lua
app:get("/index", function(req, res, next)
    res:send("hello world!")
end)
```





## request对象

我们注意到有大量的地方出现了形如`function(req, res, next) end`的函数，这个函数其实就是lor框架的核心机制，也就是lor的常规插件。  
其中的`req`指的就是request对象，它包装了OpenResty收到的HTTP请求参数，并附带了一些方法来完成session、cookie数据交互，路由处理等其他后续操作。

req的常用属性和方法介绍如下：

### req.path

请求的uri，一般用作框架内部使用，如处理路由，解析参数，重定向请求等等，若用户不清楚修改该值会有什么影响，切勿随意更改此值。

### req.query

这是一个table，指的是url解析后的query string，比如"/find/user?id=1&name=sumory&year=2016"被解析后会生成对象req.query,它的值为：

```lua
{
    id = "1",
    name = "sumory",
    year = "2016"
}
```

### req.params

这是一个table，指的是url解析后的path variable，比如声明了以下路由处理方法

```lua
app:get("/query/:id/book/:name", function(req, res, next)
    local params = req.params
end)
```

那么访问"/query/123/book/abc"这个uri时得到的req.params值为:

```lua
{
    id = "123",
    name = "abc"
}
```

### req.body

这是一个table，指的是form表单提交上来的数据。


### req:is_found()

用于判断uri是否被路由到，如果这个方法返回值最终为false，说明`404`了。






## response对象

reponse对象指的是`function(req, res, next) end`函数中的res，它包装了OpenResty处理HTTP响应的一些API，并附带了一些方法来完成诸如模板渲染、重定向、json返回、session/cookie处理等其他后续操作。

res的常用属性和方法介绍如下：

### res:render(view, data)

- 渲染html页面，响应头Content-Type值为text/html; charset=UTF-8
- 参数说明
    - view，模板文件路径，比如app:conf("views", "./app/views")设置了模板路径为./app/views，那么想使用模板文件./app/views/user/index.html时，这个值应为"user/index"
    - data，类型为table，值得是模板文件渲染时需要的数据

### res:html(content)

- 返回内容为"content"的html，响应头Content-Type值为text/html; charset=UTF-8
- 参数说明：content应为字符串类型

### res:json(data)

- 返回内容为json格式，响应头Content-Type值为application/json; charset=utf-8
- 参数说明：data格式因为lua table

### res:send(text)

- 返回内容为text，响应头Content-Type值为text/plain; charset=UTF-8
- 参数说明：text应为字符串、数字或是array等类型

### res:set_header(key, value)

- 设置响应头，即调用`ngx.header[key] = value`

### res.cookie.set(...)

- 设置cookie，底层使用lua-resty-cookie库，请自行查看该库参数格式
- 参数说明，两种使用方式:
    - res.cookie.set({key = "c2", value = "c2_value"})
    - res.cookie.set("c1", "c1_value")

### res:redirect(url)

- 重定向，即调用`ngx.redirect(url)`




## next函数

`function(req, res, next) end`函数中的形参next是lor pipeline式路由能顺利进行的关键，它有两种调用方式：

- 不带参数，直接调用next()，则会将请求传给下一个调用者，继续后面的处理
- 带参数，如next("something")，这时会跳过之后的调用，直接去寻找能匹配的"错误处理插件"







