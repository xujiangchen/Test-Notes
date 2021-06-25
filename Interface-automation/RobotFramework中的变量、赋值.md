- [RobotFramework中的变量、赋值](#robotframework-------)
  * [1、变量标识符](#1变量标识符)
  * [2、变量声明](#2变量声明)
  * [3、变量作用域](#3变量作用域)
  * [4、如何定义一个变量](#4如何定义一个变量)
  * [5、变量之间的优先级(重点)](#5变量之间的优先级重点)
    + [5.1 启动参数 和 结构化参数](#51-启动参数-和-结构化参数)
    + [5.2 启动参数 和 自定义变量](#52-启动参数-和-自定义变量)
    + [5.3 结构化参数 和 自定义变量](#53-结构化参数-和-自定义变量)
    + [5.4 自定义变量之间](#54-自定义变量之间)


# RobotFramework中的变量、赋值

## 1、变量标识符

每个变量都可以用 `变量标识符{变量名}` 来表示

- Scalar用$标识，${变量名}，表示一个数据，作为参数传递时，作为一个参数
- List用@标识，@{变量名}，表示一组数据，作为参数传递时，有几个数据就是几个变量
- Dictionary用&标识，&{变量名}，表示一组键值对数据，作为参数传递时，几个键值对就是几个变量

## 2、变量声明

RobotFramework 的底层其实就是python，所以rf中的变量也是赋初值就可以直接使用，不需要额外的进行变量声明这个步骤


## 3、变量作用域

默认情况下，变量只在作用域内有效。

但是也可以通过对于关键字的处理，对作用域进行改变，常用的关键子有 `Set Global Variable`、`Set Suit Variable`、`Set Test Variable`。

**注：在还不理解的情况下尽量少用改变作用域的关键字。**

## 4、如何定义一个变量

```
# 创建一个Scalar变量
${name}    Set Valiable    hello world

# 创建一个List变量
@{list}    Create List    a    b    123

# 创建一个字典变量
&{dict}    Create Dictionary    key=value    foo=bar
&{dict}    Create Dictionary    key    value    foo    bar
```

其中Scalar 可以于 Dictionary，List 这两种变量相互转化的

```
@{list}    Create List    a    b    123
&{dict}    Create Dictionary    key=value    foo=bar

# 列表、字典作为参数，是单个值，用$符号，数据就会变成单个值
log   ${list}
log   ${dict} 
```

## 5、变量之间的优先级(重点)

在使用 robot Framework 中，我们常常会使定义各种各样的变量，来满足不同测试用例中数据的变化，所以了解这些定义的变量是在什么时候生效，和这些变量的优先级对简化测试用例会有很大的帮助。

首先我们要了解我们一般会在哪里定义这些变量：

- 在启动脚本时，会通过启动命令定义一些**启动参数**，`--variable`，我们可以将这些参数理解为全局变量
- 为了消减自动化脚本的维护成本，我们常常会事先在rosource文件或者python文件中定义一些变量，我们将这些变量称之为**结构化参数**
- 在书写自动话脚本时我们常常会使用各种各样的**自定义变量**。

### 5.1 启动参数 和 结构化参数

启动参数 > 结构话参数

```
在启动脚本时添加 --variable data:hello

*** Variables ***
${data}           333

*** Test Cases ***
demoCase
	log    ${data}


# 结果为
# INFO : hello
```

### 5.2 启动参数 和 自定义变量 

自定义变量 > 启动参数

```
在启动脚本时添加 --variable data:hello

*** Test Cases ***
demoCase
    ${data}    Set Variable    world
    log    ${data}


# 结果为
# INFO : world
```

### 5.3 结构化参数 和 自定义变量 

自定义变量 > 结构化参数

```
*** Variables ***
${data}           123

*** Test Cases ***
demoCase
    ${data}    Set Variable    world
    log    ${data}


# 结果为
# INFO : world
```

### 5.4 自定义变量之间

前面说到过 变量作用域可以人为的修改

- `Set Global Variable`	是设置全局变量，从设置这个变量的地方开始，**如果后续代码中不在进行任何赋值操作**，直到所有脚本全部跑完，所有调用的地方都是同一个值。
- `Set Suit Variable`  是设定suite级变量，从设置这个变量的地方开始，**如果后续代码中不在进行任何赋值操作**，直到这个测试套件运行结束，所有调用的地方都是同一个值。
- `Set Test Variable`  是设定case级变量，从设置这个变量的地方开始，**如果后续代码中不在进行任何赋值操作**，直到这个测试用例运行结束，所有调用的地方都是同一个值。

在脚本中上述三个关键词定义的变量的优先级会如何变化呢？

- 就近原则，不论是用上面三个中的那一种方法，都是在使用前的最后一个赋值生效。
- 作用域之间不会互相影响，

  ```
  *** Test Cases ***
  caseOne
      Set Suite Variable    ${data}    hello
      Set Test Variable    ${data}    world
      log    ${data}
  
  caseTwo
      log    ${data}
      
  # 结果为
  # INFO : world
  # INFO : hello
  # 虽然在第一个用例中对data二次赋值了，但是当 Set Test Variable 的作用域结束后，Set Suite Variable 生效的作用域中的值没有修改。
  ```

