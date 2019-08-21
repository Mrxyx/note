# Docker端口映射
容器中可以运行一些网络应用，要让外部也可以访问这些应用，可以通过 -P 或 -p 参数来指定端口映射。

# 实现
* -p指定要映射的端口，一个指定端口上只可以绑定一个容器
* -P将容器内部开放的网络端口随机映射到宿主机的一个端口上（当使用 -P 标记时，Docker 会随机映射一个 49000~49900 的端口到内部容器开放的网络端口。）

# 格式
```
ip:hostport:containerport #指定ip、指定宿主机port、指定容器port    　　
ip::containerport #指定ip、未指定宿主机port（随机）、指定容器port  　　  
hostport:containerport #未指定ip、指定宿主机port、指定容器port　
```

# 映射的几种方法

## 一、将容器暴露的所有端口，都随机映射到宿主机上。
```
docker run -P -it ubuntu /bin/bash 
```

## 二、将容器指定端口随机映射到宿主机一个端口上。
```
docker run -P 80 -it ubuntu /bin/bash
```

## 三、将容器指定端口指定映射到宿主机的一个端口上。
```
docker run -p 8000:80 -it ubuntu /bin/bash
```
 
## 四、将容器ip和端口，随机映射到宿主机上。
```
docker run -P 192.168.0.100::80 -it ubuntu /bin/bash
```
## 五、将容器ip和端口，指定映射到宿主机上。
```
docker run -p 192.168.0.100:8000:80 -it ubuntu /bin/bash
```

## 三、将容器指定端口指定映射到宿主机的一个端口上。
```
docker run -p 8000:80 -it ubuntu /bin/bash
```

# 查看映射端口配置
```
docker port container_ID #容器ID
#结果输出
80/tcp -> 0.0.0.0:800
```

# Docker 错误 “port is already allocated” 解决方法
报错:“bind for 0.0.0.0：80 failed:port is already allocated”  

原因:容器占用的port没有完全释放。查看进程发现，虽然相关的容器并没有在运行，而 docker-proxy 却依然绑定着端口。可能之前rm docker容器时未完全删除就意外中止导致的  

解决办法：
1. 停止 doker 进程
2. 删除所有容器
3. 删除 local-kv.db 这个文件
4. 再启动 docker 就可以了。

```
$ sudo service docker stop
$ docker rm $(docker ps -aq)
$ sudo rm /var/lib/docker/network/files/local-kv.db
$ sudo service docker start
```