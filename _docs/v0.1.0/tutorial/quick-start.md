---
version: v0.1.0
category: Tutorial
title: '快速开始'
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/tutorial/quick-start.md'
---


lor提供了一个命令行工具`lord`来构建基于lor的项目骨架，此后开发者可以根据需要自行调整目录结构和代码.

### 命令行工具lord

在安装`lor`之后, 在终端输入`lord -h`:

```bash
$ lor -h
lor v0.1.0, a Lua web framework based on OpenResty.

Usage: lor COMMAND [OPTIONS]

Commands:
 new [name]             Create a new application
 start                  Starts the server
 stop                   Stops the server
 restart                Restart the server
 version                Show version of lor
 help                   Show help tips
```

### 使用lord创建项目

`lor new [project_name]`创建一个项目骨架:

```bash
lor new lor_demo
```

进入lor_demo查看，一个lor项目就被创建好了，它的结构如下：

```text
$ tree .
lord_demo
└── app
    ├── main.lua # lor程序主入口，也就是要被OprenResty加载的入口文件
    ├── router.lua # 路由器
    ├── routes # 各个具体的路由
    │   ├── test.lua
    │   └── user.lua
    └── views # html模板
        └── index.html
```


__注意__: 请先阅读以上示例的代码，在对`lor`熟悉之前，请不要随意删除文件或者修改代码位置，以免示例无法运行。一旦熟悉了如何使用lor的路由和插件，你可以任意调整目录结构和模块引用方式，lor对此几乎没有任何限制。


### 启动项目

进入刚才创建的项目目录lor_demo, 然后执行`lord start`来启动项目:

```bash
$ cd lor_demo
$ lor start
```

现在打开浏览器，访问[http://localhost:8888](http://localhost:8888)来查看你的第一个lor项目吧.
