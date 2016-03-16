---
version: v0.1.0
category: 问题
title: 安装问题集锦
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/faq/install.md'
---

#### **安装结束后无法使用lord命令**

- 请检查要安装的路径是否有权限
	- 比如sh install.sh会把`lord`命令安装到/usr/local/bin，把lor的包安装到/usr/local/lor，请确认当前安装的用户是否对这两个目录有操作权限
- 特别注意安装时的输出日志，比如执行sh install.sh时正常输出日志如下：

	```sh
	$ sh install.sh
	start installing lor...
	use default PATH: /usr/local/lor
	install lor cli to /usr/local/bin/
	install lor package to /usr/local/lor
	lor framework installed.
	```
- lord命令使用到了`luajit`和`nginx`两个命令

	- 在命令行环境中要能直接执行`nginx`和`luajit`，比如`luajit -v`和`nginx -v`能正常输出。
	- 若不知道如何将luajit、nginx命令加入到环境变量中，请自行google、百度。

#### **我的lor到底安装在了哪里**

通过lord path命令查看

#### **是否可以不用安装lor，直接使用它来构建一个基于OpenResty的项目**

- 可以，lor只是一个框架，也是一个程序库，可以像其他`库`或`工具包`一样使用
- 使用方法如下
	- 新建一个空白文件夹作为项目目录，如mkdir /tmp/myproject 
	- 下载lor，git clone https://github.com/sumory/lor
	- 新建一个app文件夹，作为代码目录， 新建conf文件夹和nginx.conf作为配置文件，新建logs文件夹用于存放日志，当前目录结构如下：

		```
		myproject
		├── app
		├── conf
		│   └── nginx.conf
		├── logs
		└── lor
		    ├── ...
		```
	- 在app文件夹下新建一文件main.lua作为项目入口，代码如下：

		```lua
		  local lor = require("lor.index")
		local app = lor()

		app:get("/index", function(req, res, next)
		    res:send("Hello World!")
		end)

		app:run()
		```

	- 新建nginx配置文件nginx.conf，加入以下配置。 即在`lua_package_path`中指定lor目录`lor`和我们自己编写的代码所在目录`app`,并在`content_by_lua_file`阶段指定我们刚才编写的项目入口文件。

		```sh
		  # 注意这里只是为了演示的最简配置，实际项目中要根据项目需要自行配置
		pid logs/nginx.pid;
		worker_processes 4;
		events {
		    worker_connections 4096;
		}

		http {
		    lua_package_path "./app/?.lua;./lor/?.lua;;";
		    
		    server {
		        listen 8888;
		        
		        access_log logs/access.log combined buffer=16k;
		        error_log logs/error.log;

		        location / {
		            content_by_lua_file ./app/main.lua;
		        }
		    }
		}
		```

	- 至此，所有准备工作已经完成，目录结构如下

		```
		myproject
		├── app
		│   └── main.lua
		├── conf
		│   └── nginx.conf
		├── logs
		└── lor
		    ├── ...
		```

	- 使用以下命令启动本项目

		```
		nginx -c ./conf/nginx.conf -p `pwd`
		```

	启动后，访问http://localhost:8888/index, 就会出现我们熟悉的`Hello World!`
