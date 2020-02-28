# Git学习总结（二）

### `git stash`运用

当手头工作没有完成，可把工作现场“隐藏”，新建分支来修复bug，修复完成后再回到工作。若要使修复的bug同步到工作分支上，可以使用`git cherry-pick <commit_id>`，把bug“复制”到当前分支。
```
$ git status	//在当前分支上，有未提交的文件
$ git stash    //将当前分支“储藏”起来
$ git status    //结果显示当前工作已完成
$ git switch -c bug	//新建bug分支
……	//修复bug，提交，切换到master分支
$ git merge --no-ff -m "message" bug
$ git branch -d bug	//删除bug分支，记下bug分支的<commit_id>，此处记作<bug_id>
……	//切换到工作分支
$ git cherry-pick <bug_id>	//将bug“复制”到工作分支
$ git stash pop	//恢复工作，解决冲突
	或 git stash apply && git stash drop
```
### 