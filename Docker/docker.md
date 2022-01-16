- [Docker](#docker)
  - [一、Linux安装docker](#一linux安装docker)
  - [二、docker的基本增删改查相关镜像](#二docker的基本增删改查相关镜像)
  - [四、自定义镜像](#四自定义镜像)
    - [4.1 commit构建镜像](#41-commit构建镜像)
    - [4.2 Dockerfile构建镜像](#42-dockerfile构建镜像)

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


## 二、docker的基本增删改查相关镜像

- 查看本地镜像：`docker images`
- 搜索镜像：`docker search centos`
- 搜索镜像并过滤是官方的： `docker search --filter "is-official=true" centos`
- 搜索镜像并过滤大于多少颗星星的：`docker search --filter stars=10 centos`
- 下载centos7镜像：`docker pull centos:7`
- 修改本地镜像名字（小写）：`docker tag centos:7 mycentos:1`
- 本地镜像的删除：`docker rmi centos:7`


## 四、自定义镜像

- docker目前镜像的制作有俩种方法：
  - 基于Docker Commit制作镜像
  - 基于dockerfile制作镜像，Dockerfile方式为主流的制作镜像方式

### 4.1 commit构建镜像

以contos:7 镜像为例：

- 启动并进入容器：`docker run -it centos:7 /bin/bash`
- 在/home 路径下创建test文件夹：`mkdir /home/test`
- 安装ifconfig命令：`yum -y install net-tools`
- 构建镜像：
  - `docker commit -a "XD" -m "mkdir /home/test" [容器编号] mcentos:7`
  - -a：标注作者
  - -m：说明注释


### 4.2 Dockerfile构建镜像

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