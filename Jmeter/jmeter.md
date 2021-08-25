- [jmeter](#jmeter)
  - [一、本地快速安装Jmeter4.x](#一本地快速安装jmeter4x)
  - [二、Jmeter目录](#二jmeter目录)


# jmeter

## 一、本地快速安装Jmeter4.x
-  1、需要安装JDK8。或者JDK9，JDK10
-  2、快速下载
   - windows： http://mirrors.tuna.tsinghua.edu.cn/apache//jmeter/binaries/apache-jmeter-4.0.zip
   - mac或者linux：http://mirrors.tuna.tsinghua.edu.cn/apache//jmeter/binaries/apache-jmeter-4.0.tgz
- 文档地址：http://jmeter.apache.org/usermanual/get-started.html

## 二、Jmeter目录

- bin:核心可执行文件，包含配置
  - jmeter.bat: windows启动文件：
  - jmeter: mac或者linux启动文件：
  - jmeter-server：mac或者Liunx分布式压测使用的启动文件
  - jmeter-server.bat：mac或者Liunx分布式压测使用的启动文件
  - jmeter.properties: 核心配置文件
- extras：插件拓展的包
- lib:核心的依赖包
  - ext:核心包
  - junit:单元测试包