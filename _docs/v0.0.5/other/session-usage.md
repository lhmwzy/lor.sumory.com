---
version: v0.0.5
category: Others
title: '使用session'
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/other/session-usage.md'
---

lor提供了session的一个[默认插件](https://github.com/sumory/lor/blob/master/lor/lib/middleware/session.lua)，只要在路由里加载就可以使用。

**注意**: 

- 在使用这个插件时，用户应该先了解[lua-resty-session](https://github.com/bungle/lua-resty-session)
- lor提供的默认session使用了cookie的存储方式，但真实项目中可能需要基于redis或是memcache等集中式的session管理，这时用户应自行创建一个lor插件来使用


使用lor提供的默认session方式如下:

```lua
local lor = require("lor.index")
local app = lor()

-- 加载session插件
local middleware_session = require("lor.lib.middleware.session")
app:use(middleware_session())

-- 模拟session的使用
-- 加载session插件后，`session`对象被注入到了`req`对象里

-- 在session里赋值
app:get("/session/set", function(req, res, next)
    local k = req.query.k
    local v = req.query.v
    if k then
        req.session.set(k,v)
        res:send("session saved: " .. k .. "->" .. v)
    else
        res:send("null session key")
    end
end)

-- 从session里取值
app:get("/session/get/:key", function(req, res, next)
    local k = req.params.key
    if not k then
        res:send("please input session key")
    else
        res:send("session data: " .. req.session.get(k))
    end
end)

-- 销毁session
app:get("/session/destroy", function(req, res, next)
    req.session.destroy()
end)


app:run()
```
