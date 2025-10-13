# 1. Docker解决的问题
一次镜像，处处使用，从搬家到搬楼。
解决了运行环境和配置问题的软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术。

- 把自己自定义的容器build成镜像发布push 在dockerhub上，服务器pull并start即可

## 1.1 统一标准
- 应用构建
	- 不管是java c++ javaScript都可以docker buiild ... 成为一个**镜像**
	- 就像windows系统中把打包成可执行文件.exe
	- ==所有的软件包，所有的应用都称为一个**镜像**==
- 应用分享
	- 所有的软件的镜像都放到一个指定的地方 docker hub
	- 类似安卓的应用市场，提供给所有的人安装
- 应用运行
	- 统一标准的**镜像**  docker run 启动应用
	- 类似windows中的可执行文件（双击）

## 1.2 容器化技术
1.  基础镜像MB级别： 因为他是基于主机的操作系统
2. 创建简单：docker build
3. 隔离性强：像是每个隔离的沙箱，每个app（容器）都是一个最小完整运行环境
4. 启动速度秒级：根据镜像启动的
5. 移植与分享方便
![[Pasted image 20220624110924.png]]

## 1.3 资源隔离
- cpu、memory资源隔离与限制
- 访问设备隔离与限制
- 网络隔离与限制
- 用户、用户组隔离限制
- ......


# 2. 架构
## 2.1 基本概念
Docker 包括三个基本概念:

-   **镜像（Image）**：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
-   **容器（Container）**：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，==镜像是静态的定义，容器是镜像运行时的实体。==容器可以被创建、启动、停止、删除、暂停等。
-   **仓库（Repository）**：仓库可看成一个代码控制中心，用来保存镜像。

![[Pasted image 20220624111928.png]]
-   Docker_Host：
	-   安装Docker的主机
-   Docker Daemon：
	-   运行在Docker主机上的Docker后台进程
-   Client：
	-   操作Docker主机的客户端（命令行、UI等）
-   Registry：
	-   镜像仓库
	-   Docker Hub
-   Images：
	-   镜像，带环境打包好的程序，可以直接启动运行
-   Containers：
	-   容器，由镜像启动起来正在运行中的程序

交互逻辑
	装好**Docker**，然后去 **软件市场** 寻找**镜像**，下载并运行，查看**容器**状态日志等排错


# 3. 基础实战
## 3.1 找镜像
> 去[docker hub](http://hub.docker.com)，找到nginx镜像

下载镜像
- `docker pull nginx`  ：下载最新的nginx
- `docker pull nginx:1.20.1`：下载指定版本(标签)的nginx镜像

删除镜像
- `docker rmi nginx`：删除最新的nginx
- `docker rmi nginx:1.20.1`：删除指定版本的nginx镜像

查看本地下载的所有镜像
- `docker images`

## 3.2 启动容器
启动容器
- `docker run [OPTIONS] IMAGE [COMMAND][ARG...]`：镜像启动运行
- `docker run --name=mynginx -d nginx`：让程序在后台一直进行
- `docker run --name=mynginx -d --restart=always nginx`：让应用重新启动的时候开机自启

给容器更新配置参数
- `docker update 容器id/名字 --restart=always`：将该容器设置成开机自启

停止容器
- `docker stop 容器id/名字`

再次启动
- `docker start 容器id/名字`

删除已经启动的容器
- `docker rm [container id/names]`：将停止的容器删除
- `docker rm -f [container id/names]` ：将已启动的容器强行删除

查看容器
- `docker ps`：查看正在运行的容器
- `docker ps -a`：查看所有的容器（包括停止的）





> `docker run --name=mynginx -d --restart=always -p 88:80 nginx`
> 该命令的意思是让主机的88端口映射到80端口
> `0.0.0.0:3000->3000/tcp`
> 任意主机的3000端口信息都转到这个容器的3000端口

![[Pasted image 20220627195905.png]]
![[Pasted image 20220627200117.png]]


## 3.3 修改容器内容
> 修改默认的index.html页面

### 1、进容器内部修改
```bash
# 进入容器内部的系统，修改容器内容
docker exec -it 容器id  /bin/bash

# 退出容器内部系统
exit
```


### 2、挂载数据到外部修改

用不着进入到容器内部再修改容器内部的信息
直接映射到本机中的/data/html中进行修改
```bash
docker run --name=mynginx -d --restart=always -p 88:80 \
-v /data/html:/usr/share/nginx/html nginx
# -v 是挂载 主机/linux
```

修改页面只需要去 主机的 /data/html
挂载要注意 ==主机上的文件必须存在==才可以挂载到容器中去


![[Pasted image 20220628174240.png]]


## 3.4 提交改变
将自己修改好的镜像提交
```bash
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
# -a 作者名 / -m 提交信息 /  镜像id  /  保存的镜像名字:版本号
docker commit -a "leifengyang"  -m "首页变化" 341d81f7504f guignginx:v1.0
```

### 1、镜像传输
```bash
# 将镜像保存成压缩包
docker save -o abc.tar guignginx:v1.0

# 将压缩包传输给同一组的另外一台主机
# 在打包好的主机中将文件传输到另外一台主机上的/root/目录下
scp abc.tar root@139.198.186.134:/root/

# 别的机器加载这个镜像
docker load -i abc.tar

# 打开该镜像
docker run --name=mynginx -d -p 80:80 guignginx:v1.0

# 离线安装
```


### 2、推送远程仓库
> 推送镜像到docker hub；应用市场
```bash
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
```

```bash
# 把旧镜像的名字，改成仓库要求的新版名字
docker tag guignginx:v1.0 xan/guignginx:v1.0

# 登录到docker hub
docker login       


docker logout（推送完成镜像后退出）

# 推送
docker push xan/guignginx:v1.0


# 别的机器下载
docker pull xan/guignginx:v1.0

# 查看是否下载好镜像
docker images

# 启动容器
docker run -d -p 80:80 xan/guignginx:v1.0

# 查看启动的容器
docker ps
```


## 3.5 补充
```bash
docker logs 容器名/id   排错

docker exec -it 容器id /bin/bash


# docker 经常修改nginx配置文件
docker run -d -p 80:80 \
-v /data/html:/usr/share/nginx/html:ro \
-v /data/conf/nginx.conf:/etc/nginx/nginx.conf \
--name mynginx-02 \
nginx


#把容器指定位置的东西复制出来 
docker cp 5eff66eec7e1:/etc/nginx/nginx.conf  /data/conf/nginx.conf
#把外面的内容复制到容器里面
```



# 4.docker进阶
## 4.1 部署中间件
部署一个Redis+应用，尝试应用操作Redis产生数据

代码中的 \ 是多行用 \ 分隔
```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

#redis使用自定义配置文件启动

# -v 是挂载将主机的/data/redis/redis.conf挂载到容器中的/etc/redis/redis.conf下
#    这里挂载了数据data和redis配置文件
# -d 给容器起名
# -p 端口映射
docker run -v /data/redis/redis.conf:/etc/redis/redis.conf \
-v /data/redis/data:/data \
-d --name myredis \
-p 6379:6379 \
redis:latest  redis-server /etc/redis/redis.conf
# 启动redis最新版镜像 redis-server启动指定的是容器中的配置文件
```
![[Pasted image 20220628180653.png]]