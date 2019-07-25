# 错误类型

## 报error: RPC failed; HTTP 411 curl 22 The requested URL returned error: 411 Length Required错误  
---
解决办法  
找到.git文件  
找到config文件并打开  
在后面添加  
```
[http]  
postBuffer = 524288000  
```
## 若修改后仍然报错 error: RPC failed; HTTP 413 curl 22 The requested URL returned error: 413 Request Entity Too Large  

##### 因为上传大文件时，若gogos使用nginx反代的话 一般nginx会返回413错误。
----
解决办法  
    1. 打开nginx配置文件 nginx.conf, 路径一般是：/etc/nginx/nginx.conf。
	2. 在http{}段中加入 client_max_body_size 50m; 50m为允许最大上传的大小。
	3. 保存后重启nginx，问题解决。