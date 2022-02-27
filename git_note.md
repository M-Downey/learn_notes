# git学习笔记
### 初始化一个本地仓库
    git init
### 配置作者信息
    # 配置当前项目的作者信息 
    # 配置信息在.git/config中
    git config user.name tony
    git config user.eamil 92394058@qq.com
    
    # 配置用户的作者信息
    # Linux应该在~目录下，Windows在/c/user/lenovo下
    git config --global ...
    
    # 配置系统的作者信息
    # 在/etc/gitconfig中
    git config --system ...
    
    # 优先级：
    # 当前 > 用户 > 系统

    # 打开配置文件
    git config -e
    git config -e --global
    git config -e --system