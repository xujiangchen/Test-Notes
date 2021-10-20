- [1、安装python环境](#1---python--)
- [2、查看pip版本](#2---pip--)
- [2、安装Robot Framework](#2---robot-framework)
- [3、安装robot framework ride](#3---robot-framework-ride)
- [4、常见问题](#4-----)
  * [4.1  ERROR: Command errored out with exit status 1:](#41--error--command-errored-out-with-exit-status-1-)
  * [4.2 RIDE输出乱码问题解决](#42-ride--------)
  * [4.3 rcommand: “no pybot“ --argumentfile](#43-rcommand---no-pybot----argumentfile)
  * [4.4 Suite ‘XXX‘ contains no tests matching name ‘XXX‘ in sin suite](#44-suite--xxx--contains-no-tests-matching-name--xxx--in-sin-suite)

#### 1、安装python环境

python官网(https://www.python.org/downloads/)



#### 2、查看pip版本

```
pip --version

如果提示不是最新的版本，建议安装pip的最新版本

python -m pip install --upgrade pip
```



#### 2、安装Robot Framework

```
pip install robotframework
```



#### 3、安装robot framework ride

```
pip install robotframework-ride

# 使用豆瓣的镜像
pip install -i https://pypi.douban.com/simple robotframework-ride

安装完成后会提示是否在桌面显示，选择确定
```



#### 4、常见问题

##### 4.1  ERROR: Command errored out with exit status 1:

```
安装 robotframework-ride 报错 ERROR: Command errored out with exit status 1:

有两种情况都有可能导致这个问题：（1）缺少文件 （2）python版本

第一种问题的解决方法：
1、下载该库文件对应版本的.whl文件
	下载地址：https://www.lfd.uci.edu/~gohlke/pythonlibs/
	
	版本选择说明：
	PyAudio‑0.2.11‑cp38‑cp38‑win_amd64.whl 表示适合python版本为3.8，Windows64位
	PyAudio‑0.2.11‑cp27‑cp27m‑win32.whl 表示适合python版本为2.7，Windows32位
	
2、将下载好对应文件，文件拷贝到python安装路径的pip.exe同级目录下。

3、3、win+R打开cmd命令窗口，切换到pip.exe路径下，执行以下命令：
	pip install 对应文件
	
  
第二种问题解决方法：

卸载当前版本python，降低python版本，目前3.9 版本以上的python版本都不支持robotframework-ride

```



##### 4.2 RIDE输出乱码问题解决

```
找到python的安装路径，然后按以下路径找到 testrunnerplugin.py 文件
\Lib\site-packages\robotide\contrib\testrunner\testrunnerplugin.py

找到代码块
encoding = {'CONSOLE': CONSOLE_ENCODING,
'SYSTEM': SYSTEM_ENCODING,
'OUTPUT': OUTPUT_ENCODING}　

修改
SYSTEM_ENCODING 为 OUTPUT_ENCODING

重启RIDE即可

含义：目的使系统输出也作为上级输出即py输出.
```



##### 4.3 rcommand: “no pybot“ --argumentfile

```
导致原因不知道,猜测是python 3.x 以上对某些exe文件或者bat 文件进行了删减和优化，只能手动指定Robot文件

解决办法:
1、随便选择一个用例
2、打开右边run的tab页
3、左上角的Execution Profile选择为 custom script
4、点击browse 指定 robot.exe 文件 
	（Python安装路径\Local\Programs\Python\Python36\Scripts\robot.exe）
```


##### 4.4 Suite ‘XXX‘ contains no tests matching name ‘XXX‘ in sin suite

```
RIED运行测试用例报错 Suite ‘XXX‘ contains no tests matching name ‘XXX‘ in sin suite


第一种情况:
	pip安装的各个组件之间的版本差异，导致相互不支持而引起报错，只能自行排查。

第二种情况：
	创建suite的时候format默认是txt，在以前基于python2.安装的RF都是使用的txt格式。目前基于python3安装的RF需要在创建suite的时候format选择为robot，即可正常运行。

第三种情况：
	应用的Library错误，或者没有引用到
```



