---
version: v0.1.0
category: Tutorial
title: 如何使用html模板
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/tutorial/view-use.md'
---

lor框架默认不开启模板功能，lor内部是通过lua-resty-template这个库来支持模板渲染的，开启方式如下。


```lua
local lor = require("lor.index")
local app = lor()

-- 模板配置
app:conf("view enable", true) -- 开启模板
app:conf("view engine", "tmpl") -- 模板引擎，当前lor只支持lua-resty-template，所以这个值暂时固定为"tmpl"
app:conf("view ext", "html") -- 模板文件后缀，可自定义
app:conf("views", "./app/views") -- 模板文件所在目录，可自定义，可为相对路径也可为绝对路径


```
