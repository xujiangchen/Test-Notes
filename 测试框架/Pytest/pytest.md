- [1、什么是pytest](#1什么是pytest)
- [2、用例的识别和运行的规则](#2用例的识别和运行的规则)
- [3、pytest的demo](#3pytest的demo)
- [4、运行时参数](#4运行时参数)
- [5、fixture 用法](#5fixture-用法)
  - [5.1 @pytest.fixture](#51-pytestfixture)
  - [5.2 共享 fixture 函数：conftest.py](#52-共享-fixture-函数conftestpy)
  - [5.3 yield方法](#53-yield方法)
  - [5.4 fixture 作用范围：Scope](#54-fixture-作用范围scope)


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