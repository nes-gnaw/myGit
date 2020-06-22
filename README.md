# mygit

### Git是什么？

Git是目前世界上最先进的分布式版本控制系统（没有之一）。



Linus在1991年创建了开源的Linux，从此，Linux系统不断发展，已经成为最大的服务器系统软件了。

Linus花了两周时间自己用C写了一个分布式版本控制系统，这就是Git！



### 集中式版本控制系统

版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。



### 分布式版本控制系统

根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。



安装git后，配置机器信息

```
# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

### 配置

```
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]
```

### 创建本地仓库

```
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]

# 查看隐藏文件
$ ls -ah
```

### 提交

```
# 添加文件到暂存区
$ git add [file1][file2]
$ git add .

# 提交到暂存区
$ git commit -m "message"

# 提交到远程仓库
$ git push

# 查看仓库状态
$ git status

# 查看修改的内容
$ git diff
```

### 回退版本

```
# 查看git历史
$ git log
$ git log --pretty=oneline

# 回退版本
$ git reset --hard HEAD^
$ git reset --hard HEAD^^
$ git reset --hard HEAD~n

# 记录你的每一次命令
$ git reflog
$ git reset --hard commit_id
```

### 撤销内容

```
# 丢弃工作区内容
$ git checkout -- file

# 添加到了暂存区时，想丢弃修改
$ git reset HEAD [file]
$ git checkout -- file
```

### 删除

```
# 删除本地
$ rm [file]

# 删除git
$ git rm [file]
$ git commit -m "msg"
```

