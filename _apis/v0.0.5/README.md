---
version: v0.0.5
category: 'API'
title: 目录
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/apis/v0.0.5/README.md'
permalink: /apis/v0.0.5/index.html
---

请注意您正在查看的文档是正确的版本，版本号在url中有所体现，如果错了，这里有所有版本的文档，[请切换](/apis/).

# API



## lor

```
local lor = require("lor.index")
```

lor对象: lor框架暴露出来的最重要对象，理论上通过调用该对象的方法和属性即可使用绝大部分lor框架提供的功能。



### lor()

```
local lor = require("lor.index")
local app = lor()
```

该方法是框架暴露出来的最重要的方法，用于创建一个app对象，之后大部分api将通过`app`对象展开.  
如无特别说明，下文中出现的`app`均指通过`lor()`方法创建的application对象.


### lor:Router()

生成一个`group router`对象，`group router`对象指一个`路由组`，用来在业务上聚合一组相关的路由，它具有`get`、`post`、`delete`、`put`等等HTTP方法，这些API与下文介绍的`app`上的`get`、`post`、`delete`、`put`等方法使用方式一致。







## app对象


### app:use(path, middleware)

- app:use用于加载一个插件(middleware)
- 参数说明
	- path, 插件作用的路径，可以为空，也就是说app:use可以只有一个`middleware`参数，这时插件作用在所有path上
	- middleware，插件，格式为`function(req, res, next) end`, 对请求做预处理或者善后处理

- 示例1：

该实例加载了一个作用在所有路径上的插件，它的作用是在请求上注入了一个参数`inject_param`,这样后续匹配到的路由可以使用这个参数。

```
app:use(function(req, res, next)
    -- 注入一个参数
    req.params.inject_param = "from all path middleware" 
    next() -- 注意，如果不调用next()方法，请求到这里就截止了，不在匹配后面的路由
end)
```

	
- 示例2：

该实例类似于一个拦截器，app加载了一个作用在前缀为"/user/"路径上的插件。
插件的作用是判断传入的请求参数里是否含有已经被授权的token，没有则直接返回错误，如果有就放过，继续下个路由。

```
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

```
-- 统一错误处理插件
app:erroruse(function(err, req, res, next)
    -- err是错误对象，直接将err打到response，生产环境请勿这样做
    res:status(500):send(err)
end)
```

	
- 示例2：

该实例加载了一个作用在前缀为"/user/"路径上的错误插件，也就是以"/user/"开始的请求处理过程中如果发生了错误就会被路由到这里做处理.

```
app:erroruse("/user", function(err, req, res, next)
    -- ...
end)
```
