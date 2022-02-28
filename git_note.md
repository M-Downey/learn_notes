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
```
```
# 配置用户的作者信息
# Linux应该在~目录下，Windows在/c/user/lenovo下

git config --global ...

# 配置系统的作者信息
# 在/etc/gitconfig中

git config --system ...

# 优先级：当前 > 用户 > 系统
```
### 查看配置文件
```
# 打开配置文件
git config -e
git config -e --global
git config -e --system
```
### git基本操作
1. #### `git add file`
    
    1. `git add file`将file存到暂存区
    
    2. `git add all`可以提交未跟踪，修改，删除文件
    
    3. `git add .`可以提交未跟踪，修改，**但是不能处理删除文件**
    
    4. `git add *`不会忽略.gitignore中的文件
2. #### `git commit file -m "message"`
3. #### `git status`

    查看工作区、暂存区的状态
4. #### `git rm --cached file`
    
    将file从暂存区删除
5. #### `git restore file`

    将file在暂存区的状态替换工作区的状态（回溯）
6. #### `git checkout`

    将
7. #### `git log`
    
    1. #### 查看日志
    
       1. `git log`

       2. `git log --pretty=oneline`

       3. `git log --oneline`

       4. `git reflog`（还包括版本重置操作）
8.  #### `git reset`

    1. #### 版本重置

       1. `git reset --hard 索引值`重置到对应索引值版本
       2. `git reset --hard HEAD^^^`几个^就回退几个版本
       3. `git reset --hard HEAD~n`回退n个版本

    2. #### 参数对比：
       1. --soft 仅仅移动本地库的指针，不动工作区和暂存区

       2. --mixed 移动本地库的指针，重置暂存区（和库中版本一样）
    
       3. --hard 移动本地库的指针，重置暂存区，工作区 
9.  #### `git diff`

    1. #### 文件比较

       1. `git diff file`

            比较工作区和暂存区的区别
       2. `git diff HEAD file`

            比较暂存区和版本库的区别

### 分支
1. #### `git branch`

    1. `git branch name`创建分支
    2. `git branch -v`查看分支
    3. `git branch checkout name`切换分支
    4. `git merge`合并分支

        1. `git checkout to_branch_name`要先切换到要合并的分支上
        2. `git merge from_branch_name`执行merge命令 
2. #### 分支冲突

    1. 冲突的标志：

        ```
        <<<<<<
        confict_content
        ======
        confict_content
        >>>>>>
        ```
    2. 手工修改好冲突
    3. `git add conflict_file`
    4. `git commit`

        **注意不能加文件名**