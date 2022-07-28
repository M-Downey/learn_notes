### Linux学习笔记

#### 腾讯云服务器

1. 创建一个新实例，如果是`ubuntu`，则默认用户是`ubuntu`，`web shell`用户是`lighthouse`
2. 创建一个新实例，要在控制台重置`ubuntu`的密码，然后登录后，也可以改`lighthouse`的密码

```shell
# 设置某用户的密码
sudo passwd user_name
```

3. `ssh`登录服务器，如果出现`host key for xxx has changed`错误

[解决方法：](https://blog.csdn.net/duanbiren123/article/details/80836408)

```shell
# 在本机上
# 修改了~/.ssh/known_hosts
ssh-keygen -R Ip
```

4. `ssh`免密登录服务器

```shell
# 需要密码的
ssh user_name@Ip
# ssh免密登录，要先在服务器对应用户下添加公钥
# 在本机下
ssh-copy-id -i ~/.ssh/id_rsa.pub user_name@IP
```

5. `ssh config`方便`ssh`登录

```shell
# 为了不用每次登录服务器输入ip，端口转发等信息
# 配置~/.ssh/config(用户)
# 系统的在/etc/ssh/下
# 教程：https://blog.csdn.net/senlin1202/article/details/122081089

# ssh ubuntu即可登录
Host ubuntu
  HostName 124.223.204.233
  User ubuntu
  IdentityFile ~/.ssh/id_rsa
  LocalForward 8999 localhost:8999（服务器端口）
```

5. 创建用户

```shell
# 创建了家目录的
sudo adduser user_name
# 不创建家目录的
sudo useradd user_name
```

6. 删除用户

```shell
# 删除家目录的
sudo deluser --remove-home user_name
# 不删除家目录的
sudo userdel user_name
```

#### `Conda`

1. 默认不激活`conda`环境

```shell
conda config --set auto_activate_base false
```

2. `Jupyter`能切换`conda`环境，[教程](https://zhuanlan.zhihu.com/p/139776843)

```shell
conda install nb_conda
# 每个环境都安装jupyter notebook
# 重启jupyter
```

#### `Jupyter`

1. 服务器的`jupyter notebook`在本机浏览器上跑，[教程](https://www.jianshu.com/p/505a97a9fcec)

在`xshell`中，需要设置端口映射

> 在会话属性中，选隧道，添加一个本地端口，远程主机是服务器`IP`，端口是`jupyter`的端口
>
> 下面的x11转发要取消勾

用`ssh`登录时，可直接端口映射

```shell
ssh -p 22 -L 8888:127.0.0.1:8888(服务器端口) user_name@IP
```

2. 配置`jupyter`的`config`，[教程](http://blog.iis7.com/article/28231.html)

```python
from notebook.auth import passwd
passwd()
# 然后复制输出到.jupyter/jupyter_notebook_config.py里
# vi .jupyter/jupyter_notebook_config.py
# 添加如下内容
# c.NotebookApp.ip = '*'
# c.NotebookApp.password = u'argon2:$argon2id$v=19$m=10240,t=10,p=8$ls7Kg'
# c.NotebookApp.port = 9820  ##这个端口和Xshell里的目标端口相同
# c.InteractiveShellApp.matplotlib = 'inline'
# c.NotebookApp.open_browser = False
```

#### Linux笔记

##### 一. 初识Linux

1. Linux可划分为四部分：
   - Linux内核
   - GNU工具
   - 图形化桌面环境
   - 应用软件
2. Bash shell
   1. shell是GNU中的一种特殊的交互式工具
   2. bash shell是源于Unix的标准shell，Bourne shell而来，是Bourne again shell的缩写

##### 三. 基本的bash shell命令

1. `/etc/passwd`

   在其中每行最后一个字段，查看每个用户的默认shell

2. man手册

   ```shell
   # 查询某命令的手册
   man ls
   man man
   # 查找与关键字有关的命令
   man -k terminal
   # man section# topic
   man 1 hostname
   man 7 hostname
   ```

3. help

   大多数命令都接受`-help`，`--help`的参数

4. 常见目录及用途

   | 目录         | 用途                                                |
   | ------------ | --------------------------------------------------- |
   | /            | 虚拟目录的根目录                                    |
   | /bin         | 二进制目录，存放用户级的GNU工具                     |
   | /sbin        | 系统二进制目录，存放GNU管理员级工具                 |
   | /opt         | 可选目录，存放第三方软件包和数据文件                |
   | /dev         | 设备目录，在这里创建设备节点                        |
   | /usr         | 用户二进制目录，用户及的GNU工具和数据文件存储在这里 |
   | /etc         | 系统配置文件目录                                    |
   | /mnt，/media | 挂载目录，可移动媒体设备的常用挂载点                |
   | /home        | 主目录，Linux在这里创建用户目录                     |

5. `ls`

   > -F：区别显示文件夹/，文件，可执行文件*
   >
   > -a：显示所有文件
   >
   > -R：递归选项，显示子目录下的文件
   >
   > -l：显示长列表
   >
   > -i：显示文件inode编号
   >
   > 正则过滤输出：
   >
   > - ?：代表一个字符
   > - *：代表0个或多个字符
   > - [a-z]：代表所有字母
   > - !：将不需要的内容排除在外

6. 处理文件

   > 1. touch
   >
   >    **创建一个空文件**或者**改变修改时间**
   >
   >    -a：改变访问时间
   >
   > 2. cp
   >
   >    复制文件
   >
   >    -i：检查是否会覆盖
   >
   > 3. 链接命令ln
   >
   >    1. 符号链接
   >
   >       ln -s source_file dest_file
   >
   >       dest_file是一个指向source_file的指针，内容会同步变化
   >
   >    2. 硬链接
   >
   >       ln source_file dest_file
   >
   >       dest_file和source_file一样，内容同步变化
   >
   > 4. mv
   >
   >    **改名**或**移动文件位置**
   >
   >    -i：确定是否覆盖
   >
   > 5. rm
   >
   >    -i：确认
   >
   >    -r：递归
   >
   >    -f：强制

7. 处理目录

   > 1. `mkdir`
   >
   >    创建目录
   >
   >    -p：创建缺失的父目录
   >
   >    ```shell
   >    mkdir dir1/dir2/dir3
   >    ```
   >
   > 2. `rm`
   >
   >    删除目录
   >
   >    -r：递归删除
   >
   >    -f：强制
   >
   >    -rf：终极大法

8. 查看文件内容

   > 1. `file`
   >
   >    查看文件类型，可以文本编码，目录，链接文件，shell脚本
   >
   > 2. `cat`
   >    -n：添加行号
   >
   >    -b：只给有文本的行加上行号
   >
   >    -T：将制表符用^I代替
   >
   > 3. `more`
   >
   >    显示一页数据就停下来
   >
   > 4. `less`
   >
   >    `more`的升级版
   >
   > 5. `tail`
   >
   >    查看文件尾部内容，默认10行
   >
   >    -n：改变行数
   >
   >    -f：文件被使用时，也能显示内容，可以实时查看内容变化
   >
   > 6. `head`
   >
   >    查看文件头的内容默认10行
   >
   >    -n：行数

##### 四. 更多的bash shell命令

1. 监测程序

   > 1. `ps`
   >
   >    显示进程信息
   >
   >    -e：显示所有进程
   >
   >    -f：显示完整格式输出	
   >
   >    --forest：显示进程间关系
   >
   > 2. `top`
   >
   >    实时显示进程信息
   >
   > 3. `kill`
   >
   >    用`PID`终止进程
   >
   >    -s：支持指定其他信号
   >
   > 4. `killall`
   >
   >    支持通过进程名结束进程，也支持通配符

2. 监测磁盘空间

   > 1. `mount`
   >
   >    用来挂载媒体，默认输出当前系统上挂载的设备列表
   >
   >    ```shell
   >    mount -t type device directory
   >    
   >    # eg:
   >    mount -t vfat /dev/sdb1 /media/disk
   >    # 手动将U盘/dev/sdb1挂载到/media/disk
   >    ```
   >
   > 2. `umount`
   >
   >    移除一个可移动设备时，要先卸载
   >
   >    支持通过设备文件或挂载点来指定要卸载的设备
   >
   > 3. `df`
   >
   >    查看所有已挂载磁盘的使用情况
   >
   >    -h：转换成用户易读的形式显示
   >
   > 4. `du`
   >
   >    显示当前目录磁盘使用情况
   >
   >    -h：转换成用户易读的形式显示

3. 处理数据文件

   > 1. `sort`
   >
   >    对文本文件内容按字典序排序
   >
   >    -n：把文件内字符当成数字排序
   >
   >    -M：按月排序
   >
   >    -t：指定分隔符
   >
   >    -k：按第几个字段排序（从1开始）
   >
   >    -r：降序
   >
   >    ```shell
   >    #
   >    sort -t ':' -k 3 -n /etc/passwd
   >    #
   >    du -sh * | sort -nr
   >    ```
   >
   > 2. `grep`
   >
   >    在文本中找一行数据
   >
   >    `grep [options] pattern [file]`
   >
   >    -v：输出不匹配该模式的行
   >
   >    -n：显示行号
   >
   >    -c：显示有多少行匹配的
   >
   >    -e：指定多个匹配模式
   >
   >    [tf]：输出包含t或f的行
   >
   > 3. 压缩数据
   >
   >    `bzip`，文件扩展名`.bz2`
   >
   >    `compress`，文件扩展名，`.Z`
   >
   >    `gzip`，文件扩展名`.gz`
   >
   >    `zip`，文件扩展名`zip`
   >
   > 4. `tar`
   >
   >    `Unix`和`Linux`上最广泛使用的归档工具
   >
   >    `tar function [options] obj1 obj2 ...`
   >
   >    function常用：
   >
   >    -c：--create，创建一个新的tar归档文件
   >
   >    -t：--list，列出所有tar归档文件的内容
   >
   >    -x：--extract，从已有tar归档文件中提取文件
   >
   >    options常用：
   >
   >    -f：file，输出结果到文件或设备file
   >
   >    -v：在处理文件时显示文件
   >
   >    ```shell
   >    # 归档文件
   >    tar -cvf test.tar test1/ test2
   >    # 列出tar文件的内容
   >    tar -tf test.tar
   >    # 从tar文件中提取内容
   >    tar -xvf test.tar
   >       
   >    # 解压.tgz
   >    tar -zxvf filename.tgz
   >    ```

##### 五. 理解shell

1. shell的类型

   > 1. shell有两种：用户交互型shell和系统shell
   >
   > 用户交互型：每个用户自己启动时用的shell
   >
   > 系统：用于那些需要在启动时使用的系统shell脚本

2. shell的父子关系

   > 1. 在CLI提示符输入bash命令，会创建一个子shell，再输入会再创建一个，会嵌套。（可用ps --forest查看，或者PID和PPID）
   >
   > 2. 创建子shell，只有部分父进程的环境被复制到子shell环境中
   >
   > 3. bash
   >
   >    -c string：从string中读取命令并进行处理
   >
   >    -s：从标准输入中读取命令
   >
   > 4. exit：有条不紊地退出子shell

3. 进程列表

   > 1. **命令列表**，一行用;隔开的命令，可以一起执行
   >
   >    ```shell
   >    ls ; pwd ; cd
   >    ```
   >
   > 2. **进程列表**：用（）括起来的命令列表，是创建子shell并在子shell中执行的，可以用$ZSH_SUBSHELL查看（数字大小代表是第几个子shell）
   >
   >    ```shell
   >    (ls; pwd; echo %ZSH_SUBSHELL)
   >    ```
   >
   > 3. **后台模式：**命令后加&，在后台运行，还能继续使用CLI；在进程列表后加上&也可以
   >
   > 4. `jobs`
   >
   >    查看后台作业信息
   >
   >    -l：作业当前状态以及命令信息
   >
   > 5. 协程：coproc，可以同时做两件事，它在后台生成shell，并在子shell中执行命令
   >
   >    ```shell
   >    coproc sleep 10
   >    
   >    # 自定义进程名，后面花括号要有空格和;
   >    coproc My_Job { sleep 10; }
   >    ```

4. 内建命令与外部命令

   > 1. **外部命令：**也叫文件系统命令，是存在与bash shell之外的程序**（运行时会创建一个子进程，叫衍生）**，常位于`/bin`，`/usr/bin`，`/sbin`，`/usr/sbin`中
   >
   > 2. 使用`which`，`type -a`查看命令路径
   >
   > 3. **内建命令：**不需要子进程来执行，已经和shell编译成了一体。可以用type来确认某命令的实现
   >
   > 4. `history`
   >
   >    命令历史记录，只有在退出终端的时候才会更新
   >
   >    -a：直接往~/.zsh_history中更新历史命令
   >
   > 5. `alilas`
   >
   >    命令别名
   >
   >    -L：查看已有别名
   >
   >    alias li='ls -li'
   >
   >    **注意：**一个命令别名只有在本shell中有用，在子shell中无用

##### 六. 使用Linux环境变量

1. 什么是环境变量
   - 用于存储有关**shell会话**和**工作环境**的信息，允许你在内存中存储数据，以便程序或shell中运行的脚本能够轻松访问到它们。这是存储持久数据的一种简便方法。
   - 种类有：**全局变量**和**局部变量**
     - 全局变量：对所有shell可见
     - 局部变量：对创建它的本shell可见
   - 查看命令：
     - `env`：所有全局变量
     - `printenv HOME`：可以看全局变量以及某个具体的全局变量
     - `set`：所有全局变量，局部变量，用户自定义变量
2. 设置环境变量
   - 设置**局部环境变量**，variable=value即可，含空格的字符串要加引号
   - 设置**全局环境变量**，
