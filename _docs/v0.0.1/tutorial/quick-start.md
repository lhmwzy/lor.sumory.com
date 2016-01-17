---
version: v0.0.1
category: Tutorial
title: 'Quick Start'
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/tutorial/quick-start.md'
---

# Quick Start

You can create a project scaffold with cli tool of `lor`. And then begin quickly based on this scaffold.

### CLI tool

After `lor` installed, just type `lor -h` or `lor help` in your terminal:

```bash
$ lor -h
lor v0.0.1, a Lua web framework based on OpenResty.

Usage: lor COMMAND [OPTIONS]

Commands:
 new [name]             Create a new application
 start                  Starts the server
 stop                   Stops the server
 restart                Restart the server
 version                Show version of lor
 help                   Show help tips

Options:
 --debug                Show some runtime details
```

### Create project with lor scaffold

Use `lor new [project_name]` to start a new project:

```bash
lor new lor_demo
```

Generally, a lor app is structured like this:

```text
$ tree .
.
├── application # application folder, shouldn't change its name.
│   ├── controllers # controllers' folder, shouldn't change its name.
│   │   ├── error.lua
│   │   └── index.lua
│   ├── library # some common libs
│   ├── main.lua # the entrance for this app, invoked by OpenResty
│   ├── models
│   │   ├── dao
│   │   │   └── table.lua
│   │   └── service
│   │       └── user.lua
│   ├── nginx # some lua configuration about nginx(OpenResty)
│   │   ├── init.lua
│   │   └── sh_dict.lua
│   └── views # views file folder, shouldn't change its name.
│       ├── error
│       │   └── common_err.html
│       └── index
│           └── index.html
└── config # all needed configurations for this app
    ├── application.lua # app config, check the content for detail
    ├── errors.lua
    └── nginx.lua # nginx(OpenResty) configuration, such as port/runtime files etc.
```


__Note__: Before you have been really familiar with `lor`, you shouldn't rename or delete some files generated. But, you may have no limit to adjust the project structure so long as you take some practice.


### Start the project

Enter the project folder(very important), and use `lor start` to run this project:

```bash
$ cd lor_demo
$ lor start
app in dev was succesfully started on port 8888
```

### Congratulations

Now, just open [http://localhost:8888](http://localhost:8888) to check your first `lor` project.
