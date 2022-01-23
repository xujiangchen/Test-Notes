- [Docker](#docker)
  - [一、Linux安装docker](#一linux安装docker)
  - [二、docker的基本操作](#二docker的基本操作)
    - [2.1 对镜像的基础增删改查](#21-对镜像的基础增删改查)
    - [2.2 容器的构建的基本操作](#22-容器的构建的基本操作)
    - [2.3 容器的文件复制与挂载](#23-容器的文件复制与挂载)
  - [三、自定义镜像](#三自定义镜像)
    - [3.1 commit构建镜像](#31-commit构建镜像)
    - [3.2 Dockerfile构建镜像](#32-dockerfile构建镜像)
  - [四、自定义镜像实战](#四自定义镜像实战)
    - [4.1 构建JAVA网站镜像](#41-构建java网站镜像)
    - [4.2 构建nginx镜像](#42-构建nginx镜像)
  - [五、网络模式](#五网络模式)
    - [5.1 桥接模式](#51-桥接模式)
    - [5.2 host模式](#52-host模式)
    - [5.3 none模式](#53-none模式)
    - [5.4 容器间基于Link实现单向通信](#54-容器间基于link实现单向通信)
    - [5.5 利用brige网桥实现双向通信](#55-利用brige网桥实现双向通信)
    - [5.6 docker容器的特权模式](#56-docker容器的特权模式)
  - [六、Compose操作容器](#六compose操作容器)
    - [6.1 安装compose](#61-安装compose)
    - [6.2 Compose快速上手](#62-compose快速上手)
      - [6.2.1 一个最简单的docker-compose.yml](#621-一个最简单的docker-composeyml)
      - [6.2.2 compose操作容器](#622-compose操作容器)
    - [6.3 Docker-Compose常用技能](#63-docker-compose常用技能)
  - [七、镜像仓库](#七镜像仓库)

# Docker

## 一、Linux安装docker

- 安装环境：Centos 7
- 安装条件：docker官方要求至少3.8以上，建议3.10以上

**安装步骤：**

- 下载阿里云docker社区版 yum源
```
cd /etc/yum.repos.d/

yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
- 查看docker安装包：`yum list | grep docker`
- 安装Docker Ce 社区版本：`yum install -y docker-ce.x86_64`
- 设置开机启动：`systemctl enable docker`
- 更新xfsprogs：`yum -y update xfsprogs`
- 启动docker：`systemctl start docker`
- 查看版本：`docker version`
- 查看详细信息：`docker info`

配置阿里云镜像加速：

- 阿里云镜像加速器配置地址：https://cr.console.aliyun.com/cn-shenzhen/instances/mirrors
  
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://[网页中具体编号].mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```


## 二、docker的基本操作

### 2.1 对镜像的基础增删改查

- 查看本地镜像：`docker images`
- 搜索镜像：`docker search centos`
- 搜索镜像并过滤是官方的： `docker search --filter "is-official=true" centos`
- 搜索镜像并过滤大于多少颗星星的：`docker search --filter stars=10 centos`
- 下载centos7镜像：`docker pull centos:7`
- 修改本地镜像名字（小写）：`docker tag centos:7 mycentos:1`
- 本地镜像的删除：`docker rmi centos:7`

### 2.2 容器的构建的基本操作

- 构建容器：`docker run -itd --name=mycentos centos:7`
- -i ：表示以交互模式运行容器（让容器的标准输入保持打开）
- -d：表示后台运行容器，并返回容器ID
- -t：为容器重新分配一个伪输入终端
- --name：为容器指定名称
- 查看本地所有的容器(运行中的和停止的)：`docker ps -a`
- 查看本地正在运行的容器：`docker ps`
- 停止容器：`docker stop CONTAINER_ID / CONTAINER_NAME`
- 一次性停止所有容器：`docker stop $(docker ps -a -q)`
- 启动容器：`docker start CONTAINER_ID / CONTAINER_NAME`
- 重启容器：`docker restart CONTAINER_ID / CONTAINER_NAME`
- 删除容器：`docker rm CONTAINER_ID / CONTAINER_NAME`
- 强制删除容器：`docker rmi -f CONTAINER_ID / CONTAINER_NAME`
- 查看容器详细信息：`docker inspect CONTAINER_ID / CONTAINER_NAME`
- 进入容器：docker `exec -it 0ad5d7b2c3a4 /bin/bash`

### 2.3 容器的文件复制与挂载

1、从宿主机复制到容器：docker cp 宿主机本地路径 容器名字/ID：容器路径
- `docker cp /root/123.txt mycentos:/home/`

2、从容器复制到宿主机：docker cp 容器名字/ID：容器路径 宿主机本地路径
- `docker cp mycentos:/home/456.txt /root`

3、宿主机文件夹挂载到容器里：docker run -itd -v 宿主机路径:容器路径 镜像ID
- `docker run -itd -v /root/xdclass/:/home centos:7`


## 三、自定义镜像

- docker目前镜像的制作有俩种方法：
  - 基于Docker Commit制作镜像
  - 基于dockerfile制作镜像，Dockerfile方式为主流的制作镜像方式

### 3.1 commit构建镜像

以contos:7 镜像为例：

- 启动并进入容器：`docker run -it centos:7 /bin/bash`
- 在/home 路径下创建test文件夹：`mkdir /home/test`
- 安装ifconfig命令：`yum -y install net-tools`
- 构建镜像：
  - `docker commit -a "XD" -m "mkdir /home/test" [容器编号] mcentos:7`
  - -a：标注作者
  - -m：说明注释


### 3.2 Dockerfile构建镜像

- 一个简单的Dockerfile文件
```
# This is dockerfile
FROM centos:7
MAINTAINER XU 519260804@Qqq.com
RUN echo "正在构建镜像"
WORKDIR /home/work
COPY test.txt /home/work
RUN yum install -y net-tools            
```
- 构建：`docker build -t mycentos:v2 .`

**dockerfile的基础指令：**
```
- FROM
  - 基于哪个镜像

- MAINTAINER
  - 注明作者

- COPY
  - 复制文件进入镜像（只能用相对路径，不能用绝对路径）

- ADD
  - 复制文件进入镜像（假如文件是.tar.gz文件会解压）

- WORKDIR：
  - 指定工作目录，假如路径不存在会创建路径（一进入系统的路径）

- ENV
  - 设置环境变量

- EXPOSE
  - 暴露端口，在构建镜像的时候执行，作用于镜像层面

- RUN
  - 执行命令，在构建镜像的时候执行，作用于镜像层面

- ENTRYPOINT
  - 执行命令，在容器启动的时候执行，作用于容器层，dockerfile里有多条时只允许执行最后一条

- CMD
  - 执行命令
  - 在容器启动的时候执行，作用于容器层，dockerfile里有多条时只允许执行最后一条
  - 容器启动后执行默认的命令或者参数，允许被修改

- 命令格式
  - shell命令格式：RUN yum install -y net-tools
  - exec命令格式：RUN [ "yum","install" ,"-y" ,"net-tools"]
```

## 四、自定义镜像实战

### 4.1 构建JAVA网站镜像
```
FROM centos:7
MAINTAINER XU 519260804@Qqq.com
RUN echo "正在构建镜像"
ADD jdk-8u211-linux-x64.tar.gz /usr/local
RUN mv /usr/local/jdk1.8.0_211 /usr/local/jdk
ENV JAVA_HOME=/usr/local/jdk
ENV JRE_HOME=$JAVA_HOME/jre
ENV CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
ENV PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
ADD apache-tomcat-8.5.35.tar.gz /usr/local
RUN mv /usr/local/apache-tomcat-8.5.35 /usr/local/tomcat
EXPOSE 8080
ENTRYPOINT ["/usr/local/tomcat/bin/catalina.sh","run"]
```

### 4.2 构建nginx镜像
- dockerfile
```
FROM centos:7
ADD nginx-1.16.0.tar.gz /usr/local
COPY nginx_install.sh /usr/local
RUN sh /usr/local/nginx_install.sh
EXPOSE 80
```

- 安装nginx的shell脚本
```
#!/bin/bash
yum install -y gcc gcc-c++ make pcre pcre-devel zlib zlib-devel
cd /usr/local/nginx-1.16.0
./configure --prefix=/usr/local/nginx && make && make install
```

- Nginx镜像启动注意
  - 在容器里nginx是以daemon方式启动，退出容器时，nginx程序也会随着停止：
  - 使用前台方式永久运行：/usr/local/nginx/sbin/nginx -g "daemon off;"
  - 启动镜像的命令 `docker run -itd -p 80:80 mycentos:nginx /usr/local/nginx/sbin/nginx -g "daemon off;"`


## 五、网络模式

启动参数为: `--net=[模式]`， 默认为桥接

### 5.1 桥接模式

桥接模式是docker 的默认网络设置，当Docker服务启动时，会在主机上创建一个名为docker0的虚拟网桥，并选择一个和宿主机不同的IP地址和子网分配给docker0网桥

每创建一个容器，容器都会桥接到docker0上面，也就是说在容器中执行命令`route -n`显示的网关为docker0

![bridge](https://github.com/xujiangchen/Test-Notes/blob/main/Docker/images/bridge.png)

**注意**：当使用桥接模式的时候，外部主机是没有办法进行访问容器内部的，只能在启动容器的时候将端口映射出去。宿主机可以访问容器的是因为有docker0这个网卡。

### 5.2 host模式

该模式下容器是不会拥有自己的ip地址，而是使用宿主机的ip地址和端口。

![host](https://github.com/xujiangchen/Test-Notes/blob/main/Docker/images/host.png)

优点：
- 网络性能比桥接会好很多

缺点：
- 网络的隔离性会非常的差，例如：宿主机有Nginx，容器1有Nginx，容器2也有Nginx，它们都会去抢占80端口。

### 5.3 none模式
- none模式：关闭模式
- 无法连外网

### 5.4 容器间基于Link实现单向通信

Tomcat容器需要与Mysql容器通信，Tomcat需要向Mysql发送网络包读取数据，一般情况Mysql不需要主动向Tomcat发起请求，这种情况就是单项通信。

一般情况下采用docker inspect 容器id查看IPAddress然后两个容器间互相Ping IP地址是可以Ping通的，因为启动采用是默认的桥接方式。

但这种直接调用IP地址的情况存在局限性，比如：有一天一个容器死掉了，需要重新run一次镜像起一个新的容器，Docer可能会重新为其中一个容器分配IP地址，那么需要到所有对应的容器中修改配置文件中这个地址，在容器量很大的情况下，这就造成维护困难。

**启动mysql容器**：`docker run --name databaseContain -e MYSQL_ROOT_PASSWORD=***** -d mysql:5.7`

**启动Tomcat**：`docker run -itd --name tomcat1 --link databaseContain tomcat:latest`

不管容器如何变化，Link链接只认容器名，新建的容器名只要修改容器名为Link设置的容器名，就依然可以保持通信。Link的原理是对当前容器的Host文件做了一个解析。

### 5.5 利用brige网桥实现双向通信
- 创建一个新的网桥：`docker network create -d bridge my_bridge`
- 启动第一个容器：`docker run -itd --name tomcat centos:7`
- 启动第二个容器：`docker run -itd --name redis centos:7`
- 把第一个容器加入网桥：`docker network connect my_bridge tomcat`
- 把第二个容器加入网桥：`docker network connect my_bridge redis`

### 5.6 docker容器的特权模式

- **方案1：**
```
容器创建运行时使用privileged=true参数
```

- **方案2：**

已存在容器开启特权privileged模式
  
```
#cd /var/lib/docker/containers/[Contrainer Id]/

#docker stop [Container ID[

#vim hostconfig.json

修改"Privileged":false  =>"Privileged":true

保存配置文件
docker start [Container ID]
```

## 六、Compose操作容器

### 6.1 安装compose
docker-compose：是一个用于定义和运行多容器 Docker 的应用程序工具，可以帮助我们可以轻松、高效的管
理容器
- compose的安装
  - 安装pip工具
    - `yum install -y epel-release`
    - `yum install -y python-pip`
    - `pip3 install --upgrade pip`
- 安装docker-compose
  - `pip3 install docker-compose==1.24.1`
- 查看docker-compose版本：
  - `docker-compose version`

### 6.2 Compose快速上手
#### 6.2.1 一个最简单的docker-compose.yml
启动一个名词为:redis的容器
```yml
# yml文件格式版本
version: '3'
# 容器
services:
  # 容器名称
  redis:
    # 对应的镜像
    image: centos:7
    # 让后台启动生效
    tty: true
```
#### 6.2.2 compose操作容器
**一定要进入配置文件目录**

- 后台启动容器：`docker-compose up -d`
- 查看容器运行情况：`docker-compose ps`
- 停止并删除容器：`docker-compose down`
- 停止并删除容器并删除volume：`docker-compose down --volumes`
- 停止启动容器：`docker-compose stop；docker-compose start`
- docker-compose exec的使用：`docker-compose exec redis bash`

### 6.3 Docker-Compose常用技能

- docker-compose.yml的三大部分：version，services，networks，最关键是services和networks两个部分
- compose设置网络模式
- compose使用端口映射
- compose设置文件共享
- compose管理多个容器
```yml
# yml文件格式版本
version: '3'
  # 容器
  services:
    # 容器名称
    nginx:
      # 对应的镜像
      image: mycentos:nginx
      # 让后台启动生效
      tty: true
      # 设置网络模式（host和post不能同时使用）
      network_mode: "host"
      # 文件共享（多个，宿主机路径:容器文件路径）
      volumes:
      - /home:/usr/local/nginx/html
      - /var/logs/nginx/logs:/usr/local/nginx/logs
      # 启动nginx命令
      command: /usr/local/nginx/sbin/nginx -g "daemon off;"

      # 容器名称
    redis:
      # 对应的镜像
      image: centos:7
      # 让后台启动生效
      tty: true
      # 端口映射 本地宿主机的端口：容器端口
      post:
      - "6380:6379"
```

## 七、镜像仓库