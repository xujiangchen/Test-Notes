- [Allure](#allure)
- [Java环境使用Allure](#java----allure)
  * [前置条件](#前置条件)
  * [Allure的Maven](#Allure的Maven)
  * [Allure的注解](#Allure的注解)
  * [查看Allure的测试报告](#查看Allure的测试报告)


# Allure

Allure 是一款轻量级的开源自动化测试报告生成框架。它支持绝大部分测试框架，比如 TestNG、Junit 、pytest、unittest 等。

Allure的官网 https://docs.qameta.io/allure/#_testng

# Java环境使用Allure

## 前置条件
- 1、下载Allure的命令行工具 https://github.com/allure-framework/allure2/releases
- 2、配置环境变量：解压缩Allure命令行压缩包，并将其bin目录配置到环境变量下

## Allure的Maven

```xml
<dependency>
    <groupId>io.qameta.allure</groupId>
    <artifactId>allure-testng</artifactId>
    <version>2.13.0</version>
</dependency>
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.10</version>
</dependency>
```

## Allure的注解

```java
/**@Epic -Epics可用作您的产品或项目的大量需求的占位符。Epic将在适当的时候分为较小的用户故事。
*用户故事可以拆分为较小的任务，并且可以是较大的Feature和Epic的一部分。
*/
@Epic
@Features
//是一个标注信息注解，但是改标注可以把相同的标注统一到相同模块下用于筛选
@Stories/@Story
//使用@Severity批注测试缺陷等级，例如BLOCKER，CRITICAL，NORMAL，MINOR，TRIVIAL
@Severity(SeverityLevel.BLOCKER)
//测试方法描述
@Description("测试流程描述")
//@Step注释是对任何（公共，私有，受保护）对任何方法进行注释。例如- @Step（“输入{0}和{1}”）
@Step
//@Attachment-附件只是带有注释的方法，@Attachment该方法返回String或byte []，应将其添加到报表中。我们可以将故障屏幕截图作为附件
@Attachment
//@Links-我们可以将测试链接到某些资源，例如TMS（测试管理系统）或错误跟踪器。将测试用例链接到测试方法总是有帮助的。
@Link
```

## 查看Allure的测试报告

​		Allure的测试报告和ExtentReports的测试报告有所不同，Allure并不会直接生成对应的html文件，而是会生成多个js文件。

​		如果想要查看测试报告，首先进入对应项目的目录中，打开cmd窗口，通过`allure serve`生成对应的文档。
