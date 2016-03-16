---
version: v0.1.0
category: Tutorial
title: 'hello world'
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/tutorial/hello.md'
---

让我们来看一下最简单的lor程序如何写。


```lua
local lor = require("lor.index")
local app = lor()

app:get("/", function(req, res, next)
    res:send("hello world!")
end)

app:run() -- 启动lor

```

就是如此简单！