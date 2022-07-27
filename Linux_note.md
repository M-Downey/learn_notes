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
