- [jmeter](#jmeter)
  - [一、本地快速安装Jmeter4.x](#一本地快速安装jmeter4x)
  - [二、Jmeter目录](#二jmeter目录)
  - [三、JMeter核心组件](#三jmeter核心组件)
    - [3.1 线程组](#31-线程组)
    - [3.2 Sampler（采样器）](#32-sampler采样器)
    - [3.3 查看结果](#33-查看结果)
  - [四、断言](#四断言)
  - [五、压测结果聚合报告分析](#五压测结果聚合报告分析)
  - [六、Jmeter压测脚本JMX](#六jmeter压测脚本jmx)
    - [6.1 测试计划](#61-测试计划)
    - [6.2 线程组](#62-线程组)
    - [6.3 HTTP请求(Sampler)](#63-http请求sampler)
    - [6.4 查看结果数](#64-查看结果数)
    - [6.5 结果断言](#65-结果断言)
    - [6.6 聚合报告](#66-聚合报告)
  - [七、自定义变量和CSV可变参数](#七自定义变量和csv可变参数)
    - [7.1 自定义变量](#71-自定义变量)
    - [7.2 CSV可变参数](#72-csv可变参数)


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

![jmeter-1](https://github.com/xujiangchen/Test-Notes/blob/main/Jmeter/imgs/jmeter-1.png)

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

![assert](https://github.com/xujiangchen/Test-Notes/blob/main/Jmeter/imgs/assert.png)

每个sample下面可以加单独的结果树，然后同时加多个断言，最外层可以加个结果树进行汇总

## 五、压测结果聚合报告分析

**新增聚合报告：线程组->添加->监听器->聚合报告（Aggregate Report）**

- lable: sampler的名称
- Samples: 一共发出去多少请求,例如10个用户，循环10次，则是 100
- Average: 平均响应时间
- Median: 中位数，也就是 50％ 用户的响应时间

- 90% Line : 90％ 用户的响应不会超过该时间 （90% of the samples took no more than this time. The remaining samples at least as long as this）
- 95% Line : 95％ 用户的响应不会超过该时间
- 99% Line : 99％ 用户的响应不会超过该时间
- min : 最小响应时间
- max : 最大响应时间

- Error%：错误的请求的数量/请求的总数
- Throughput： 吞吐量——默认情况下表示每秒完成的请求数（Request per Second) 可类比为qps
- KB/Sec: 每秒接收数据量

## 六、Jmeter压测脚本JMX

### 6.1 测试计划
```xml
<!-- 测试计划 testname：计划名称， enabled：是否启用-->
<TestPlan guiclass="TestPlanGui" testclass="TestPlan" testname="测试计划" enabled="true">
  <stringProp name="TestPlan.comments"></stringProp>
  <boolProp name="TestPlan.functional_mode">false</boolProp>
  <boolProp name="TestPlan.tearDown_on_shutdown">true</boolProp>
  <boolProp name="TestPlan.serialize_threadgroups">false</boolProp>
  <elementProp name="TestPlan.user_defined_variables" elementType="Arguments" guiclass="ArgumentsPanel" testclass="Arguments" testname="用户定义的变量" enabled="true">
    <collectionProp name="Arguments.arguments"/>
  </elementProp>
  <stringProp name="TestPlan.user_define_classpath"></stringProp>
</TestPlan>
```

### 6.2 线程组
```xml
<!-- 线程组 testname：线程组名称，enabled：是否启用 -->
<ThreadGroup guiclass="ThreadGroupGui" testclass="ThreadGroup" testname="10人线程组" enabled="true">
  <stringProp name="ThreadGroup.on_sample_error">continue</stringProp>
  <!-- 循环控制器 -->
  <elementProp name="ThreadGroup.main_controller" elementType="LoopController" guiclass="LoopControlPanel" testclass="LoopController" testname="循环控制器" enabled="true">
    <!-- 是否永远循环 -->
    <boolProp name="LoopController.continue_forever">false</boolProp>
    <!-- 循环次数，如果continue_forever为true，循环测试为 -1 -->
    <stringProp name="LoopController.loops">2</stringProp>
  </elementProp>
  <!-- 线程数 -->
  <stringProp name="ThreadGroup.num_threads">30</stringProp>
  <!-- 启动时间 -->
  <stringProp name="ThreadGroup.ramp_time">5</stringProp>
  <!-- 调度器开关 -->
  <boolProp name="ThreadGroup.scheduler">false</boolProp>
  <!-- 持续时间 sec为单位-->
  <stringProp name="ThreadGroup.duration"></stringProp>
  <!-- 启动延迟 sec为单位 -->
  <stringProp name="ThreadGroup.delay"></stringProp>
</ThreadGroup>
```

### 6.3 HTTP请求(Sampler)
```xml
<!-- http请求 -->
<HTTPSamplerProxy guiclass="HttpTestSampleGui" testclass="HTTPSamplerProxy" testname="HTTP请求" enabled="true">
  <!-- 用户自定义变量 -->
  <elementProp name="HTTPsampler.Arguments" elementType="Arguments" guiclass="HTTPArgumentsPanel" testclass="Arguments" testname="用户定义的变量" enabled="true">
    <collectionProp name="Arguments.arguments"/>
  </elementProp>
  <!-- 服务器IP -->
  <stringProp name="HTTPSampler.domain">127.0.0.1</stringProp>
  <!-- 请求端口 -->
  <stringProp name="HTTPSampler.port">8080</stringProp>
  <!-- 请求协议 -->
  <stringProp name="HTTPSampler.protocol">http</stringProp>
  <!-- 请求编码类型 -->
  <stringProp name="HTTPSampler.contentEncoding"></stringProp>
  <!-- 请求路径 -->
  <stringProp name="HTTPSampler.path">/users</stringProp>
  <!-- 请求类型 -->
  <stringProp name="HTTPSampler.method">GET</stringProp>
  <!-- 跟随重定向开关 -->
  <boolProp name="HTTPSampler.follow_redirects">true</boolProp>
  <!-- 自动重定向开启 -->
  <boolProp name="HTTPSampler.auto_redirects">false</boolProp>
  <!-- 是否使用长连接 -->
  <boolProp name="HTTPSampler.use_keepalive">true</boolProp>
  <!-- 使用多个/表单进行发布 -->
  <boolProp name="HTTPSampler.DO_MULTIPART_POST">false</boolProp>
  <stringProp name="HTTPSampler.embedded_url_re"></stringProp>
  <!-- 连接超时时间 -->
  <stringProp name="HTTPSampler.connect_timeout"></stringProp>
  <!-- 请求超时时间 -->
  <stringProp name="HTTPSampler.response_timeout"></stringProp>
</HTTPSamplerProxy>
```

### 6.4 查看结果数
```xml
<ResultCollector guiclass="ViewResultsFullVisualizer" testclass="ResultCollector" testname="察看结果树" enabled="true">
  <boolProp name="ResultCollector.error_logging">false</boolProp>
  <objProp>
    <name>saveConfig</name>
    <value class="SampleSaveConfiguration">
      <time>true</time>
      <latency>true</latency>
      <timestamp>true</timestamp>
      <success>true</success>
      <label>true</label>
      <code>true</code>
      <message>true</message>
      <threadName>true</threadName>
      <dataType>true</dataType>
      <encoding>false</encoding>
      <assertions>true</assertions>
      <subresults>true</subresults>
      <responseData>false</responseData>
      <samplerData>false</samplerData>
      <xml>false</xml>
      <fieldNames>true</fieldNames>
      <responseHeaders>false</responseHeaders>
      <requestHeaders>false</requestHeaders>
      <responseDataOnError>false</responseDataOnError>
      <saveAssertionResultsFailureMessage>true</saveAssertionResultsFailureMessage>
      <assertionsResultsToSave>0</assertionsResultsToSave>
      <bytes>true</bytes>
      <sentBytes>true</sentBytes>
      <threadCounts>true</threadCounts>
      <idleTime>true</idleTime>
      <connectTime>true</connectTime>
    </value>
  </objProp>
  <stringProp name="filename"></stringProp>
</ResultCollector>
```

### 6.5 结果断言
```xml
<ResponseAssertion guiclass="AssertionGui" testclass="ResponseAssertion" testname="响应断言" enabled="true">
  <collectionProp name="Asserion.test_strings">
    <!-- 断言的内容 -->
    <stringProp name="49586">200</stringProp>
  </collectionProp>
  <!-- 断言失败，返回数据 -->
  <stringProp name="Assertion.custom_message">http error</stringProp>
  <!-- 断言的响应字段类型 -->
  <stringProp name="Assertion.test_field">Assertion.response_code</stringProp>
  <boolProp name="Assertion.assume_success">false</boolProp>
  <!-- 匹配规则 包括：2，匹配：1，equal：8，substring：16-->
  <!-- 否：+4， 或者：+22-->
  <intProp name="Assertion.test_type">8</intProp>
</ResponseAssertion>
```

### 6.6 聚合报告
```xml
<ResultCollector guiclass="StatVisualizer" testclass="ResultCollector" testname="聚合报告" enabled="true">
  <boolProp name="ResultCollector.error_logging">false</boolProp>
  <objProp>
    <name>saveConfig</name>
    <value class="SampleSaveConfiguration">
      <time>true</time>
      <latency>true</latency>
      <timestamp>true</timestamp>
      <success>true</success>
      <label>true</label>
      <code>true</code>
      <message>true</message>
      <threadName>true</threadName>
      <dataType>true</dataType>
      <encoding>false</encoding>
      <assertions>true</assertions>
      <subresults>true</subresults>
      <responseData>false</responseData>
      <samplerData>false</samplerData>
      <xml>false</xml>
      <fieldNames>true</fieldNames>
      <responseHeaders>false</responseHeaders>
      <requestHeaders>false</requestHeaders>
      <responseDataOnError>false</responseDataOnError>
      <saveAssertionResultsFailureMessage>true</saveAssertionResultsFailureMessage>
      <assertionsResultsToSave>0</assertionsResultsToSave>
      <bytes>true</bytes>
      <sentBytes>true</sentBytes>
      <threadCounts>true</threadCounts>
      <idleTime>true</idleTime>
      <connectTime>true</connectTime>
    </value>
  </objProp>
  <stringProp name="filename"></stringProp>
</ResultCollector>
```

## 七、自定义变量和CSV可变参数

### 7.1 自定义变量

**线程组->add -> Config Element(配置原件)-> User Definde Variable（用户定义的变量）**

- 引用方式 `${XXX}`，在接口中变量中使用


### 7.2 CSV可变参数

**线程组->add -> Config Element(配置原件)-> CSV data set config (CSV数据文件设置)**
