### 初始化一个项目

1. 建立本地仓库
```
git init
```
2. 将文件添加到本地仓库（或者叫暂存区？）
```
git add .
```
3. 把文件提交到本地仓库
```
git commit -m "commit 1"
```
4. 将本地仓库与github仓库关联
```
git remote add origin https://github.com/scyuan/learnGit.git
```
5. 提交到远程仓库
```
git push origin master 
```



### 版本回退

1. git reset

```JavaScript
--soft 回退后a分支修改的代码被保留并标记为add的状态（git status 是绿色的状态）
--mixed 重置索引，但不重置工作树，更改后的文件标记为未提交（add）的状态。默认操作。
--hard 重置索引和工作树，并且a分支修改的所有文件和中间的提交，没提交的代码都被丢弃了。
--merge 和--hard类似，只不过如果在执行reset命令之前你有改动一些文件并且未提交，merge会保留你的这些修改，hard则不会。【注：如果你的这些修改add过或commit过，merge和hard都将删除你的提交】
--keep 和--hard类似，执行reset之前改动文件如果是a分支修改了的，会提示你修改了相同的文件，不能合并。如果不是a分支修改的文件，会移除缓存区。git status还是可以看到保持了这些修改。
```

> 适合不是很多人（少于4人，或者单人）的协同开发项目。

情形一：当A在`master`本地提交了一次了`commit`后（没有提交远程分支）。这个时候通过`git reflog`可以查看所有的`reset`和commit`。通过`git reset --hard [HEAD]`回退到当前`HEAD`。


情形二：当A在`master`本地提交了一次了`commit`后，并提交至远程后。并且B`pull`了最新的代码。然后你猛的发现提交代码有误。这个时候怎么办？

首先，A通过`git reset --hard [HEAD]`回退到当前`HEAD`；然后强制提交`git push -f origin branch_name`；这个就覆盖了远程分支上的代码。最后B通过`git fetch --all`拉取所有远程代码（不自动合并），然后通过`git reset --hard origin/<branch_name>`回退分支。这样大家的代码都一致性的回到某个`commit`了。

回退成功。

2. git revert


