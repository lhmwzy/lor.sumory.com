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

	```
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

#### **是否可以不用安装lor，直接使用**