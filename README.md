# 前言
用于记录测试工作中的各个学习笔记和问题解决方法

## 一、测试报告
- [ExtentReports测试报告](https://github.com/xujiangchen/Test-Notes/blob/main/Test-Report/extentreports.md)
- [Allure测试报告](https://github.com/xujiangchen/Test-Notes/blob/main/Test-Report/allure.md)


## 二、前端自动化测试
### 2.1 前端元素定位方式

**推荐的定位方式的优先级：id > name > CSS selector > Xpath**

- [xpath定位](https://github.com/xujiangchen/Test-Notes/blob/main/Web-Auto-Test/%E5%85%83%E7%B4%A0%E5%AE%9A%E4%BD%8D/xpath%E5%AE%9A%E4%BD%8D%E6%96%B9%E5%BC%8F.md)
- [css定位]()

**css selector和xpath的简单对比**

- css是配合html来工作，它实现的原理是匹配对象的原理，而xpath是配合xml工作的，它实现的原理是遍历的原理，所以两者在设计上，css性能更优秀
- css selector 语言简洁，明了，相对xpath
- 前段开发主要是使用css，不使用xpath，所以在技术上面，我们可以获得帮助的机会非常多

> 个人吐槽：虽然说css selector有各种有点，在实际工作中，因为Vue,React等各种响应式布局的广泛应用，其实大多数下还是使用xpath而不是css selector，很是无语

### 2.2 Selenium (Java)

- [Selenium常用API](https://github.com/xujiangchen/Selenium-Webdrive-3-Java/blob/main/Warehouse/Selenium%E5%B8%B8%E7%94%A8API.md)
  
  Java 代码中如何使用Selenium的jar包所提供的各种方法，来模拟操作页面上的各种操作。

## 三、接口自动化测试

### 3.1 Robot Framework
- [安装 Robot Framework](https://github.com/xujiangchen/Test-Notes/blob/main/Interface-automation/%E5%AE%89%E8%A3%85%20Robot%20Framework.md)
  （Robot Framework 安装的正确姿势，和在安装中常见的异常情况解决方法）
