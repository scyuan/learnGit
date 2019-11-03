### Git的一些基本知识

1. Working Directory（工作区）
2. Stage area（暂存区）
3. Git Directory（git目录、git 仓库、版本库）

**工作区**也就是我们正在编辑的文件、`git add .`之后会将我们修改后的改动保存到**暂存区**。暂存区的内容就是下一次`commit`所提交的内容。`git commit`会将暂存区内容提交到Git Directory（我习惯叫git仓库，网上也有叫版本库的）。仓库当然就是存放项目元数据和对象数据库的地方。

> 所以说一个完整的流程就是如下：

1. 在工作树中修改文件
2. 选择性地暂存要作为下一次提交的一部分的更改，仅将这些更改添加到暂存区域
3. 执行一次提交，该提交将按照暂存区域中的文件原样将其保存，并将该快照永久存储到Git目录中

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

> 常用用法

撤回head为`[HEAD]`的这次提交

```
git revert [HEAD]
```

情形：A提交了一次修改了`a.txt`内容为`AAAA`，此次`HEAD`为`c1c1c1`，然后继续提交一次并修改`a.txt`为`AAAA BBBB`。此次`HEAD`为`c2c2c2`。那么想回到第一次的提交怎么办？

> 做法：撤销第二次提交即可，并会自动生成一次提交。如果使用 `revert` 撤销的不是最近一次提交，那么一定会有代码冲突，需要你合并代码，合并代码只需要把当前的代码全部去掉，保留之前版本的代码就可以了


> `git revert` 命令意思是撤销某次提交。它会产生一个新的提交，虽然代码回退了，但是版本依然是向前的，所以，当你用`revert`回退之后，所有人`pull`之后，他们的代码也自动的回退了。


```
git revert c2c2c2
```

3. 一些其他命令

4. haha