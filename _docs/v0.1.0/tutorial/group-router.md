---
version: v0.1.0
category: Tutorial
title: 使用组路由
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/tutorial/group-router.md'
---

#### 说明

Group Router即组路由，用于收纳一组相关的路由，比如同一业务对象的增删改查。


#### 使用方式

我们声明一个group router叫user_router，在这个router完成用户的增删改查，示例代码如下

```lua
local lor = require("lor.index")
local app = lor()

-- 声明一个group router
local user_router = lor:Router()

user_router:get("/:username/query", function(req, res, next)
 -- 查询用户
end)

user_router:put("/:username/create", function(req, res, next)
 -- 创建用户
end)


user_router:post("/:username/update", function(req, res, next)
 -- 修改用户信息
end)

user_router:delete("/:username/delete", function(req, res, next)
 -- 删除用户
end)

-- 以middleware的形式将该group router加入到路由中
app:use("/user", user_router())

app:run()
```

之后就可以通过类似于`/user/my_usernmae/query`, `/user/my_usernmae/create`, `/user/my_usernmae/update`, `/user/my_usernmae/delete`来访问该router对应的业务了。