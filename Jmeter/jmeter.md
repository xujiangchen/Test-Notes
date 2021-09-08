- [jmeter](#jmeter)
  - [一、本地快速安装Jmeter4.x](#一本地快速安装jmeter4x)
  - [二、Jmeter目录](#二jmeter目录)
  - [三、JMeter核心组件](#三jmeter核心组件)
    - [3.1 线程组](#31-线程组)
    - [3.2 Sampler（采样器）](#32-sampler采样器)
    - [3.3 查看结果](#33-查看结果)
  - [四、断言](#四断言)


# jmeter

## 一、本地快速安装Jmeter4.x
-  1、需要安装JDK8。或者JDK9，JDK10
-  2、快速下载
   - windows： http://mirrors.tuna.tsinghua.edu.cn/apache//jmeter/binaries/apache-jmeter-4.0.zip
   - mac或者linux：http://mirrors.tuna.tsinghua.edu.cn/apache//jmeter/binaries/apache-jmeter-4.0.tgz
- 文档地址：http://jmeter.apache.org/usermanual/get-started.html

## 二、Jmeter目录

- bin:核心可执行文件，包含配置
  - jmeter.bat: windows启动文件
  - jmeter: mac或者linux启动文件
  - jmeter-server：mac或者Liunx分布式压测使用的启动文件
  - jmeter-server.bat：mac或者Liunx分布式压测使用的启动文件
  - jmeter.properties: 核心配置文件
- extras：插件拓展的包
- lib:核心的依赖包
  - ext:核心包
  - junit:单元测试包

## 三、JMeter核心组件

### 3.1 线程组

**添加-> threads -> 线程组（控制总体并发）**

- 线程数（Number of Threands）：虚拟用户数。一个虚拟用户占用一个进程或线程
- 准备时长（Ramp-Up Period(in seconds)）：全部线程启动的时长，比如100个线程，20秒，则表示20秒内100个线程都要启动完成，每秒启动5个线程
- 循环次数（Loop Count）：每个线程发送的次数，假如值为5，100个线程，则会发送500次请求，可以勾选永远循环。必须有值（默认为1）

### 3.2 Sampler（采样器）

**线程组->添加-> Sampler(采样器) -> Http （一个线程组下面可以增加多个Sampler）**

- 名称：采样器名称
- 注释：对这个采样器的描述
  
- web服务器：
  - 默认协议是http
  - 默认端口是80
  - 服务器名称或IP ：请求的目标服务器名称或IP地址

- 路径：服务器URL

- Use multipart/from-data for HTTP POST ：当发送POST请求时，使用Use multipart/from-data方法发送，默认不选中。

### 3.3 查看结果

**线程组->添加->监听器->察看结果树**

## 四、断言

**增加断言: 线程组 -> 添加 -> 断言 -> 响应断言**
- apply to(应用范围):
  - Main sample only: 仅当前父取样器 进行断言，一般一个请求，如果发一个请求会触发多个，则就有sub sample（比较少用）
- 要测试的响应字段：
  - 响应文本：即响应的数据，比如json等文本
	- 响应代码：http的响应状态码，比如200，302，404这些
	- 响应信息：http响应代码对应的响应信息，例如：OK, Found
	- Response Header: 响应头
- 模式匹配规则：
  - 包括：包含在里面就成功
  - 匹配：响应内容完全匹配，不区分大小写
  - equals：完全匹配，区分大小写