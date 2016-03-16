---
version: v0.1.0
category: Others
title: '使用cookie'
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/other/cookie-usage.md'
---

lor提供了cookie的一个[默认插件](https://github.com/sumory/lor/blob/master/lor/lib/middleware/cookie.lua)，只要在路由里加载就可以使用。

**注意**: 

- 在使用这个插件时，用户应该先了解[lua-resty-cookie](https://github.com/cloudflare/lua-resty-cookie)的API使用方式
- 真实项目中如有其它cookie需求，用户应自行创建一个lor插件来使用


使用lor提供的默认cookie方式如下:

```lua
local lor = require("lor.index")
local app = lor()

-- 加载cookie插件
local middleware_cookie = require("lor.lib.middleware.cookie")
app:use(middleware_cookie())

-- 模拟cookie的使用
-- 加载cookie插件后，`cookie`被注入到了`req`和`res`两个对象里

app:get("/cookie", function(req, res, next)
    -- 使用req.cookie来读取客户端传过来的cookie，req.cookie是一个table

    -- 使用res.cookie.set这个API来写cookie， 支持table参数和k/v参数
    res.cookie.set({key = "c2", value = "c2_value"})
    res.cookie.set("c1", "c1_value")
end)


app:run()
```
