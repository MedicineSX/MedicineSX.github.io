# Git学习总结（上）

### 安装Git

* Debian & Ubuntu: `sudo apt-get install git`或`sudo apt install git`
* 其他的Linux: 通过源码，解压源码，依次输入`./config`，`make`，`sudo make install`.
* Windows: [Git官网下载传送门](https://git-scm.com/downloads)
* Mac OS: [通过homebrew安装](http://brew.sh/) 或 [百度"通过Xcode安装"](https://www.baidu.com)

安装完成后还需自报家门，在命令行输入：

```
$ git config --global user.name "your name"	//双引号不用写
$ git config --global user.email "email@example.com"	//双引号不用写
```

### 创建版本库

1. 初始化一个仓库，在要初始化仓库的文件夹中进行：

```
$ git init
```

2. 添加文件到Git仓库，分两步：

```
$ git add <file1> <file2> <...>	//添加到暂存区
$ git commit -m <message>	//加入到分支，并为本次提交进行描述
```

### 基础查阅

* 在进行一定操作后，可使用`git status`查看仓库当前状态。

```
$ git status
```

* 修改文件后，若忘记修改内容，可使用`git diff`命令，对比**当前分支中文件**与**当前工作区文件**的不同：

```
$ git diff <file>
```

### 版本退回

* 修改了很多次后，想回到以前的某个版本，可根据每次提交时的描述找回：

```
$ git log	//显示最近到最远的提交日志
$ git log --pretty=oneline	//只显示每次的commit_id和提交描述
$ git reset --hard HEAD^	//回退到上一个提交版本
$ git reset --hard commit_id	//回退到commit_id指向的那个版本，包括上一步已回退的版本
$ git reflog	//查阅每次命令的记录
```

> * 在`git reset --hard HEAD^`命令中，`HEAD^`为上个版本，`HEAD^^`为上上个版本，`HEAD~N`为上N个版本。
> * 在`git reset --hard commit_id`中，`commit_id`只需写前几位，Git会自动查找。
> * HEAD指向的版本就是当前版本，在回退版本时，Git仅仅是把HEAD指向改向指定的版本。

### 工作区与缓存区

在初始化版本库时，使用的目录就是工作区；该目录下`.git`目录中的stage（或者index）就是暂存区。`git add`是把要提交的所有修改放到暂存区；`git commit`是一次性把暂存区的所有修改提交到分支。

> 注意：`git commit`只会把暂存区的所有修改提交到分支！

### 撤销修改

```
/*
	撤销修改的类型：
	1. 文件修改后还未放入暂存区，撤销修改后，工作区文件回到版本库相同的状态
	2. 文件修改后已添加到暂存区，撤销修改后，git add命令被撤销，执行git commit无效
*/
git checkout -- <file>	//用版本库中的版本替换工作区的版本
git reset HEAD <file>	//把暂存区的修改撤销掉，重新放回工作区
```

### 删除文件

```
$ rm <file>	//删除源文件，接下来有两个选择
$ git rm <file> && git commit -m <message>	//从版本库中删除该文件
	或 git checkout -- <file>	//从版本库中恢复
```

### 添加远程仓库(GitHub)

1. 创建SSH Key：创建后，位置在`~/.ssh`。`~/.ssh`中，`id_rsa`是私钥，不可泄露；`id_rsa.pub`是公钥，可告诉任何人。

```
$ ssh-keygen -t rsa -C "myemail@example.com"	//C大写
```

2. GitHub添加SSH Key。"GitHub" -> "Account setting" -> "SSH Keys" -> "Add SSH Key"。`title`任意填写，文本框中填写`id_rsa.pub`中的内容。
3. 添加远程库，在GitHub中添加`repo`，尽量与本地仓库同名。

```
$ cd <localreponame>
/*
	把本地仓库的内容推送到GitHub仓库
	1. 下面的命令中，origin就是远程库的名称，是Git默认的叫法，可更改。
	2. githubidname指GitHub ID的名称
	3. reponame指制定GitHub ID上仓库的名称
*/
$ git remote add origin git@github.com:githubidname/reponame.git	//关联远程库
/* eg: git remote add origin git@github.com:MedicineSX/MedicineSX.github.io.git */

$ git push -u origin master	//关联后，第一次推送master分支所有内容到远程，-u关联master分支
$ git push origin master	//本地文件有修改时，把本地最新修改推送到远程
```

### 从远程仓库克隆

```
$ git clone git@github.com:githubidname/reponame.git	//克隆远程仓库
/* eg: git clone git@github.com:MedicineSX/MedicineSX.github.io.git */
```

### 创建与合并分支

```
$ git switch -c <branchname> 或 git checkout -b <branchname>	//创建并切换分支，一般使用第一个
$ git branch	//列出所有分支，当前分支前有*号
$ git switch master 或 git checkout master	//切换到master分支
$ git merge <branchname>	//合并指定分支到当前分支，fast forward合并，看不出来曾经做过合并
$ git branch -d <branchname>	//删除分支
```

### 解决分支冲突

* Git无法自动合并分支时，应先解决冲突，再提交，合并完成。
* 解决冲突就是把Git合并失败的文件手动编辑为我们所希望的文档，再提交。
* `git log --graph`可以看到分支的合并图。

### 分支管理策略

```
$ git switch -c <branchname>
$ vim <file>
$ git add <file>
$ git commit -m "message"
$ git switch master
$ git merge --no-ff -m "message" <branchname>	//禁用fast forward，保留分支信息
```















