- [1、安装jenkins](#1安装jenkins)
- [2、jenkins的job管理](#2jenkins的job管理)
  - [2.1 General](#21-general)
  - [2.2 源码管理](#22-源码管理)
  - [2.3 触发器](#23-触发器)
  - [2.4 构建环境](#24-构建环境)
  - [2.5 构建](#25-构建)
  - [2.6 构建后操作](#26-构建后操作)


# 1、安装jenkins
使用docker进行Jenkins的部署

- 拉取Jenkins的镜像：`docker pull jenkins/jenkins:lts`(lts是稳定版本)
- 创建docker文件映射卷：`docker volume create jenkins`
- 使用docker命令启动jenkins：`docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins:/var/jenkins_home -e JAVA_OPTS=-Duser.timezone=Asia/shanghai jenkins/jenkins:lts`
- 获取jenkins：`/var/jenkins_home/secrets/initialAdminPassword`

# 2、jenkins的job管理

## 2.1 General
对项目的基本描述，并且可以按照自己的需要配置job的一些设置，例如：丢弃旧的构建就可以让我们的Jenkins将一些旧的job自动删除掉，以避免占用特别大的硬盘空间

![General](https://github.com/xujiangchen/Test-Notes/blob/main/Jenkins/images/General.png)

## 2.2 源码管理
对所需要构建的源码进行管理

![SourceCode](https://github.com/xujiangchen/Test-Notes/blob/main/Jenkins/images/SourceCode.png)

## 2.3 触发器
在什么情况下会进行构建
- 触发远程构建 (例如,使用脚本)：这里使用于自动化构建，拼接url后写入代码中可以实现在脚本或者工具执行构建
- Build periodically：定时执行构建任务，不管远程代码分支上的代码是否发生变化，都执行一次构建。
- Poll SCM：设置定时检查代码仓库是否有变更，有变更则构建

```
定时构建语法

第一个参数代表的是分钟 minute，取值 0~59；
第二个参数代表的是小时 hour，取值 0~23；
第三个参数代表的是天 day，取值 1~31；
第四个参数代表的是月 month，取值 1~12；
最后一个参数代表的是星期 week，取值 0~7，0 和 7 都是表示星期天。

每隔5分钟构建一次 => H/5 * * * *
每两小时构建一次  => H H/2 * * *
每天中午下班前定时构建一次 => 0 12 * * *
每天下午下班前定时构建一次 => 0 18 * * *
```

![BuildTrigger](https://github.com/xujiangchen/Test-Notes/blob/main/Jenkins/images/BuildTrigger.png)

## 2.4 构建环境
根据实际情况进行配置

## 2.5 构建
如何对项目进行构建，比如通过shell脚本，windows命令。

## 2.6 构建后操作
是否对文档进行归档，追踪

![other](https://github.com/xujiangchen/Test-Notes/blob/main/Jenkins/images/other.png)