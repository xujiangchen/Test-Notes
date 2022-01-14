- [Docker](#docker)
  - [一、Linux安装docker](#一linux安装docker)
  - [二、docker的基本增删改查相关镜像](#二docker的基本增删改查相关镜像)

## Docker

### 一、Linux安装docker

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


### 二、docker的基本增删改查相关镜像

- 查看本地镜像：`docker images`
- 搜索镜像：`docker search centos`
- 搜索镜像并过滤是官方的： `docker search --filter "is-official=true" centos`
- 搜索镜像并过滤大于多少颗星星的：`docker search --filter stars=10 centos`
- 下载centos7镜像：`docker pull centos:7`
- 修改本地镜像名字（小写）：`docker tag centos:7 mycentos:1`
- 本地镜像的删除：`docker rmi centos:7`
