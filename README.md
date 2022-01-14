- [前言](#前言)
- [一、测试报告](#一测试报告)
- [二、前端自动化测试](#二前端自动化测试)
  - [2.1 前端元素定位方式](#21-前端元素定位方式)
  - [2.2 Selenium WebDriver 3](#22-selenium-webdriver-3)
    - [2.2.1 Java + Selenium](#221-java--selenium)
    - [2.2.2 python + Selenium](#222-python--selenium)
- [三、自动化测试框架](#三自动化测试框架)
  - [3.1 Robot Framework](#31-robot-framework)
  - [3.2 TestNG](#32-testng)
  - [3.3 Unittest](#33-unittest)
  - [3.3 Pytest](#33-pytest)
- [四、接口自动化](#四接口自动化)
  - [4.1 接口协议与抓包](#41-接口协议与抓包)
- [五、安全测试](#五安全测试)


## 前言
用于记录测试工作中的各个学习笔记和问题解决方法

## 一、测试报告
- [ExtentReports测试报告](https://github.com/xujiangchen/Test-Notes/blob/main/Test-Report/extentreports.md)
- [Allure测试报告](https://github.com/xujiangchen/Test-Notes/blob/main/Test-Report/allure.md)


## 二、前端自动化测试
### 2.1 前端元素定位方式

**推荐的定位方式的优先级：id > name > CSS selector > Xpath**

- [xpath定位](https://github.com/xujiangchen/Test-Notes/blob/main/Web-Auto-Test/%E5%85%83%E7%B4%A0%E5%AE%9A%E4%BD%8D/xpath%E5%AE%9A%E4%BD%8D%E6%96%B9%E5%BC%8F.md)
- [css定位](https://github.com/xujiangchen/Test-Notes/blob/main/Web-Auto-Test/%E5%85%83%E7%B4%A0%E5%AE%9A%E4%BD%8D/css%E5%AE%9A%E4%BD%8D.md)

**css selector和xpath的简单对比**

- css是配合html来工作，它实现的原理是匹配对象的原理，而xpath是配合xml工作的，它实现的原理是遍历的原理，所以两者在设计上，css性能更优秀
- css selector 语言简洁，明了，相对xpath
- 前段开发主要是使用css，不使用xpath，所以在技术上面，我们可以获得帮助的机会非常多

> 个人吐槽：虽然说css selector有各种有点，在实际工作中，因为Vue,React等各种响应式布局的广泛应用，其实大多数下还是使用xpath而不是css selector，很是无语

### 2.2 Selenium WebDriver 3 

#### 2.2.1 Java + Selenium
- [用IDEA+Maven搭建一个简单的selenium测试环境]()

- [Selenium常用API](https://github.com/xujiangchen/Selenium-Webdrive-3/blob/main/Java/Warehouse/Selenium%E5%B8%B8%E7%94%A8API.md)
  
  Java 代码中如何使用Selenium的jar包所提供的各种方法，来模拟操作页面上的各种操作。
  
- [Selenium中的等待方式](https://github.com/xujiangchen/Selenium-Webdrive-3/blob/main/Java/Warehouse/%E7%AD%89%E5%BE%85%E6%96%B9%E5%BC%8F.md)

  在个人实际的应用中，显示等待和隐式等待一般配合使用，在初始化driver的时候，会定义一个时间稍长的隐式等待（一般10s），以防在某些页面加载慢但不需要校验的时候，页面元素没有加载处理而导致用例报错；
  而显示等待一般时间较短，用于某些测试用例中需要对页面元素进行特殊校验的时候使用。
  
 - [WebDriver的高级应用实例](https://github.com/xujiangchen/Selenium-Webdrive-3/blob/main/Java/Warehouse/WebDriver%E7%9A%84%E9%AB%98%E7%BA%A7%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B.md)

    通过WebDriver对富文本框，时间控件，上传下载控件等一些特殊组件的具体操作方法

- [PageObject模式](https://github.com/xujiangchen/Selenium-Webdrive-3/blob/main/Java/Warehouse/PageObject%E6%A8%A1%E5%BC%8F.md)

    什么是PageObject模式，我们为什么要使用这个模式，用PageObject写一个简单的用例

#### 2.2.2 python + Selenium
    
## 三、自动化测试框架

### 3.1 Robot Framework
- [安装 Robot Framework](https://github.com/xujiangchen/Test-Notes/blob/main/%E6%B5%8B%E8%AF%95%E6%A1%86%E6%9E%B6/RobotFramework/%E5%AE%89%E8%A3%85%20Robot%20Framework.md)
  
  Robot Framework 安装的正确姿势，和在安装中常见的异常情况解决方法
  
- [RobotFramework中的变量、赋值](https://github.com/xujiangchen/Test-Notes/blob/main/%E6%B5%8B%E8%AF%95%E6%A1%86%E6%9E%B6/RobotFramework/RobotFramework%E4%B8%AD%E7%9A%84%E5%8F%98%E9%87%8F%E3%80%81%E8%B5%8B%E5%80%BC.md)

  Robot Framework 中是如何对定义变量，并给变量赋值，那么不同的变量之间的优先级关系是什么。

- [如何实际应用RobotFramework]()

  在实际的使用中，我们应该正确的给RF分层，最大化的利用它关键字驱动的特性。

> Robot Framework 的个人使用体会：在实际的测试过程中RF确实有着不少优势，但是同样RF也存在着很多难以掩盖的缺点。
>
> 先来说说RF大致优点（优点网上很多，在此不在赘述）：
>
> 1. 图形化界面操作，表格化用例，降低编写接口和用例的难度
> 2. 关键字驱动，重用性好，利用现有关键字组装新关键字，简化自动化测试过程
> 3. 有比较高的可扩展性 (个人对这个优点认同度有限)
> 4. 易于集成，功能全面，支持WEB测试，SSH，Telnet，API接口多种测试方式。
> 
>
> 再来详细说说缺点：
> 1. 虽然它图形化界面操作，表格化用例的，但这种操作也它大大加大了测试用例的管理难度。
> 2. 它对python3的支持很差，而且RF的语法非常奇怪，对于有一点开发经验的人来说非常的不友好。
> 3. RF提供的很多关键字，都会有着意想不到的bug，对于稍微复杂的操作，都需要测试人员自己去定义关键字。
> 4. 在实际的使用过程中，RF的维护成本，其实非常的高，并没有网上说的那么好，对关键字进行改动时，难以评估其影响。
> 5. RF也存在一定的学习成本，总体来说没有学习python性价比高
> 6. RF的灵活性会随着系统的复杂程度而逐渐降低
> 7. RF自身也有很多多的bug，图像化界面很卡。
>
> 我对RF的使用建议是，如果系统较小，对于项目对技术依赖不高，只是单纯的接口或者web测试，可以使用RF。
>
> 但是如果项目体量大，迭代周期长，存在许多定制化开发的技术点，以接口结合场景测试为主。建议还是使用真正的python代码去完成自动化测试。
> 

### 3.2 TestNG

- [使用TestNG](https://github.com/xujiangchen/Test-Notes/tree/main/%E6%B5%8B%E8%AF%95%E6%A1%86%E6%9E%B6/TestNG)

  Java测试框架TestNG，testng的用法，特殊使用场景，如何进行testng.xml配置

### 3.3 Unittest

- [使用Unittest](https://github.com/xujiangchen/Test-Notes/tree/main/%E6%B5%8B%E8%AF%95%E6%A1%86%E6%9E%B6/Unittest)

  Unittest的简单介绍和使用，目前在自动化测试过程中不再建议使用Unittest测试框架。

### 3.3 Pytest

- [使用Pytest](https://github.com/xujiangchen/Test-Notes/tree/main/%E6%B5%8B%E8%AF%95%E6%A1%86%E6%9E%B6/Pytest)

  Pytest的使用和安装，pytest所特有的优化，以及如何通过allure定制化的生成测试报告

- [pytest插件开发](https://github.com/xujiangchen/Test-Notes/blob/main/%E6%B5%8B%E8%AF%95%E6%A1%86%E6%9E%B6/Pytest/pytest%E6%8F%92%E4%BB%B6%E5%BC%80%E5%8F%91.md)

  如何开发pytest插件，了解什么是hook函数，打包发布自定义的pytest插件

## 四、接口自动化

### 4.1 接口协议与抓包

- [常见协议和请求](https://github.com/xujiangchen/Test-Notes/blob/main/Interface/%E5%B8%B8%E8%A7%81%E6%8E%A5%E5%8F%A3%E5%8D%8F%E8%AE%AE.md)

  七层和四层网络协议，tcp和udp的异同，什么是Restful，http/https......

- [抓包分析](https://github.com/xujiangchen/Test-Notes/blob/main/Interface/%E6%8A%93%E5%8C%85.md)

  设置代理，使用curl命令行和Charles抓包工具进行抓包

## 五、安全测试

- [搭建Dvwa安全测试环境](https://github.com/xujiangchen/Test-Notes/blob/main/%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95/%E6%90%AD%E5%BB%BA%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E7%8E%AF%E5%A2%83.md)

- [常见安全测试漏洞](https://github.com/xujiangchen/Test-Notes/blob/main/%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95/%E5%B8%B8%E8%A7%81%E6%BC%8F%E6%B4%9E.md)