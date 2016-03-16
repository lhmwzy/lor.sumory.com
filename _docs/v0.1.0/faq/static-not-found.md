---
version: v0.1.0
category: 问题
title: 静态文件无法加载
source_url: 'https://github.com/sumory/lor.sumory.com/blob/master/docs/faq/static-not-found.md'
---


#### 可能原因


- nginx.conf里没有配置mime.types，在http里加入即可，注意路径

	```

	  http {
    	include /opt/server/nginx/conf/mime.types;
    	...
    }
    ```

- 静态文件的location配置有问题，检查nginx相关配置