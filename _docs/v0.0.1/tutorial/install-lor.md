---
version: v0.0.1
category: Tutorial
title: 'Install lor'
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/tutorial/install-lor.md'
---

- clone the repository 

```bash
git clone https://github.com/sumory/lor
```

- install with script `install.sh`

```bash
#set path to keep lor package. if not, /tmp/lua_framework will be used
sh install.sh /opt/lua/lor 
```

- check

	- the `lor` cli has been installed in /usr/local/bin. use `which lor` to check:

		```bash
		$ which lor
		/usr/local/bin/lor
		```

	- the `lor package` has been installed in /opt/lua/lor. use `ll /opt/lua/lor` to check:

		```bash
		$ ll /opt/lua/lor
		total 48
		drwxr-xr-x  12 root  wheel   408B  1 16 20:28 .
		drwxr-xr-x   3 root  wheel   102B  1 16 20:28 ..
		-rw-r--r--   1 root  wheel     0B  1 16 16:00 CHANGELOG.md
		-rw-r--r--   1 root  wheel   1.0K  1 16 16:06 LICENSE
		-rw-r--r--   1 root  wheel     0B  1 14 21:09 Makefile
		-rw-r--r--   1 root  wheel    52B  1 15 23:50 README.md
		-rw-r--r--   1 root  wheel   314B  1 16 20:27 install.md
		-rw-r--r--   1 root  wheel   1.0K  1 16 20:23 install.sh
		drwxr-xr-x   8 root  wheel   272B  1 15 21:38 lor
		```


After all the above, `lor` is successfully installed.