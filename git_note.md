# git学习笔记
### 初始化一个本地仓库
```
git init
```
### 配置作者信息
```
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
```
### git基本操作
1. `git add file`
    
    - `git add file`将file存到暂存区
    
    - `git add all`可以提交未跟踪，修改，删除文件
    
    - `git add .`可以提交未跟踪，修改，**但是不能处理删除文件**
    
    - `git add *`不会忽略.gitignore中的文件
2. `git commit file -m "message"`
3. `git status`

    查看工作区、暂存区的状态
4. `git rm --cached file`
    
    将file从暂存区删除
5. `git restore file`

    将file在暂存区的状态替换工作区的状态（回溯）
6. `git checkout`