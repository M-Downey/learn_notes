### git问题总结

> 1. `git pull`出现443
>
> 百度搜的大部分是让删掉`～/.gitconfig`中的http下的内容
>
> 我的解决是，把`vpn`关了
>
> 2. `ssh`免密
>
> 免密`git clone/push/pull`需要基于`git`协议
>
> 有一个仓库最初clone用的是`https`协议，所以无法免密使用，需要更改仓库地址为
>
> ```shell
> git remote set-url origin git@github.com:zhangsan/shuofa.git
> ```
>
> 3. zsh更新一直失败
>
> 也是提示443
>
> ```shell
> git remote -v
> ```
>
> 查看发现远程仓库也是`https`，照着2修改后即可
>
> ```shell
> git remote set-url origin git@github.com:ohmyzsh/ohmyzsh.git
> ```