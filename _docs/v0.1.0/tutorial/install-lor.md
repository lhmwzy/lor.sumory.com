---
version: v0.1.0
category: Tutorial
title: '安装lor'
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/tutorial/install-lor.md'
---

#### 安装说明

目前lor提供了手动安装的方式，即通过shell脚本将命令行工具`lord`和运行时包放到指定目录。

- `lord`命令应只作为初学者快速建立一个项目骨架时使用，生产环境建议大家使用各自的脚本或者其他方式来管理OpenResty程序
- `运行时包`指的是lor框架本身的代码，用户可以把lor框架理解为一个普通的第三方库，比如lua-resty-mysql、lua-resty-http等等

#### 安装步骤

- 获取源码

```bash
git clone https://github.com/sumory/lor
```

- 使用脚本`install.sh`安装

```bash
# 执行以下命令将lor的运行时包安装到默认目录/usr/local/lor，将lord命令安装到/usr/local/bin下
sh install.sh

# 或执行以下命令指定运行时包的安装路径，注意此时lord仍将安装到/usr/local/bin下
sh install.sh /opt/lua/ 
```

- 检查是否安装成功

	- 默认的，lor的命令行工具`lord`会被安装在/usr/local/bin下。使用命令`which lor`查看:

		```bash
		$ which lord
		/usr/local/bin/lord
		```

	- lor的运行时包安装在指定目录下，比如/opt/lua/lor。 使用命令`ll /opt/lua/lor`检查:

		```bash
		$ ll /opt/lua/lor
		total 32
		drwxr-xr-x  11 root  wheel   374B  1 25 14:25 .
		drwxrwxrwt   9 root  wheel   306B  1 25 16:09 ..
		-rw-r--r--   1 root  wheel   368B  1 25 14:25 CHANGELOG.md
		-rw-r--r--   1 root  wheel   1.0K  1 25 14:25 LICENSE
		-rw-r--r--   1 root  wheel     0B  1 25 14:25 Makefile
		-rw-r--r--   1 root  wheel   3.8K  1 25 14:25 README.md
		drwxr-xr-x   4 root  wheel   136B  1 25 14:25 bin
		-rw-r--r--   1 root  wheel   1.0K  1 25 14:25 install.sh
		drwxr-xr-x   4 root  wheel   136B  1 25 14:25 lor
		drwxr-xr-x   7 root  wheel   238B  1 25 14:25 resty
		drwxr-xr-x  14 root  wheel   476B  1 25 14:25 test
		```


至此，lor已成功安装到您的系统中，若有安装问题请查看[安装问题集锦](/docs/v0.1.0/faq/install/)