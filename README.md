<h1 align="center">Linux</h1>

[**🏷️基础**](#基础)&emsp;&emsp;[命令](#命令)&emsp;&emsp;[Vim](#Vim)&emsp;&emsp;[GCC](#GCC)&emsp;&emsp;[库](#库)&emsp;&emsp;[Makefile](#Makefile)&emsp;&emsp;[CMake](#CMake)&emsp;&emsp;[GDB](#GDB调试)

[**🏷️文件IO**](#文件IO)

[**🏷️进程**](#进程)&emsp;&emsp;[控制](#进程控制)&emsp;&emsp;[通信](#进程通信)&emsp;&emsp;[守护进程](#守护进程)&emsp;&emsp;[线程](#线程)

[**🏷️套接字**](#套接字通信)&emsp;&emsp;[概念](#概念)&emsp;&emsp;[Socket](#Socket)&emsp;&emsp;[IO多路转接](#IO多路转接)

## 基础

**Linux 下文件和目录的特点**

* `.` 开头是隐藏文件，用 -a 显示
* **.** 代表当前目录
* **..** 代表上一级目录
* **文件**/**目录** 名称最长 `256` 个字符
* 所有的 **目录** 和 **文件名** 对大小写敏感

**目录下文件的详细信息**

​	权限 硬链接数 拥有者 组大小 时间 名称

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/Rights.png" style="zoom:70%;" />

**passwd 文件**

`/etc/passwd` 文件存放的是用户的信息，由 6 个分号组成的 7 个信息，分别是

1. 用户名
2. 密码（x，表示加密的密码）
3. UID（用户标识）
4. GID（组标识）
5. 用户全名或本地帐号
6. 家目录
7. 登录使用的 Shell，就是登录之后，使用的终端命令，`ubuntu` 默认是 `dash`



`/etc` 目录是专门用来保存 **系统配置信息** 的目录

`/etc/passwd` 是用于保存用户信息的文件

`/usr/bin/passwd` 是用于修改用户密码的程序

`/etc/group` 是用于保存组信息的文件



**`bin` 和 `sbin`**

* 在 `Linux` 中，绝大多数可执行文件都是保存在 `/bin`、`/sbin`、`/usr/bin`、`/usr/sbin`
* `/bin`（`binary`）是二进制执行文件目录，主要用于具体应用
* `/sbin`（`system binary`）是系统管理员专用的二进制代码存放目录，主要用于系统管理
* `/usr/bin`（`user commands for applications`）后期安装的一些软件
* `/usr/sbin`（`super user commands for applications`）超级用户的一些管理程序

> * `cd` 这个终端命令是内置在系统内核中的，没有独立的文件，因此用 `which` 无法找到 `cd` 命令的位置

### 命令

#### 基本操作

`tab`	自动补齐

`↑/↓`	曾经使用过的命令

`ctrl + c`	取消

`command --help`	显示 `command` 命令的帮助信息

`man command`      	查阅 `command` 命令的使用手册

| 操作键 | 功能                 |
| :----- | :------------------- |
| 空格   | 显示手册页的下一屏   |
| Enter  | 一次滚动手册页的一行 |
| b      | 回滚一屏             |
| f      | 前滚一屏             |
| q      | 退出                 |
| /word  | 搜索 **word** 字符串 |

| 通配符 | 含义                                 |
| ------ | ------------------------------------ |
| *      | 代表任意个数个字符                   |
| ?      | 代表任意一个字符，至少 1 个          |
| []     | 表示可以匹配字符组中的任一一个       |
| [abc]  | 匹配 a、b、c 中的任意一个            |
| [a-f]  | 匹配从 a 到 f 范围内的的任意一个字符 |

#### 文件操作

| cd    | 变更当前工作目录                       |
| ----- | -------------------------------------- |
| cd    | 切换到当前用户的主目录(/home/用户目录) |
| cd ~  | 切换到当前用户的主目录(/home/用户目录) |
| cd .  | 保持在当前目录不变                     |
| cd .. | 切换到上级目录                         |
| cd -  | 可以在最近两次工作目录之间来回切换     |

| ls   | 显示目录内容                                 |
| ---- | -------------------------------------------- |
| -a   | 显示指定目录下所有子目录与文件，包括隐藏文件 |
| -l   | 以列表方式显示文件的详细信息                 |
| -h   | 配合 -l 以人性化的方式显示文件大小           |

| rm   | 删除文件或目录                                        |
| ---- | ----------------------------------------------------- |
| -f   | 强制删除，忽略不存在的文件，无需提示                  |
| -r   | 递归地删除目录下的内容，**删除文件夹** 时必须加此参数 |

| cp   | 复制给定文件或目录至另一个文件或目录中                       |
| ---- | :----------------------------------------------------------- |
| -i   | 覆盖文件前提示                                               |
| -r   | 若源文件是目录文件，将递归复制该目录下的所有子目录和文件，目标文件必须为一个目录名 |

| cat  | 查看文件内容、创建文件、文件合并、追加文件内容等 |
| ---- | ------------------------------------------------ |
| -b   | 对非空输出行编号                                 |
| -n   | 对输出的所有行编号                               |

| grep | 文本搜索                                 |
| ---- | ---------------------------------------- |
| -n   | 显示匹配行及行号                         |
| -v   | 显示不包含匹配文本的所有行（相当于求反） |
| -i   | 忽略大小写                               |

| 参数 | 含义                         |
| ---- | ---------------------------- |
| ^a   | 行首，搜寻以 **a** 开头的行  |
| ke$  | 行尾，搜寻以 **ke** 结束的行 |

`touch`	创建文件或修改文件末次修改时间

`mkdir`	创建一个新的目录	[-p 递归]

`tree [目录名]`	  以树状图列出文件目录结构	[-d 只显示目录]

`cp 源文件 目标文件`	  复制文件或者目录

`mv 源文件 目标文件`	  移动文件或者目录／文件或者目录重命名

`mv` 	移动并重命名文件或目录

`more` 	分屏显示文件内容，每次显示一页

`echo`	显示参数指定的文字

`>`	输出

`>>`	追加

`|`	将一个命令的输出通过管道做为另一命令的输入

`find`	在特定的目录下搜索符合条件的文件	[-name "*.txt" 按名称]

`ln -s [目标路径] [软链接路径]`	建立文件的软链接	[-s 软链接]

#### 用户／组 权限

| usermod                     | 设置用户的主组／附加组和登录Shell |
| --------------------------- | --------------------------------- |
| usermod -g 组 用户名        | 修改用户的主组（passwd 中的 GID） |
| usermod -G 组 用户名        | 修改用户的附加组                  |
| usermod -s /bin/bash 用户名 | 修改用户登录 Shell                |

`su`	使用另一个用户的身份

`sudo`	以其他身份来执行命令，预设的身份为 `root`

`groupadd 组名`	  添加组

`groupdel 组名`	删除组

`cat /etc/group`	确认组信息

`chgrp -R 组名 文件/目录名`	递归修改文件/目录的所属组

`useradd -m -g 组 新建用户名`	  添加新用户	[-m 自动建立用户家目录，-g 指定用户所在的组]

`passwd 用户名`	设置用户密码

`userdel -r 用户名`	  删除用户	[自动删除用户家目录]

`cat /etc/passwd \| grep 用户名`	确认用户信息

`id [用户名]`	查看用户 UID 和 GID 信息

`who`	  查看当前所有登录的用户列表

`whoami`	  查看当前登录用户的账户名

`which` 	查看执行命令所在位置

`su - 用户名`	切换用户，并且切换目录	[  - 切换到用户家目录]

`exit`	  退出当前登录账户

`chown 用户名 文件名|目录名`	修改文件|目录的拥有者

`chgrp -R 组名 文件名|目录名`	递归修改文件|目录的组

`chmod +/-rwx 文件名|目录名`	修改用户／组对文件／目录的 读|写|执行 权限

#### 系统相关

| shutdown 选项 时间 | 关机／重新启动	[-r 重启]         |
| ------------------ | ----------------------------------- |
| shutdown -r now    | 重新启动操作系统，其中 now 表示现在 |
| shutdown now       | 立刻关机，其中 now 表示现在         |
| shutdown 20:25     | 系统在今天的 20:25 会关机           |
| shutdown +10       | 系统再过十分钟后自动关机            |
| shutdown -c        | 取消之前指定的关机计划              |

`date`	查看系统时间

`cal`	 查看日历，`-y` 选项可以查看一年的日历

`df -h`	显示磁盘剩余空间

`du -h [目录名]`	显示目录下的文件大小

`ps aux`	查看进程的详细状况

`top`	动态显示运行中的进程并且排序

`kill [-9] 进程代号`	  终止指定代号的进程，`-9` 表示强行终止

#### 网络

`ifconfig`	查看/配置计算机当前的网卡配置信息

`ping`	检测到目标 ip地址 的连接是否正常

#### SSH

`ssh 用户名@ip`	关机／重新启动	[-p port 指定端口号]

`scp 用户名@ip:文件名或路径 用户名@ip:文件名或路径`	远程复制文件

`ssh-keygen` 	生成 SSH 钥匙

`ssh-copy-id -p port user@remote`	上传公钥到服务器

**ssh配置别名，在 `~/.ssh/config` 下追加以下内容：**

```
Host 别名
    HostName ip地址
    User itheima
    Port 22
```

#### 打包&压缩

| 选项 | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| c    | 生成档案文件，创建打包文件                                   |
| x    | 解开档案文件                                                 |
| v    | 列出归档解档的详细过程，显示进度                             |
| f    | 指定档案文件名称，f 后面一定是 .tar 文件，所以必须放选项最后 |

`tar -cvf 打包文件.tar 被打包的文件／路径...`	打包文件

`tar -xvf 打包文件.tar`	解包文件

`tar -zcvf 打包文件.tar.gz 被压缩的文件／路径...`	压缩文件

`tar -zxvf 打包文件.tar.gz`	解压缩文件

`tar -zxvf 打包文件.tar.gz -C 目标路径`	解压缩到指定路径	[-C 指定目录，必须存在]

`tar -jcvf 打包文件.tar.bz2 被压缩的文件／路径...`	压缩文件

`tar -jxvf 打包文件.tar.bz2`	解压缩文件

#### 其他

`sudo apt install 软件包`	安装软件

`sudo apt remove 软件名`	卸载软件

`sudo apt upgrade`	更新已安装的包

`git rebase --exec 'GIT_COMMITTER_DATE="$(git log -1 --format=%at)" git commit --amend --no-edit -n -S' -i HEAD~n`	为前n次commit签名

### Vim

Vim中共有三种模式： **命令模式**  **末行模式**  **编辑模式**



**命令模式**

`vim 文件名`	打开文件

`ZZ`	保存退出

`gg=G`	代码格式化

`h`,`j`,`k`,`l`	光标移动

`0`	移至行首

`$`	移至行尾

`gg`	移至文件头

`G`	移至文件尾

`nG`	行跳转

`v`	字符可视模式

`V`	行可视模式

`ctrl + v`	块可视模式

`I`	insert模式（可进行//）

`d`	删除(剪切)

`dd`	删除当前行

`y`	复制

`yy`	复制当前行

`p`	粘贴

`u`	撤销

`ctrl + r`	反撤销

`/n`	向下查找

`#N`	向下查找本单词

**编辑模式**

`i`	编辑模式

`o`	编辑模式（另起一行）

**末行模式**

`:`	末行模式

`Esc × 2`	命令模式

`q`	退出

`q!`	强制退出

`w`	保存

`wq`	保存退出

`[行1,行2]s/旧值/新值/g`	  [行1,行2]替换

`%s/旧值/新值/g`	  全部替换

`vsp [文件名]`	垂直分屏

`-O`	vim打开文件时垂直分屏

`ctrl + w + w`	  屏幕间切换

`qall`	同时退出多个屏幕

`:行号`	行跳转

`!命令`	执行shell命令

### GCC

`gcc -o Dest a.c`

- **注意：c文件gcc，cpp文件g++**

**选项：**

`-E/S/c`	预处理/编译/汇编

`-o [destFile] [srcFile]`	链接

`-I`	指定头文件的搜索目录

`-L`	库的路径

`-l`	库名

`-fpic`	生成与位置无关的代码

`-shared`	生成共享目标文件

### 库

静态库：libNAME**.a**&emsp;&emsp;&emsp;&emsp;&emsp;动态库：libNAME**.so**

- **静态库制作**

```shell
# 汇编
$ gcc srcFile.c -c
# 打包	c:创建库 s:创建索引 r:插入模块
$ ar rcs libNAME.a srcFile.o
# 发布：libNAME.a srcFile.h
```

**使用**

```shell
# 编译 静态库,头文件,测试代码
$ gcc -o Dest main.c -L ./ -l NAME
```

- **动态库制作**

```shell
# 汇编
$ gcc srcFile.c -c -fpic
# 打包
$ gcc -o libNAME.so srcFile.o -shared
# 发布：libNAME.so srcFile.h
```

**使用**

```shell
# 编译 动态库,头文件,测试代码
$ gcc -o Dest main.c -L ./ -l NAME
```

**关于动态库无法加载问题**

`ldd`	检测程序能否加载到动态库

- 添加系统环境变量
  1. 找到配置文件 *~/.bashrc*		//此为用户级别，系统级别：*/etc/profile*
  2. 在文件中追加 `export LD_LIBRARY_PATH =$LD_LIBRARY_PATH :动态库的绝对路径`
  3. 重启终端或执行`. ~/.bashrc`	// . 是 source 的简写
- 更新系统动态库缓存
  1. 找到动态库所在的绝对路径（不含库名，例如：*/home/voxhugh/Library/*）
  2. 将以上路径追加至文件 */etc/ld.so.conf* 中
  3. 更新配置文件数据至缓存中 `sudo ldconfig`

- 拷贝库的软链接至库目录

### Makefile

**make：解释 makefile 中的指令，实现自动化编译**



**规则**

```makefile
# 每条规则的语法格式:
target1,target2...: depend1, depend2, ...
	command
	......
	......
```

**变量**

1. 自定义变量

   ```makefile
   # 定义时必须初始化
   obj=a.o  b.o  c.o  c.o
   $(obj)	# 取变量的值
   ```

3. 自动变量

   ```makefile
   $^	# 所有依赖文件	(以空格间隔且不重复)
   $@	# 目标文件
   $<	# 首个依赖文件	
   ```

**模式匹配**

```makefile
# 依赖一个,目标一个, 通过通配符 % 匹配文件名
%.o:%.c
	gcc $< -c
```

**函数**

```makefile
# 获取指定目录下指定类型的文件名
$(wildcard PATTERN...)
# 按指定模式替换指定文件名的后缀
$(patsubst <pattern>,<replacement>,<text>)

# wildcard举例: 
src = $(wildcard /home/Tom/a/*.c *.c)
# patsubst举例：
src = a.cpp b.cpp c.cpp
obj = $(patsubst %.cpp, %.o, $(src)) 
```

**完美编写示范**

```makefile
# 搜索当前目录下的源文件
src=$(wildcard *.c)
# 将源文件的后缀替换为 .o
obj=$(patsubst %.c, %.o, $(src))
target=Dest

# 链接
$(target):$(obj)
        gcc -o $(target) $(obj)

# 汇编
%.o:%.c
        gcc $< -c

# 删除 生成文件 可执行程序
.PHONY:clean	# 声明为伪文件，防止make检测时间戳
clean:
        -rm $(obj) $(target) 
        echo "hello"
```

### CMake

跨平台自动化构建系统生成工具



- **语法**

```cmake
cmake_minimum_required(VERSION 3.10)		# 指定 CMake 的最低版本要求

project(MyPrj CXX)				# 定义项目名及语言

add_executable(MyExe main.cpp)			# 指定生成的目标文件和源文件

add_library(MyLib STATIC library.cpp)		# 创建一个库及源文件

target_link_libraries(MyExe MyLib)		# 链接目标文件和库

find_package(Boost 1.70 REQUIRED)		# 查找库，指定版本

include_directories(MyPrj/include)		# 设置包含目录

link_directories(${Boost_LIBRARY_DIRS})		# 设置链接目录

target_include_directories(MyExe PRIVATE ${PROJECT_SOURCE_DIR}/include)	# 设置目标属性

if(expr)					# 条件语句，endif是结束标志
endif()

set(MY_VAR "Hello")				# 定义变量

message(STATUS "Variable is ${MY_VAR}")		# 使用变量
```

- **流程**

```shell
MyProject/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   ├── lib/
│   │   ├── module1.cpp
│   │   ├── module2.cpp
│   ├── include/
│       └── mylib.h
└── tests/
    ├── test_main.cpp
    └── CMakeLists.txt

# 编写 CMakeLists.txt
$ touch CMakeLists.txt src/CMakeLists.txt tests/CMakeLists.txt
# 项目根目录下创建构建目录
$ mkdir build && cd build
# 构建
$ cmake ..
# 编译
$ make
# 运行测试
$ ./MyExecutable
$ ./TestMyLib
```

- **MyProject/**

```cmake
cmake_minimum_required(VERSION 3.10)	# 指定最低 CMake 版本
project(MyPrj VERSION 1.0)          	# 定义项目名称和版本

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 包含头文件路径
include_directories(${PROJECT_SOURCE_DIR}/src/include)

# 添加子目录
add_subdirectory(src)
add_subdirectory(tests)
```

- **MyProject/src/**

```cmake
# 创建库目标
add_library(MyLib STATIC
    lib/module1.cpp
    lib/module2.cpp
)

# 指定库的头文件
target_include_directories(MyLib PUBLIC ${CMAKE_SOURCE_DIR}/src/include)

# 创建可执行文件目标
add_executable(MyExe main.cpp)

# 链接库到可执行文件
target_link_libraries(MyExe PRIVATE MyLib)
```

- **MyProject/tests/**

```cmake
# 查找 GTest 包
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

# 创建测试目标
add_executable(TestMyLib test_main.cpp)

# 链接库和 GTest 到测试目标
target_link_libraries(TestMyLib PRIVATE MyLib ${GTEST_LIBRARIES})
```



### GDB调试

```shell
# -g 程序调试
$ gcc -o Dest main.c -g
# 启动gdb
$ gdb Dest
# 设置参数，启动时传入
$ set args 参数1 参数2 .... ...
# 查看命令行参数
$ show args
# 执行程序
$ r
# 查看代码			（a.c的第10行附近：a.c:10） 
$ l
# 第13行设置断点		（a.c中：a.c:13）
$ b 13
# 第20行设置条件断点
$ b 20 if i==5
# 查看断点
$ i b
# 删除前两个断点
$ d 1-2
# 设置 3 4 断点无效
$ dis 3 4
# 生效 3 断点
$ ena 3
# 继续执行
$ c
# 打印变量值
$ p i
# 打印变量类型
$ ptype array
# 自动显示变量 a[i]
$ display a[i]
# 查看自动显示列表
$ i display
# 删除 2-4 自动显示
$ undisplay 2-4
# 禁用 1 自动显示
$ dis display 1
# 当前 阻塞至函数1前一行，单步调试进入函数
$ s
# 函数内无有效断点，跳出
$ finish
# 当前 阻塞至函数2前一行，单步调试跳过函数
$ n
# 当前 刚进入循环体，在结束行执行跳出
$ until
# 当前 刚进入两位数循环，设置变量值实现跳出
$ set var i=100
# 退出gdb
$ q
```



## 文件IO

Linux中一切皆文件

**虚拟地址空间**

运行磁盘上的可执行程序会产生进程，内核为每个 **进程** 创建专属虚拟地址空间，并将程序数据载入对应地址 。

- 大小由OS决定，32位的为 2^32 B，即 4G

- 进程数据经 CPU 中 MMU 从虚拟地址空间映射到物理内存

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/virtual_add_space.png" style="zoom:70%;" />

- **保留区**：位于最底部，未赋予物理地址，任何对其引用均非法，程序中空指针所指的地址
- **堆**：存放进程运行时动态分配的内存，内容匿名，只能通过指针间址，向上生长，不连续
- **内存映射区**：加载磁盘文件或程序运行所需动态库
- **栈**：存储局部变量、函参，向下生长，连续
- **命令行参数**：存储进程执行时传递给 `main()` 的参数，argc，argv[]
- **环境变量**：存储和进程相关的环境变量, 如: 工作路径, 进程所有者等信息

**文件描述符**

fd：进程打开或新建文件时，内核返回对应文件描述符。

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/fd_table.png" style="zoom:70%;" />

- 终端是设备文件，当前终端可用 `/dev/tty` 表示
- 每个进程 fd 表的文件打开上限默认为 1024，多 fd 可共用同一磁盘文件
- 进程启动时，内核 PCB fd表预分配 3 个指向启动终端的fd：
  1. `STDIN_FILENO`：标准输入，通过fd向终端文件输入数据，宏值为0
  2. `STDOUT_FILENO`：标准输出，通过fd由终端文件输出数据，宏值为1
  3. `STDERR_FILENO`：标准错误，通过fd由终端文件输出错误信息，宏值为2



## 进程

程序是磁盘可执行文件，进程是其执行实例，是资源分配的最小单位。

### 进程控制

**PCB**

进程控制块，本质是内核 `task_struct` 结构体，记录进程运行相关信息：

- 进程id：唯一的 `pid_t` 类型的进程ID
- 进程状态：就绪、运行、阻塞等
- 进程对应的虚拟地址空间的信息
- 绑定启动终端的信息
- 当前工作目录：默认启动进程的目录
- umask掩码：创建新文件时用于屏蔽操作权限
- fd表：每个 fd 对应一个已打开的磁盘文件
- 和信号相关的信息：函数调用/快捷键/shell命令等操作会产生信号
- 阻塞信号集：记录阻塞当前进程已产生的信号
- 未决信号集：记录进程中未处理的信号
- 用户id和组id：进程所属用户和组
- 会话和进程组：进程组是进程集合，Session 是进程组集合
- 进程可以使用的资源上限：`ulimit -a` 查看详情

`ps aux`			// 查看进程

`kill -9 pid`		// 强制杀死进程

**父子进程**

`fork()` 用于创建子进程

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/fork.png" style="zoom:70%;" />

函数调用成功后，各自的虚拟地址空间中：

- 父进程：返回子进程pid
- 子进程：返回0

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/process_exe_loca.png" style="zoom:70%;" />

父进程成功创建子进程后，子进程拥有父进程代码区所有代码，且从父进程调用 **fork()函数之后** 开始执行。

故以下循环执行了3次，最终得到了 2^3 个进程

```c
for(int i=0; i<3; ++i)
{
    pid_t pid = fork();
    printf("当前进程pid: %d\n", getpid());
}
```

> 分析多进程程序需拆分代码，若无条件控制，父子进程均能执行所有代码

**回收**

子进程退出时，用户区资源自释，内核区PCB资源需父进程释放 

- 启动的进程创建子进程后，若父进程先退出，子进程即为孤儿进程，被系统进程领养
- 父进程未释放先结束子进程的PCB资源，子进程即为僵尸进程，需杀死父进程，杀子无效

### 进程通信

**管道**

本质是内核缓冲区内存，数据存于其中的环形队列，无法直接操作。

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/pipe.png" style="zoom:70%;" />

- 队列大小固定，默认为4k
- 分读写两端，写端进、读端出，管道操作即文件 IO
- 单工，数据从写端流向读端，读数据即出队
- 读写默认阻塞，类似生产者-消费者模型

> 管道通信必须保证数据单向流动，如p1读、p2写

`mkfifo`	//  创建有名管道

**内存映射**

 进程通过映射同一磁盘文件的内存映射区实现数据交互。

**共享内存**

独立于进程，通过系统函数获取，关联后可读写，需信号量同步，效率最高。

`ipcs -m `	// 查看共享内存详情

*Linux中信号是高优先级整数消息机制，多场景触发，通信低效且干扰流程，慎用。*

### 守护进程

Daemon是独立于控制终端、生存期长的后台服务进程，常以 d 结尾，周期性执行任务或等待事件。

- 创建步骤：
  1. 创建子进程并终止父进程，使子进程成为孤儿进程
  2. 子进程调用 `setsid()` 创建新会话，脱离控制终端
  3. 可选：使用 `chdir()` 更改工作目录至根目录或非挂载点
  4. 可选：使用 `umask()` 重设文件权限掩码
  5. 关闭/重定向标准输入输出和错误流（通常指向/dev/null）
  6. 执行守护进程核心逻辑

### 线程

线程是轻量级进程，Linux下本质为进程，是OS调度执行的最小单位。

- 多线程共享地址空间、独享栈区与寄存器（内核管理）， 线程组内可互访栈数据
- 线程更轻量级，上下文切换比进程快的多
- 文件IO线程数2×CPU核数，复杂算法等于核数
- 虚拟地址空间生命周期默认与主线程一致、与子线程无关

**上下文切换** 是进程/线程分时调度时保存并恢复上下文以继续执行的行为



## 套接字通信

套接字是一套网络通信的接口，包含于标头 `<sys/socket.h>` 。

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/socket.png" style="zoom:70%;" />

### 概念

**字节序** 是不同计算机体系中多字节数据的内存存储顺序

- 小端：低位字节存低地址、高位存高地址（PC机默认）  
- 大端：低位字节存高地址、高位存低地址（套接字通信使用）

```c
// case: 0x12345678
                 内存低地址位                内存的高地址位
--------------------------------------------------------------------------->
小端:         0x78        0x56        0x34        0x12
大端:         0x12        0x34        0x56        0x78
```

一些转换接口：

```c
// 大端 <-> 小端
uint16_t htons(uint16_t hostshort);	
uint32_t htonl(uint32_t hostlong);	
uint16_t ntohs(uint16_t netshort);
uint32_t ntohl(uint32_t netlong);

// 点分十进制IP <-> 大端整形
int inet_pton(int af, const char *src, void *dst); 
const char *inet_ntop(int af, const void *src, char *dst, socklen_t size);

// 点分十进制IPv4 <-> 大端整形，windows也适用
in_addr_t inet_addr (const char *cp);
char* inet_ntoa(struct in_addr in);
```

- `inet_pton()`

  - af: 地址族, AF_INET(ipv4), AF_INET6(ipv6)

  - src: 入参, 点分十进制ip: 192.168.1.100

  - dst: 出参, 指向大端整形IP
  - return 1

- `inet_ntop`

  - af: 地址族, AF_INET(ipv4), AF_INET6(ipv6)

  - src: 入参, 指向大端整形IP

  - dst: 出参, 点分十进制ip

  - size: 修饰dst所指内存的最大容量 B
  - return dst

**sockaddr**

```c
// 通用结构体
struct sockaddr {
    unsigned short sa_family;    // 地址族协议，ipv4
    char sa_data[14];           // 端口(2B) + IP地址(4B) + 填充(8B)
};

// IPv4特化
struct sockaddr_in {
    short int sin_family;           // 地址族，AF_INET
    unsigned short int sin_port;    // 端口 -> 大端
    struct in_addr sin_addr;        // IP -> 大端
    unsigned char sin_zero[8];      // 填充
};

struct in_addr {  uint32_t s_addr;  };
```

### Socket

fd关联两块内存，读缓冲区存储待读数据，写缓冲区存储待写数据。

---

用于套接字通信的函数：

```c
int socket(int domain, int type, int protocol);					// 创建套接字
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);		// 绑定fd和ip&port
int listen(int sockfd, int backlog);						// 监听套接字
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);		// 接受连接
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);	// 建立连接
ssize_t read(int sockfd, void *buf, size_t size);				// 接收数据
ssize_t recv(int sockfd, void *buf, size_t size, int flags);
ssize_t write(int fd, const void *buf, size_t len);				// 发送数据
ssize_t send(int fd, const void *buf, size_t len, int flags);
```

- `socket()`
  - domain: 地址族
  - type: 传输协议, SOCK_STREAM(流式), SOCK_DGRAM(报式)
  - protocol: 写0使用默认协议, 流式tcp, 报式udp
  - return fd

- `bind()`
  - sockfd: 监听fd
  - addr: 入参, 待绑的大端IP&port初始化到此结构体
  - addrlen: sizeof(addr)
  - return 0
- `listen()`
  - sockfd: fd
  - backlog: 同时能处理的最大连接要求，最大128
  - return 0
- `accept()`
  - sockfd: 监听的fd
  - addr: 出参, 存储了client's infos
  - addrlen: sizeof(addr)
  - return fd
- `connect()`
  - sockfd: 通信的fd
  - addr: 存储了server's infos
  - addrlen: sizeof(addr)
  - return 0
- `recv()`
  - sockfd: 通信的fd
  - buf: 存储接收的数据
  - size: sizeof(buf)
  - flags: 一般指定 0
  - return +(接收B), 0(对方断开), -1(失败)
- `send()`
  - fd: 通信的fd
  - buf: 入参, 字符串str
  - len: len(str)
  - flags: 一般指定 0
  - return +(发送B), -1(失败)

---

- **tcp server**

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>

int main()
{
    // 1. 创建监听的套接字
    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    if(lfd == -1)
    {
        perror("socket");
        exit(0);
    }

    // 2. 将socket()返回值和本地的IP端口绑定到一起
    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(10000);   // 大端端口
    // INADDR_ANY代表本机的所有IP, 假设有三个网卡就有三个IP地址
    // 这个宏可以代表任意一个IP地址
    // 这个宏一般用于本地的绑定操作
    addr.sin_addr.s_addr = INADDR_ANY;  // 这个宏的值为0 == 0.0.0.0
//    inet_pton(AF_INET, "192.168.237.131", &addr.sin_addr.s_addr);
    int ret = bind(lfd, (struct sockaddr*)&addr, sizeof(addr));
    if(ret == -1)
    {
        perror("bind");
        exit(0);
    }

    // 3. 设置监听
    ret = listen(lfd, 128);
    if(ret == -1)
    {
        perror("listen");
        exit(0);
    }

    // 4. 阻塞等待并接受客户端连接
    struct sockaddr_in cliaddr;
    int clilen = sizeof(cliaddr);
    int cfd = accept(lfd, (struct sockaddr*)&cliaddr, &clilen);
    if(cfd == -1)
    {
        perror("accept");
        exit(0);
    }
    // 打印客户端的地址信息
    char ip[24] = {0};
    printf("客户端的IP地址: %s, 端口: %d\n",
           inet_ntop(AF_INET, &cliaddr.sin_addr.s_addr, ip, sizeof(ip)),
           ntohs(cliaddr.sin_port));

    // 5. 和客户端通信
    while(1)
    {
        // 接收数据
        char buf[1024];
        memset(buf, 0, sizeof(buf));
        int len = read(cfd, buf, sizeof(buf));
        if(len > 0)
        {
            printf("客户端say: %s\n", buf);
            write(cfd, buf, len);
        }
        else if(len  == 0)
        {
            printf("客户端断开了连接...\n");
            break;
        }
        else
        {
            perror("read");
            break;
        }
    }

    close(cfd);
    close(lfd);

    return 0;
}
```

- **tcp client**

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>

int main()
{
    // 1. 创建通信的套接字
    int fd = socket(AF_INET, SOCK_STREAM, 0);
    if(fd == -1)
    {
        perror("socket");
        exit(0);
    }

    // 2. 连接服务器
    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(10000);   // 大端端口
    inet_pton(AF_INET, "192.168.237.131", &addr.sin_addr.s_addr);

    int ret = connect(fd, (struct sockaddr*)&addr, sizeof(addr));
    if(ret == -1)
    {
        perror("connect");
        exit(0);
    }

    // 3. 和服务器端通信
    int number = 0;
    while(1)
    {
        // 发送数据
        char buf[1024];
        sprintf(buf, "你好, 服务器...%d\n", number++);
        write(fd, buf, strlen(buf)+1);
        
        // 接收数据
        memset(buf, 0, sizeof(buf));
        int len = read(fd, buf, sizeof(buf));
        if(len > 0)
        {
            printf("服务器say: %s\n", buf);
        }
        else if(len  == 0)
        {
            printf("服务器断开了连接...\n");
            break;
        }
        else
        {
            perror("read");
            break;
        }
        sleep(1);   // 每隔1s发送一条数据
    }

    close(fd);

    return 0;
}
```



**TCP粘包** 因流式传输无边界导致数据粘连，解决方案是在应用层添加带长度字段的包头以标识消息边界。

### IO多路转接

通过阻塞监测多个fd，就绪时解除阻塞并通信，实现单线程 / 进程服务器并发。

#### 1. select

跨平台，FD 上限 1024，线性轮询，双态高频拷贝。

```c
#include <sys/select.h>
struct timeval {
    time_t      tv_sec;         /* seconds */
    suseconds_t tv_usec;        /* microseconds */
};

int select(int nfds, fd_set *readfds, fd_set *writefds,
           fd_set *exceptfds, struct timeval * timeout);
```

- `select()`
  - nfds：maxfd(set1,set2,set3) + 1，线性遍历的结束条件(win指定-1)
  - readfds：fd_set, 检测 read bufs
  - writefds：fd_set 检测 write bufs, 不需要指定NULL
  - exceptfds：fd_set, 检测 except stat, 不需要指定NULL
  - timeout：超时时长，NULL | t | 0
  - return +(fd总数), 0(超时), -1(失败)


---

`fd_set` 大小是 128 B， 与 fd表 一一对应标记状态，其操作函数：

```c
void FD_CLR(int fd, fd_set *set);		// 删除fd
int  FD_ISSET(int fd, fd_set *set);		// fd是否在set中
void FD_SET(int fd, fd_set *set);		// 添加fd
void FD_ZERO(fd_set *set);				// 清空set
```

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/fd_set_1.png" style="zoom:70%;" />

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/fd_set_2.png" style="zoom:70%;" />

内核遍历读集合时，将无数据的fd在fd_set中标志位置0，有数据则保持1；
select解除阻塞后，标志位为1的描述符就绪可通信

---

**处理流程**

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/select.png" style="zoom:70%;" />

- server

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>

int main()
{
    // 1. 创建监听的fd
    int lfd = socket(AF_INET, SOCK_STREAM, 0);

    // 2. 绑定
    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(9999);
    addr.sin_addr.s_addr = INADDR_ANY;
    bind(lfd, (struct sockaddr*)&addr, sizeof(addr));

    // 3. 设置监听
    listen(lfd, 128);

    // 将监听的fd的状态检测委托给内核检测
    int maxfd = lfd;
    // 初始化检测的读集合
    fd_set rdset;
    fd_set rdtemp;
    // 清零
    FD_ZERO(&rdset);
    // 将监听的lfd设置到检测的读集合中
    FD_SET(lfd, &rdset);
    // 通过select委托内核检测读集合中的文件描述符状态, 检测read缓冲区有没有数据
    // 如果有数据, select解除阻塞返回
    // 应该让内核持续检测
    while(1)
    {
        // 默认阻塞
        // rdset 中是委托内核检测的所有的文件描述符
        rdtemp = rdset;
        int num = select(maxfd+1, &rdtemp, NULL, NULL, NULL);
        // rdset中的数据被内核改写了, 只保留了发生变化的文件描述的标志位上的1, 没变化的改为0
        // 只要rdset中的fd对应的标志位为1 -> 缓冲区有数据了
        // 判断
        // 有没有新连接
        if(FD_ISSET(lfd, &rdtemp))
        {
            // 接受连接请求, 这个调用不阻塞
            struct sockaddr_in cliaddr;
            int cliLen = sizeof(cliaddr);
            int cfd = accept(lfd, (struct sockaddr*)&cliaddr, &cliLen);

            // 得到了有效的文件描述符
            // 通信的文件描述符添加到读集合
            // 在下一轮select检测的时候, 就能得到缓冲区的状态
            FD_SET(cfd, &rdset);
            // 重置最大的文件描述符
            maxfd = cfd > maxfd ? cfd : maxfd;
        }

        // 没有新连接, 通信
        for(int i=0; i<maxfd+1; ++i)
        {
			// 判断从监听的文件描述符之后到maxfd这个范围内的文件描述符是否读缓冲区有数据
            if(i != lfd && FD_ISSET(i, &rdtemp))
            {
                // 接收数据
                char buf[10] = {0};
                // 一次只能接收10个字节, 客户端一次发送100个字节
                // 一次是接收不完的, 文件描述符对应的读缓冲区中还有数据
                // 下一轮select检测的时候, 内核还会标记这个文件描述符缓冲区有数据 -> 再读一次
                // 	循环会一直持续, 知道缓冲区数据被读完位置
                int len = read(i, buf, sizeof(buf));
                if(len == 0)
                {
                    printf("客户端关闭了连接...\n");
                    // 将检测的文件描述符从读集合中删除
                    FD_CLR(i, &rdset);
                    close(i);
                }
                else if(len > 0)
                {
                    // 收到了数据
                    // 发送数据
                    write(i, buf, strlen(buf)+1);
                }
                else
                {
                    // 异常
                    perror("read");
                }
            }
        }
    }

    return 0;
}
```

#### 2. poll

Linux平台，线性轮询，双态高频拷贝。

```c
#include <poll.h>
// 每个委托poll检测的fd都对应这样一个结构体
struct pollfd {
    int   fd;         /* 委托内核检测的文件描述符 */
    short events;     /* 委托内核检测文件描述符的什么事件 */
    short revents;    /* 文件描述符实际发生的事件 -> 传出 */
};

struct pollfd myfd[100];
int poll(struct pollfd *fds, nfds_t nfds, int timeout);
```

#### 3. epoll

Linux平台，红黑树管理，事件回调，共享内存。

```c
#include <sys/epoll.h>
int epoll_create(int size);		// 创建epoll实例
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);	// 管理fd(增删改)
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);	// 检测就绪fd
```

- `epoll_create()`
  - return fd
- `epoll_ctl()`
  - epfd：ep fd
  - op：操作类型, EPOLL_CTL_ADD(增), EPOLL_CTL_DEL(删), EPOLL_CTL_MOD(改)
  - fd：目标fd
  - event：epoll事件
    - .events：委托epoll检测的事件, EPOLLIN(读), EPOLLOUT(写), EPOLLERR(异常)
    - .data：user data var, 使用.fd存储待检测的fd
  - return 0
- `epoll_wait()`
  - epfd：ep fd
  - events：出参, 已就绪 fd's epoll_event array
  - maxevents：len(events)
  - timeout：阻塞时长
  - return +(fd总数), 0(无), -1(失败)

---

**server**

```c
#include <stdio.h>
#include <ctype.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
#include <arpa/inet.h>
#include <sys/socket.h>
#include <sys/epoll.h>

int main(int argc, const char* argv[])
{
    // 创建监听的套接字
    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    if(lfd == -1)
    {
        perror("socket error");
        exit(1);
    }

    // 绑定
    struct sockaddr_in serv_addr;
    memset(&serv_addr, 0, sizeof(serv_addr));
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(9999);
    serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);  // 本地多有的ＩＰ
    
    // 设置端口复用
    int opt = 1;
    setsockopt(lfd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt));

    // 绑定端口
    int ret = bind(lfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));
    if(ret == -1)
    {
        perror("bind error");
        exit(1);
    }

    // 监听
    ret = listen(lfd, 64);
    if(ret == -1)
    {
        perror("listen error");
        exit(1);
    }

    // 现在只有监听的文件描述符
    // 所有的文件描述符对应读写缓冲区状态都是委托内核进行检测的epoll
    // 创建一个epoll模型
    int epfd = epoll_create(100);
    if(epfd == -1)
    {
        perror("epoll_create");
        exit(0);
    }

    // 往epoll实例中添加需要检测的节点, 现在只有监听的文件描述符
    struct epoll_event ev;
    ev.events = EPOLLIN;    // 检测lfd读读缓冲区是否有数据
    ev.data.fd = lfd;
    ret = epoll_ctl(epfd, EPOLL_CTL_ADD, lfd, &ev);
    if(ret == -1)
    {
        perror("epoll_ctl");
        exit(0);
    }

    struct epoll_event evs[1024];
    int size = sizeof(evs) / sizeof(struct epoll_event);
    // 持续检测
    while(1)
    {
        // 调用一次, 检测一次
        int num = epoll_wait(epfd, evs, size, -1);
        for(int i=0; i<num; ++i)
        {
            // 取出当前的文件描述符
            int curfd = evs[i].data.fd;
            // 判断这个文件描述符是不是用于监听的
            if(curfd == lfd)
            {
                // 建立新的连接
                int cfd = accept(curfd, NULL, NULL);
                // 新得到的文件描述符添加到epoll模型中, 下一轮循环的时候就可以被检测了
                ev.events = EPOLLIN;    // 读缓冲区是否有数据
                ev.data.fd = cfd;
                ret = epoll_ctl(epfd, EPOLL_CTL_ADD, cfd, &ev);
                if(ret == -1)
                {
                    perror("epoll_ctl-accept");
                    exit(0);
                }
            }
            else
            {
                // 处理通信的文件描述符
                // 接收数据
                char buf[1024];
                memset(buf, 0, sizeof(buf));
                int len = recv(curfd, buf, sizeof(buf), 0);
                if(len == 0)
                {
                    printf("客户端已经断开了连接\n");
                    // 将这个文件描述符从epoll模型中删除
                    epoll_ctl(epfd, EPOLL_CTL_DEL, curfd, NULL);
                    close(curfd);
                }
                else if(len > 0)
                {
                    printf("客户端say: %s\n", buf);
                    send(curfd, buf, len, 0);
                }
                else
                {
                    perror("recv");
                    exit(0);
                } 
            }
        }
    }

    return 0;
}
```

**epoll 事件触发** 分为 LT 和 ET 两种模式：
LT 是默认模式，会持续通知事件；ET 模式需要搭配 EPOLLET 标志和非阻塞 IO，仅在状态变化时触发一次，需要循环读写至出现 EAGAIN 错误，效率更高。
