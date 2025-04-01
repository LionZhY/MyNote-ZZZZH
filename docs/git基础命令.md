## git基础命令

初始化本地仓库

`git init`



添加文件到暂存区

`git add <file name>`

暂存区提交到仓库

`git commit -m “提交信息”`



列出本地分支

`git branch`

列出远程分支

`git branch -r`

列出本地和远程分支

`git branch -a`

新建分支

`git branch <新分支名称>`

新建分支并切换到该分支

`git branch -b <新分支名称>`

切换到上一个分支

`git checkout`

切换到指定分支

`git checkout <指定分支名称>`

合并指定分支到当前分支

`git merge <分支名称>`

删除分支

`git branch -d <分支名称>`



clone远程仓库

`git clone <远程仓库url>`

显示远程仓库

`git remote -v`

本地连接远程

`git remote add <远程仓库名称，默认origin> <远程仓库URL>`



推送到远程

`git push <远程仓库名称><分支>`

拉取到本地

`git pull <远程仓库名称><分支>`



查看状态

`git status`

查看日志

`git log`

列出标签

`git tag`



配置git用户名和邮箱

```shell
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

