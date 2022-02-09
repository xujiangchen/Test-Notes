- [1、安装jenkins](#1安装jenkins)
- [2、jenkins的job管理](#2jenkins的job管理)
  - [2.1 General](#21-general)
  - [2.2 源码管理](#22-源码管理)
  - [2.3 触发器](#23-触发器)
  - [2.4 构建环境](#24-构建环境)
  - [2.5 构建](#25-构建)
  - [2.6 构建后操作](#26-构建后操作)
- [3、节点管理](#3节点管理)
- [4、权限控制](#4权限控制)
  - [4.1 注册的权限](#41-注册的权限)
  - [4.2 用户权限](#42-用户权限)
- [5、常用插件](#5常用插件)
- [6、邮件报警](#6邮件报警)
- [7、父子job和矩阵job](#7父子job和矩阵job)
  - [7.1 父子job](#71-父子job)


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

# 3、节点管理

- jenkins的任务可以分布在不同的节点上运行
- 节点上要配置java的运行环境，jdk版本要在1.5之上
- 节点支持Linux，Windows，Mac
- Jenkins运行的主机在逻辑上是master节点

![other](https://github.com/xujiangchen/Test-Notes/blob/main/Jenkins/images/slave.png)

# 4、权限控制

都在全局安全配置中进行配置

- Jenkins在初始化的时候会创建一个管理员用户
- 通过管理员再进行其他的账号创建
- 启用用户安全配置 Manager jenkins -> Configure Global Security

## 4.1 注册的权限
- 必须由管理员进行控制
- 用户可以自由注册，启用之后可以看到sign-up的入口
- 团队规模不大于10人，都不建议开启自由注册的功能，可以有效减少用户的管理成本
- 管理员对用户的管理 Manager jenkins -> Manager user

## 4.2 用户权限
- 由授权策略进行配置
- 原生的jenkins不提供用户组的管理，如果需要则要安装插件（Role-based Authorization Strategy）
- 原生中常用的是安全矩阵，和项目矩阵授权策略
  - 安全矩阵：是针对每个用户的
  - 项目矩阵授权策略：是以项目为维度的

# 5、常用插件

# 6、邮件报警
jenkins自带邮件报警功能，不需要安装额外的插件
- 系统管理 -> 系统设置 ：Jenkins Location 中找到`系统管理员邮件地址`输入框，输入邮箱
- 保存上述操作，再次点开系统设置，将页面拖至底部，出现`邮件通知`

![Email](https://github.com/xujiangchen/Test-Notes/blob/main/Jenkins/images/Email.png)

- 在每个项目的后面添加上异常发送报警邮件。这样配置就完成了

![EmailNotification](https://github.com/xujiangchen/Test-Notes/blob/main/Jenkins/images/EmailNotification.png)

# 7、父子job和矩阵job

## 7.1 父子job
项目之间总会有关联，如何让项目依次执行呢？这时我们就需要创建job之间的触发关系
常见的触发关系有三种：
1. 只有构建稳定时触发
2. 即使构建不稳定时也会触发
3. 即使构建失败时也会触发

再配置任务的时候可以让子job去关联父job，**当运行父job时子job也会被运行**

![fzjob](https://github.com/xujiangchen/Test-Notes/blob/main/Jenkins/images/fzjob.png)