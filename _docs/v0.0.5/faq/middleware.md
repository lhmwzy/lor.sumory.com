---
version: v0.0.5
category: 问题
title: 编写和使用lor插件
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/faq/middleware.md'
---

插件是lor框架的核心概念之一，任何一个满足格式`function(req, res, next) end`的函数都可以作为一个常规lor插件使用。
同样的，任何一个满足格式`function(err, req, res, next) end`的函数都可以作为一个"错误处理插件"使用。

一般的，使用lor插件可以解决三类需求：

- 对req和res对象做一些处理
- 对路由进行控制
	- 如果调用了next()，则将继续写一个路由匹配
	- 如果调用了next("something")， 则会抛出一个错误，之后将会尝试匹配下一个”错误处理”插件
	- 没有调用next，则请求到这里结束（请注意要调用res的相关方法已结束该请求，不要忘记响应客户端请求）
- 对错误进行处理


### 常规插件编写和使用

常规插件一般要放在**业务路由**的前面加载，先来看下一个很简单的常规lor插件(simple\_middleware.lua):


```lua
local simple_middleware =  function(params)
    return function(req, res, next)
        -- 对req和res对象做一些处理
        if req.params then
        	if not req.params.foo then
        		req.params.foo = "bar"
        	end
        end
        next()
    end
end

return simple_middleware
```

如何使用这个插件？通过`app:use`这个API，有两种方式:

1）直接使用app:use(middleware)，这个插件会匹配所有路由

```lua
local lor = require("lor.index")
local app = lor()

 -- 引入刚才编写的插件
local simple_middleware = require("path.to.simple_middleware_file")
local simple_middleware_params = {
	-- 可能需要的一些初始化参数
	-- ...
}

app:use(simple_middleware(simple_middleware_params))

-- 其他逻辑，如业务路由

app:run()
```

2）使用app:use(path, middleware)，这个插件只匹配满足“path”的路由

```lua
app:use("some_path", simple_middleware(simple_middleware_params))
```




### 错误处理插件编写和使用



错误处理插件一般要放在**业务路由**的**后面**加载，先来看下一个错误处理插件实例(simple\_error_middleware.lua):


```lua
local simple_error_middleware =  function(params)
    return function(err, req, res, next)
        -- err对象是错误插件接到的对象，它一般有两个来源
        --   1. 通过next(err)函数传过来的错误
        --   2. lua的error()错误
        
        ngx.log(500, err) -- 记录错误日志

        next()
    end
end

return simple_error_middleware
```

如何使用这个插件？通过`app:erroruse`这个API，有两种方式:

1）直接使用app:erroruse(middleware)，这个插件会匹配所有发生的错误情况

```lua
local lor = require("lor.index")
local app = lor()

 -- 引入刚才编写的插件
local simple_error_middleware = require("path.to.simple_error_middleware_file")
local simple_error_middleware_params = {
	-- 可能需要的一些初始化参数
	-- ...
}


-- 其他逻辑，如业务路由

app:erroruse(simple_error_middleware(simple_error_middleware_params))

app:run()
```

2）使用app:erroruse(path, middleware)，这个插件只匹配满足“path”的路由发生的错误

```lua
app:erroruse("some_path", simple_error_middleware(simple_error_middleware_params))
```
