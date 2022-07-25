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

