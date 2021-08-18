- [1、什么是TestNG](#1什么是TestNG)
- [2、maven依赖](#2maven依赖)
- [3、TestNG基本注解](#3TestNG基本注解)
- [4、TestNG特殊测试场景](#4TestNG特殊测试场景)
    - [4.1 预期异常测试](#41-预期异常测试)
    - [4.2 忽略测试](#42-忽略测试)
    - [4.3 超时测试](#43-超时测试)
    - [4.4 分组测试](#44-分组测试)
    - [4.5 依赖测试](#45-依赖测试)
    - [4.6 参数化测试](#46-参数化测试)
- [5、testNG的xml配置](#5testNG的xml配置)
# TestNG

## 1、什么是TestNG

TestNG是一个测试框架，其灵感来自JUnit和NUnit，但引入了一些新的功能，使其功能更强大，使用更方便。
TestNG是一个开源自动化测试框架;TestNG表示下一代(Next Generation的首字母)。 TestNG类似于JUnit(特别是JUnit 4)，但它不是JUnit框架的扩展。它的灵感来源于JUnit。它的目的是优于JUnit，尤其是在用于测试集成多类时。 TestNG的创始人是Cedric Beust(塞德里克·博伊斯特)。
TestNG消除了大部分的旧框架的限制，使开发人员能够编写更加灵活和强大的测试。 因为它在很大程度上借鉴了Java注解(JDK5.0引入的)来定义测试，它也可以显示如何使用这个新功能在真实的Java语言生产环境中。

## 2、maven依赖
```xml
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.1.0</version>
    <scope>test</scope>
</dependency>
```

## 3、TestNG基本注解
|  注解   | 描述  |
|  ----  | ----  |
| @BeforeSuite  | 在该套件的所有测试都运行在注释的方法之前，仅运行一次。 |
| @AfterSuite  | 在该套件的所有测试都运行在注释方法之后，仅运行一次。 |
| @BeforeClass  | 在调用当前类的第一个测试方法之前运行，注释方法仅运行一次。 |
| @AfterClass  | 在调用当前类的第一个测试方法之后运行，注释方法仅运行一次。 |
| @BeforeTest  | 注释的方法将在属于`<test>`标签内的类的所有测试方法运行之前运行。 |
| @AfterTest  | 注释的方法将在属于`<test>`标签内的类的所有测试方法运行之后运行。 |
| @BeforeGroups  | 配置方法将在之前运行组列表。 此方法保证在调用属于这些组中的任何一个的第一个测试方法之前不久运行。|
| @AfterGroups  | 此配置方法将在之后运行组列表。该方法保证在调用属于任何这些组的最后一个测试方法之后不久运行。 |
| @BeforeMethod  | 注释方法将在每个测试方法之前运行。 |
| @AfterMethod | 注释方法将在每个测试方法之后运行。|
| @DataProvider | 标记一种方法来提供测试方法的数据。 注释方法必须返回一个`Object[][]`，其中每个`Object[]`可以被分配给测试方法的参数列表。 要从该`DataProvider`接收数据的`@Test`方法需要使用与此注释名称相等的dataProvider名称。|
| @Factory  | 将一个方法标记为工厂，返回TestNG将被用作测试类的对象。 该方法必须返回`Object[]`。 |
| @Listeners  | 定义测试类上的侦听器。 |
| @Test | 将类或方法标记为测试的一部分。 |

## 4、TestNG特殊测试场景

### 4.1 预期异常测试
配置信息：`expectedExceptions` 期望抛出的异常
```java
@Test(expectedExceptions = ArithmeticException.class)
public void testException(){
    int i = 1 / 0;
    System.out.println("After division the value of i is :"+ i);
}
```

### 4.2 忽略测试
在敏捷开发之DDT的开发模式中，经常会存在，测试用例已经完成，但是实际代码没有完成的情况，使用`@Test(enabled = false)`,有助于禁用此测试用例。
```java
@Test(enabled = false)
public void skipTest(){
    System.out.println("skip");
}
```

### 4.3 超时测试
“超时”表示如果单元测试花费的时间超过指定的毫秒数，那么TestNG将会中止它并将其标记为失败。
```java
@Test(timeOut = 5000)
public void testThisShouldPass() throws InterruptedException {
    Thread.sleep(4000);
}

@Test(timeOut = 1000)
public void testThisShouldFail() {
    while (true){
        // to do nothing
    }
}
```

### 4.4 分组测试
分组测试是TestNG中的一个新的创新功能，它在JUnit框架中是不存在的。它允许您将方法调度到适当的部分，并执行复杂的测试方法分组。 您不仅可以声明属于某个分组的方法，还可以指定包含其他组的组。

```java
public class TestGroup {

    @Test(groups = "groupOne")
    public void testOne(){
        System.out.println("This is one");
    }

    @Test(groups = "groupOne")
    public void testTwo(){
        System.out.println("This is Two");
    }

    @Test(groups = "groupTwo")
    public void testThree(){
        System.out.println("This is Three");
    }

    @Test(groups = "groupTwo")
    public void testFour(){
        System.out.println("This is Four");
    }
}
```
```xml
<suite name="demo">

   <test name="Test">
       <groups>
           <run>
               <include name="groupOne">groupOne</include>
               <include name="groupTwo">groupTwo</include>
           </run>
       </groups>

       <classes>
           <class name="TestGroup"></class>
       </classes>
   </test>

</suite>
```

### 4.5 依赖测试

有时，我们可能需要以特定顺序调用测试用例中的方法，或者可能希望在方法之间共享一些数据和状态。

TestNG允许指定的依赖关系共有两种：
- 在`@Test`注释中使用属性`dependsOnMethods`
- 在`@Test`注释中使用属性`dependsOnGroups`

**第一种：**
如果`method1()`通过，那么将执行`method2()`。
```java
public class dependTest {

    @Test
    public void method1() {
        System.out.println("This is method 1");
    }

    @Test(dependsOnMethods = { "method1" })
    public void method2() {
        System.out.println("This is method 2");
    }

}
```
**第二种：**
如果分组`groupOne`和`groupTwo`通过，则`runFinal()`将被执行
```java
@Test(groups = "groupOne")
public void testOne(){
    System.out.println("This is one");
}

@Test(groups = "groupTwo")
public void testFour(){
    System.out.println("This is Four");
}

@Test(dependsOnGroups = { "groupOne", "groupTwo" })
public void runFinal() {
    System.out.println("runFinal");
}
```

### 4.6 参数化测试

在大多数情况下，会遇到业务逻辑需要大量测试的场景。参数化测试允许开发人员使用不同的值一次又一次地运行相同的测试。

TestNG可以通过两种不同的方式将参数直接传递给测试方法：
- 使用 `testng.xml`(后面介绍xml的时候在进行说明)
- 使用数据提供者

```java
public class TestParameterDataProvider {

    @Test(dataProvider = "provider")
    public void test(int number1, int number2){
        System.out.println(number1+ "===" +number2);
    }

    @DataProvider(name = "provider")
    public Object[][] testProvider(){
        return new Object[][]{{10, 20},{40, 50},{80, 90}};
    }
}
```
**进阶：** 根据测试方法名称传递不同的参数
```java
public class TestParameterDataProvider {

    @Test(dataProvider = "provider")
    public void test(int number1, int number2){
        System.out.println(number1+ "===" +number2);
    }

    @Test(dataProvider = "provider")
    public void test1(int number1, int number2){
        System.out.println(number1+ "===" +number2);
    }

    @DataProvider(name = "provider")
    public Object[][] testProvider(Method method){
        if (method.getName().equals("test")){
            return new Object[][]{{10, 20},{40, 50},{80, 90}};
        }else if (method.getName().equals("test1")){
            return new Object[][]{{1, 2},{4, 5},{8, 9}};
        }
        return null;
    }
}
```

## 5、testNG.xml配置
**xml的详细结构：**
```xml
<test name="xxxx">
　　<!-- 参数定义的方法 -->
　　<parameter name="first-name" value="Cedric"/>

　　<!-- groups的用法，前提是需要存在classes的组，否则所有方法不被运行 -->
　　<groups>
　　<!-- 定义组中组的方法 -->
　　　　<define name="groups_name">
　　　　　　<include name="group1"/>
　　　　　　<include name="group2"/>
　　　　</define>

　　　　<run>
　　　　　　<!-- 此处用组名来区别 -->
　　　　　　<inclue name="groups_name" />
　　　　　　<exclue name="groups_name" />
　　　　　　</run>
　　</groups>

　　<!-- classes的用法，classes中包含类名，类名底下可以包含方法名或排除方法名 -->
　　<classes>
　　　　<class name="class1">
　　　　　　<methods>
　　　　　　　　<!-- 此处用方法名来区别 -->
　　　　　　　　<inclue name="method_name" />
　　　　　　　　<exclue name="method_name" />
　　　　　　</methods>
　　　　</class>
　　</classes>
</test>
```

#### 5.1 `<suite>`   
testng.xml文档中最上层的元素

说明：一个xml文件只能有一个<suites>,是一个xml文件的根级<suite>由<test>和<parameters>组成

|参数|	说明|	使用方法|	参数值|
|  ----  | ----  | ----  | ----  |
|name|	必选项，<suite>的名字，将出现在reports里|	name="XXX"|	suite名字|
|junit|	是否执行Junit模式(识别setup()等)|	junit="true"|	true和false，默认false|
|verbose|	控制台输出的详细内容等级,0-10级（0无，10最详细）|	verbose="5"|	0到10|
|parallel|	是否在不同的线程并行进行测试，要与thread-count配套使用|	parallel="mehods"|	详见表格下内容，默认false|
|parent-module|	和Guice框架有关，只运行一次，创建一个parent injector给所有guice injectors|	 
|guice-stage|	和Guice框架有关|	guice-stage="DEVELOPMENT"|	DEVELOPMENT，PRODUCTION，TOOL，默认"DEVELOPMENT"|
|configfailurepolicy|	测试失败后是再次执行还是跳过，值skip和continue|	configfailurepolicy="skip"|	skip、continue，默认skip|
|thread-count|	与parallel配套使用，线程池的大小，决定并行线程数量|	thread-count="10"|	整数，默认5|
|annotations|	获取注解，值为javadoc时，使用JavaDoc的注释；否则用JDK5注释|	annotations="javadoc"|	javadoc|
|time-out|	设置parallel时，终止执行单元之前的等待时间（毫秒）|	time-out="10000"|	整数，单位毫秒|
|skipfailedinvocationcounts|	是否跳过失败的调用|	skipfailedinvocationcounts="true"|	true和false，默认false|
|data-provider-thread-count|	并发时data-provider的线程池数量|	data-provider-thread-count="5"|	整数|
|object-factory|	一个实现IObjectFactory接口的类，实例化测试对象|	object-factory="classname"|	类名|
|allow-return-values|	是否允许返回函数值|	all-return-values="true"|	true和false|
|preserve-order|	是否按照排序执行|	preserve-order="true"|	true和false，默认true|
|group-by-instances|	按照实例分组|	group-by-instances="true"|	true和false，默认false|

parallel
该参数的值false，methods，tests，classes，instances。默认false
- parallel必须和thread-count配套使用，否则相当于无效参数，thread-count决定了并行测试时开启的线程数量
- parallel="mehods"  TestNG将并行执行所有的测试方法在不同的线程里
- parallel="tests"  TestNG将并行执行在同一个`<test>`下的所有方法在不同线程里
- parallel="classes"  TestNG将并行执行在相同`<class>`下的方法在不同线程里
- parallel="instances"  TestNG将并行执行相同实例下的所有方法在不同的线程里

#### 5.2 `<test>`
说明：一个`<suite>`下可以有多个`<test>`，可以通过`<suite>`的parallel="tests"来进行并行测试，必须和thread-count配套使用，否则是无效参数,`<test>`由`<parameters>`、`<groups>`、`<classes>`三部分组成.

|参数	|说明|	使用方法|	参数值|
|  ----  | ----  | ----  | ----  |
|name|	test的名字，将出现在报告里|	name="testname"|	test的名字|
|junit|	是否按照Junit模式运行|	junit="true"|	true和false，默认false|
|verbose|	控制台输出的详细内容等级,0-10级（0无，10最详细），不在报告显示|	verbose="5"	|0到10|
|parallel|	是否在不同的线程并行进行测试，要与thread-count配套使用|	parallel="mehods"|	与suite的parallel一致，默认false|
|thread-count|	与parallel配套使用，线程池的大小，决定并行线程数量|	thread-count="10"|	整数，默认5|
|annotations|	获取注解，值为javadoc时，使用JavaDoc的注释；否则用JDK5注释|	annotations="javadoc"|	javadoc|
|time-out|	设置parallel时，终止执行单元之前的等待时间（毫秒）|	time-out="10000"|	整数，单位毫秒|
|enabled|	标记是否执行这个test|	enabled="true"|	true和false，默认true|
|skipfailedinvocationcounts|	是否跳过失败的调用|	skipfailedinvocationcounts="true"|	true和false，默认false|
|preserve-order|	是否按照排序执行，如果是true，将按照xml文件中的顺序去执行|	preserve-order="true"|	true和false，默认true|
|allow-return-values|	是否允许返回函数值|	all-return-values="true"|	true和false，默认false|


#### 5.3 `<parameter>`
说明：提供测试数据，有name和value两个参数

声明方法：`<parameter name = "parameter_name" value = "parameter_value "/>`。

testng.xml文件中的`<parameter>`可以声明在`<suite>`或者`<test>`级别，在`<test>`下的`<parameter>`会覆盖在`<suite>`下声明的同名变量