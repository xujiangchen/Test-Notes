- [1、什么是pytest](#1什么是pytest)
- [2、用例的识别和运行的规则](#2用例的识别和运行的规则)
- [3、pytest的demo](#3pytest的demo)
- [4、运行时参数](#4运行时参数)
- [5、fixture 用法](#5fixture-用法)
  - [5.1 @pytest.fixture](#51-pytestfixture)
  - [5.2 共享 fixture 函数：conftest.py](#52-共享-fixture-函数conftestpy)
  - [5.3 yield方法](#53-yield方法)
  - [5.4 fixture 作用范围：Scope](#54-fixture-作用范围scope)
- [6、参数化用例](#6参数化用例)
  - [6.1 参数化使用 @pytest.mark.parametrize](#61-参数化使用-pytestmarkparametrize)
    - [6.1.1 多次使用 parametrize](#611-多次使用-parametrize)
    - [6.1.2 @pytest.fixture 与 @pytest.mark.parametrize 结合](#612-pytestfixture-与-pytestmarkparametrize-结合)
  - [6.2 yaml 实现参数化](#62-yaml-实现参数化)
    - [6.2.1 读取yaml文件](#621-读取yaml文件)
    - [6.2.2 @pytest.mark.parametrize + yaml](#622-pytestmarkparametrize--yaml)
    - [6.2.3 Demo](#623-demo)
- [7、定制测试报告（allure）](#7定制测试报告allure)
  - [7.1 安装](#71-安装)
  - [7.2 使用](#72-使用)
    - [7.2.1 Demo](#721-demo)
    - [7.2.2 生成html报告](#722-生成html报告)
  - [7.3 常用特性](#73-常用特性)
    - [7.3.1 打标记](#731-打标记)
    - [7.3.2 通过标记，筛选用例执行](#732-通过标记筛选用例执行)


## 1、什么是pytest
- pytest是一个非常成熟的全功能的Python测试框架
  - 支持参数化
  - 能够支持简单的单元测试和复杂的功能测试
  - pytest具有很多第三方插件，并且可以自定义扩展
  - 测试用例的skip和xfail处理
  - 可以很好的和jenkins集成

相关文档：https://docs.pytest.org/en/latest/contents.html#toc

第三方库：https://pypi.org/search/?q=pytest

## 2、用例的识别和运行的规则

- 测试文件识别：
  - test_*.py
  - *_test.py
- 测试用例识别：
  - Test\* 类中包含的所有test_\* 的方法（测试方法中不能含有__init__方法）
  - 不在class中的所有test_\* 方法

## 3、pytest的demo
```python
import pytest


def func(x):
    return x + 1


def test_answer():
    assert func(3) == 5


class TestClass:
    def test_one(self):
        assert func(4) == 5


if __name__ == "__main__":
    pytest.main(["test_demo.py"])
```

## 4、运行时参数
```python
# test_demo.py [options] [file_or_dir] [file_or_dir] [...]

pytest.main(["-k test_", "test_demo.py", "-v"])
```

|  参数    |   作用   |  备注    |
| ---- | ---- | ---- |
| -s | 显示程序中的 print/logging 输出 |      |
| -v | 丰富信息模式, 输出更详细的用例执行信息 |      |
| -k | 运行包含某个字符串的测试用例。如：pytest -k add XX.py 表示运行 XX.py 中包含 add 的测试用例。 |      |
| -q | 简单输出模式, 不输出环境信息 |  |
| -x | 出现一条测试用例失败就退出测试。在调试阶段非常有用，当测试用例失败时，应该先调试通过，而不是继续执行测试用例。 | |


## 5、fixture 用法

pytest fixture是对传统的 xUnit 架构的setup/teardown功能的改进

### 5.1 @pytest.fixture
```python
import pytest

@pytest.fixture()
def login():
    print("登录")
    return 8

class Test_Demo():
    def test_case1(self):
        print("\n开始执行测试用例1")
        assert 1 + 1 == 2

    # 在需要执行前置步骤的时候，将方法以参数的形式传递给用例
    # 用例执行之前会去调用login的方法，并将值返回
    def test_case2(self, login):
        print("\n开始执行测试用例2")
        print(login)
        assert 2 + login == 10

    def test_case3(self):
        print("\n开始执行测试用例3")
        assert 99 + 1 == 100

if __name__ == '__main__':
    pytest.main()
```

### 5.2 共享 fixture 函数：conftest.py
多个测试文件可能都要调用 fixture 函数，可以将其移动到 conftest.py 文件中。conftest.py 文件中的 fixture 函数不需要在测试函数中导入，可以被 pytest 自动识别，查找顺序从测试类开始，然后是测试模块，然后是 conftest.py 文件，最后是内置插件和第三方插件。

### 5.3 yield方法
使用yield关键字可以实现setup/teardown的功能，在yield关键字之前的代码在case之前执行，yield之后的代码在case运行结束后执行

```python
import pytest

@pytest.fixture()
def login():
    print("登录")
    yield
    print("退出登录")

class Test_Demo():
    def test_case1(self):
        print("\n开始执行测试用例1")
        assert 1 + 1 == 2

    def test_case2(self, login):
        print("\n开始执行测试用例2")
        assert 2 + 8 == 10

    def test_case3(self):
        print("\n开始执行测试用例3")
        assert 99 + 1 == 100


if __name__ == '__main__':
    pytest.main()
```

### 5.4 fixture 作用范围：Scope

fixture 作用范围可以为module、class、session和function，默认作用域为function。
- function：每一个函数或方法都会调用
- class：每一个类调用一次
- module：每一个.py文件调用一次
- session：是多个文件调用一次

```python
# 一个class里面多个用例都调用了此fixture，那么只在class里所有用例开始前执行一次
# 即使是class里面的多个方法都调用了login，也只执行一次

import pytest

@pytest.fixture(scope="class")
def login():
    print("登录...")

```

## 6、参数化用例

### 6.1 参数化使用 @pytest.mark.parametrize

**parametrize( ) 方法源码：**
```python
def parametrize(self,argnames, argvalues, indirect=False, ids=None, \scope=None):

# argnames: 需要参数化的变量，String(逗号分隔)，list，tuple
# argvalues：参数化的值，list，list[tuple]
```
**Demo**
```python
import pytest


class TestClass:

    @pytest.mark.parametrize("a,b", [(10, 20), (30, 40)])
    def test_param(self, a, b):
        print(a+b)


if __name__ == "__main__":
    pytest.main(["test_demo.py", "-v"])
```
#### 6.1.1 多次使用 parametrize
同一个测试用例还可以同时添加多个 @pytest.mark.parametrize 装饰器, 多个 parametrize 的所有元素互相组合（类似笛卡儿乘积），生成大量测试用例。
```python
@pytest.mark.parametrize("x",[1,2])
@pytest.mark.parametrize("y",[8,10,11])
def test_foo(x,y):
    print(f"测试数据组合x: {x} , y:{y}")
```

#### 6.1.2 @pytest.fixture 与 @pytest.mark.parametrize 结合
如果测试数据**需要在 fixture 方法中使用，同时也需要在测试用例中使用**，可以在使用 parametrize 的时候添加一个参数 `indirect=True`，pytest 可以实现将参数传入到 fixture 方法中，也可以在当前的测试用例中使用。
```python
# 方法名作为参数
test_user_data = ['Tome', 'Jerry']

@pytest.fixture(scope="module")
def login_r(request):
    # 通过request.param获取参数
    user = request.param
    print(f"\n 登录用户：{user}")
    return user

@pytest.mark.parametrize("login_r", test_user_data,indirect=True)
def test_login(login_r):
    a = login_r
    print(f"测试用例中login的返回值; {a}")
    assert a != ""
```

### 6.2 yaml 实现参数化

#### 6.2.1 读取yaml文件

```python
data = yaml.safe_load(open('./data.yaml'))
```

#### 6.2.2 @pytest.mark.parametrize + yaml
- yaml文件内容
```yaml
-
  - 10
  - 20
-
  - 30
  - 40
```
- pytest写法
```
@pytest.mark.parametrize("a,b", yaml.safe_load(open('./data.yaml', encoding='utf-8')))
```

#### 6.2.3 Demo
```python

"""
environment:
  - env: dev

"""

class TestClass:

    @pytest.mark.parametrize("environment", yaml.safe_load(open("./data.yaml"))['environment'])
    def test_param(self, environment):
        env = environment['env']
        if "test" in env:
            print("这是测试环境", env)
        elif "dev" in env:
            print("这是开发环境", env)
        else:
            print("错误")

```

## 7、定制测试报告（allure）

### 7.1 安装
- 1.因为allure2需要在java的环境下，并且要求必须是jdk1.8级以上
- 2.安装pytest：`pip install pytest`
- 3.安装allure-pytest：`pip install allure-pytest`
- 4.安装allure：
  - 下载allure2：`https://github.com/allure-framework/allure2/releases`，下载后解压，放在某个位置；配置环境变量：环境变量path中加上解压好的文件夹下的bin目录下的allure.bat文件的路径

### 7.2 使用

#### 7.2.1 Demo

```python
import pytest
import allure

@allure.feature('test_module_01')
def test_case_01():
    """
    用例描述：Test case 01
    """
    assert 0


@allure.feature('test_module_02')
def test_case_02():
    """
    用例描述：Test case 02
    """
    assert 0 == 0


if __name__ == '__main__':
    pytest.main(['-s', '-q', '--alluredir', './report'])

```

运行结束之后通过,进入目录通过命令 `allure serve ./report` 生成测试报告

#### 7.2.2 生成html报告

执行代码后直接生成的是json文件，通过命令生成了html，但是如果以后再先查看就比较的麻烦，现在将这些json文件转换为html文件，用于保留。

- 生成HTML文件 `allure generate ./result/ -o ./report/ --clear` (覆盖其路径加--clear)
- 启动tomcat查看 `allure open -h 127.0.0.1 -p 8864 ./report`

### 7.3 常用特性

#### 7.3.1 打标记
allure报告可以包含很多详细的信息描述测试用例，包括epic、feature、story、title、issue、testcase、severity等
| 使用 | 参数说明 |
| --- | --- |
| @alluer.epic() | 敏捷中的概念,代表的是史诗故事，即非常大（巨大）的用户故事 |
| @alluer.feture() | 模块名称，功能点的描述 |
| @alluer.story() | 用户故事，具体而详细的功能点 |
| @alluer.title() | 用例的标题 |
| @alluer.testcase() | 测试用例的连接地址，对应测试系统里面的case |
| @alluer.issue() | 缺陷，对应缺陷管理系统里面相应的缺陷 |
| @alluer.description() | 对用例的相关描述 |
| @alluer.step() | 测试用例的步骤 |
| @alluer.severity() | 用例的等级 |
| @alluer.link() | 链接 |
| @alluer.attachment() | 附件 |

```python
import pytest
import allure


@pytest.fixture()
def open_web():
    print("前置条件：打开页面")


@allure.epic("dxp系统")
@allure.feature("登录模块")
class Test:

    @allure.testcase("www.case.com")
    @allure.issue("www.issue.com")
    @allure.title("登录测试用例1")
    @allure.story("登录成功")
    def test_login_success(self, open_web):
        print("登录成功！")
        pass

    @allure.testcase("www.case.com")
    @allure.issue("www.issue.com")
    @allure.title("登录测试用例1")
    @allure.story("登录失败")
    def test_login_failure(self, open_web):
        with allure.step("输入用户名"):
            print("输入用户名！")
        with allure.step("输入密码"):
            print("输入密码！")
        with allure.step("进行判断"):
            assert '1' == 1
            print("登录失败")


if __name__ == '__main__':
    pytest.main(['-s', '-q', 'test_allure.py', '--alluredir', './result'])
```
#### 7.3.2 通过标记，筛选用例执行

- 选择运行你要执行epic的用例
  - `pytest --alluredir ./report/allure --allure-epics=epic`
- 选择运行你要执行features的用例
  - `pytest --alluredir ./report/allure --allure-features=模块2`
- 选择运行你要执行features的用例
  - `pytest --alluredir ./report/allure --allure-stories="用户故事"`