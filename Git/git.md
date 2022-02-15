- [Git](#git)
  - [linux上安装git](#linux上安装git)

# Git

## linux上安装git
yum install gityum install 安装的git不是最新版本，如需最新版本需要自行编译

**编译git源码安装**:
- 到网站下载合适的版本​ `https://mirrors.edge.kernel.org/pub/software/scm/git/`
- 安装git的依赖项
  - `yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel`
  - `yum install gcc perl-ExtUtils-MakeMaker`
- 如果有旧版本的git则需要移除 `yum remove git`
- 解压git压缩包，并进入解压后的文件
- 预编译git `./configure --prefix=/usr/local/software/git`(git解压后的实际目录)
- 编译和安装git `make && make install`
- 将git的脚本软连接到/usr/bin/ 目录下 `ln -s /usr/local/software/git/bin/* /usr/bin/`(git解压后的实际目录)