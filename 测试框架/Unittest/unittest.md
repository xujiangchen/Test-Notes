- [1、什么是Unittest](#1什么是unittest)
- [2、重要的概念](#2重要的概念)
  - [2.1 fixture](#21-fixture)
  - [2.2 Testcase](#22-testcase)
  - [2.3 Test suite](#23-test-suite)
  - [2.4 Test runner](#24-test-runner)
- [3、简单用法](#3简单用法)
- [4、常见配置](#4常见配置)
  - [4.1 setUp和tearDown](#41-setup和teardown)
  - [4.2 断言](#42-断言)
  - [4.3 unittest执行测试用例](#43-unittest执行测试用例)
    - [4.3.1 执行方法一：unittest.main()](#431-执行方法一unittestmain)
    - [4.3.2 执行方法二：加入容器中执行TestSuit](#432-执行方法二加入容器中执行testsuit)
    - [4.3.3 执行方法三：通过套件执行某个测试类](#433-执行方法三通过套件执行某个测试类)
    - [4.3.3 执行方法四：执行文件夹下所有的测试用例](#433-执行方法四执行文件夹下所有的测试用例)

## 1、什么是Unittest

unittest 是python 的单元测试框架，unittest 单元测试提供了创建测试用例，测试套件以及批量执行的方案， unittest 在安装pyhton 以后就直接自带了，直接import unittest 就可以使用。类似于java中的JUnit，C++的CppUnit

## 2、重要的概念

### 2.1 fixture

对一个测试用例环境的搭建和销毁，是一个fixture，通过覆盖 TestCase的setUp()和tearDown()方法来实现。这个有什么用呢？比如说在这个测试用例中需要访问数据库，那么可以在setUp() 中建立数据库连接以及进行一些初始化，在tearDown()中清除在数据库中产生的数据，然后关闭连接。注意tearDown的过程很重要，要为以后的 TestCase留下一个干净的环境。关于fixture，还有一个专门的库函数叫做fixtures，功能更加强大。

### 2.2 Testcase

一个TestCase的实例就是一个测试用例。什么是测试用例呢？就是一个完整的测试流程，包括测试前准备环境的搭建(setUp)，执行测试代码 (run)，以及测试后环境的还原(tearDown)。元测试(unit test)的本质也就在这里，一个测试用例是一个完整的测试单元，通过运行这个测试单元，可以对某一个问题进行验证。

### 2.3 Test suite

多个测试用例集合在一起，就是TestSuite，而且TestSuite也可以嵌套TestSuite

### 2.4 Test runner

是来执行测试用例的，其中的run(test)会执行TestSuite/TestCase中的run(result)方法

## 3、简单用法
- 1.用import unittest导入unittest模块
- 2.定义一个继承自unittest.TestCase的测试用例类，如class xxx(unittest.TestCase):
- 3.定义setUp和tearDown，这两个方法与junit相同，即如果定义了则会在每个测试case执行前先执行setUp方法，执行完毕后执行tearDown方法。
- 4.定义测试用例，名字以test开头，unittest会自动将test开头的方法放入测试用例集中。

```python
import unittest

class TestStringMethod(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        print("这是准备方法")

    def setUp(self):
        print("这是用例准备方法")

    def tearDown(self):
        print("这是用例结束方法")

    def test_up(self):
        self.assertEqual("foo".upper(), "FOO")

    def test_down(self):
        self.assertTrue("FOO".isupper())

    @unittest.skip("")
    def test_skip(self):
        print("不执行这个方法")

    @classmethod
    def tearDownClass(cls):
        print("这是结束方法")
```

## 4、常见配置

### 4.1 setUp和tearDown
- 基于测试方法级别的setUp和tearDown
  - 执行每个测试方法的时候都会执行一次
- 基于类级别的setUpClass和tearDownClass
  - 在执行这个类的里面的所有测试方法时，只在开始和结束的时候执行一次

### 4.2 断言
| **断言方法**                                       | 断言描述                                           |
| -------------------------------------------------- | -------------------------------------------------- |
| **assertEqual(arg1, arg2, msg=None)**              | 验证arg1=arg2，不等则fail                          |
| **assertNotEqual(arg1, arg2, msg=None)**           | 验证arg1 != arg2, 相等则fail                       |
| **assertTrue(expr, msg=None)**                     | 验证expr是true，如果为false，则fail                |
| **assertFalse(expr,msg=None)**                     | 验证expr是false，如果为true，则fail                |
| **assertIs(arg1, arg2, msg=None)**                 | 验证arg1、arg2是同一个对象，不是则fail             |
| **assertIsNot(arg1, arg2, msg=None)**              | 验证arg1、arg2不是同一个对象，是则fail             |
| **assertIsNone(expr, msg=None)**                   | 验证expr是None，不是则fail                         |
| **assertIsNotNone(expr, msg=None)**                | 验证expr不是None，是则fail                         |
| **assertIn(arg1, arg2, msg=None)**                 | 验证arg1是arg2的子串，不是则fail                   |
| **assertNotIn(arg1, arg2, msg=None)**              | 验证arg1不是arg2的子串，是则fail                   |
| **assertIsInstance(obj, cls, msg=None）**          | 验证obj是cls的实例，不是则fail                     |
| **assertNotIsInstance(obj, cls, msg=None)**        | 验证obj不是cls的实例，是则fail                     |
| **assertGreater (first, second, msg = None)**      | 验证first > second，否则fail                       |
| **assertGreaterEqual (first, second, msg = None)** | 验证first ≥ second，否则fail                       |
| **assertLess (first, second, msg = None)**         | 验证first < second，否则fail                       |
| **assertLessEqual (first, second, msg = None)**    | 验证first ≤ second，否则fail                       |
| **assertRegexpMatches (text, regexp, msg = None)** | 验证正则表达式regexp搜索==匹配==的文本text  regexp |

### 4.3 unittest执行测试用例
#### 4.3.1 执行方法一：unittest.main()
 会将类中所有的测试用例全部执行。
```python
if __name__ == "__main__":
    unittest.main()
```

#### 4.3.2 执行方法二：加入容器中执行TestSuit
  
```python
import unittest


class TestMethod(unittest.TestCase):

    def setUp(self) -> None:
        print("这是用例准备方法")

    def tearDown(self) -> None:
        print("这是用例结束方法")

    def test_one(self) -> None:
        self.assertEqual("foo".upper(), "FOO")


if __name__ == "__main__":
    suit = unittest.TestSuite()
    suit.addTest(TestMethod("test_one"))
    unittest.TextTestRunner().run(suit)
```
  
#### 4.3.3 执行方法三：通过套件执行某个测试类
  
```python
import unittest


class TestMethod(unittest.TestCase):

    def setUp(self) -> None:
        print("这是用例准备方法")

    def tearDown(self) -> None:
        print("这是用例结束方法")

    def test_one(self) -> None:
        self.assertEqual("foo".upper(), "FOO")


if __name__ == "__main__":
    suit1 = unittest.TestLoader().loadTestsFromTestCase(TestMethod)
    suit = unittest.TestSuite([suit1])
    unittest.TextTestRunner(verbosity=2).run(suit)
```

#### 4.3.3 执行方法四：执行文件夹下所有的测试用例

```python
import unittest

if __name__ == "__main__":
    # 文件夹的路径
    test_dir = "./pytest"
    discovery = unittest.defaultTestLoader.discover(test_dir, pattern="demo*.py")
    unittest.TextTestRunner(verbosity=2).run(discovery)
```