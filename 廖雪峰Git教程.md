### Git教程

#### 安装Git

#### 创建版本库

初始化一个Git仓库，使用 `git init` 命令。

添加文件到Git 仓库，分两步：

- 第一步，使用命令 `git add <file>` ，注意，可反复多次使用添加多个文件。
- 第二步，使用命令 `git commit -m "本次提交的说明" ` ，完成。

#### 时光机穿梭

- 要随时掌握工作区的状态，使用 `git status` 命令。
- 如果 `git status` 告诉你有文件修改过，用 `git diff <file>` 来查看文件中修改内容。

##### 版本回退

- `HEAD` 指向的版本就是当前版本，因此，Git允许我们在版本历史之间穿梭，使用命令 `git reset --hard commit_id` 。
- 穿梭前，用 `git log` 可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用 `git reflog` 查看 命令历史，以便要回到未来的哪个版本。

##### 工作区和暂存区

##### 管理修改

##### 撤销修改

- 当你乱改了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令 `git checkout -- <file>` 。
- 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令 `git reset HEAD <file>` ，就回到了第一种情况，第二步安装第一种情况操作。
- 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

#####  删除文件

- 通过 `rm <file>` 删除工作区的文件。可以通过 `git status` 命令来查看什么文件被删除了。
- 确实是要删除版本库中该文件，用命令 `git rm <file>` 删掉，并且 `git commit` 。
- 如果是删错了，可以通过 `git chekcout -- <file>` 将版本库中的版本替换工作区中的版本，即“一键还原”。

#### 远程仓库

##### 添加远程库

要关联一个远程库，使用命令 `git remote add origin git@server-name:path/repo-name.git` ；

关联后，使用命令 `git push -u origin master` 第一次推送 master分支的所有内容；

此后每次本地提交后，只要有必要，就可以使用命令 `git push origin master` 推送最新修改；

##### 从远程库克隆

要克隆一个仓库，首先必须知道仓库的地址，然后使用 `git clone` 命令克隆。

例如：

`git clone git@github.com:chunbos/gitskills.git`

`git clone https://github.com/chunbos/gitskills.git`

Git支持多种协议，包括 `https` ，但通过 `ssh` 支持原生 `git` 协议速度最快。

#### 分支管理

##### 创建与合并分支

查看分支：`git branch` 

创建分支：`git branch <name>` 

切换分支：`git checkout <name>` 

创建+切换分支：`git checkout -b <name>` 

合并某分支到当前分支：`git merge <name>` 

删除分支：`git branch -d <name>` 

##### 解决冲突

查看分支合并图：`git log --graph` 

##### 分支管理策略
合并 `dev` 分支，注意使用 `--no-ff` 参数，表示禁用 `Fast forward` ，不会丢失分支信息：

```
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt |    1 +
 1 file changed, 1 insertion(+)
```

- 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

##### Bug分支

##### 多人协作

- 首先试图用 `git push origin branch-name` 来推送自己的修改
- 如果推送失败，则是因为远程分支比你的本地版本更新，需要先用 `git pull` 试图合并；
- 如果合并有冲突，那就需要解决冲突，并在本地提交；
- 没有冲突或者解决冲突后，再用 `git push origin branch-name` 推送即可。

如果 `git pull` 提示"no tracking information"，则说明本地分支和远程分支的链接关系没有创建，用命令 `git branch --set-upstream branch-name origin/branch-name` 这就是多人协作模式。

- 查看远程库信息，使用 `git remove -v` ；
- 本地新建分支如果没有推送远程，那么对其他人就是不可见的。
- 从本地推送分支，使用 `git push origin branch-name` ，如果推送失败，先用 `git pull` 抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用 `git checkout -b branch-name origin/branch-name` ，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的链接，使用命令 `git branch --set-upstream branch-name origin/branch-name` ；
- 从远程抓取分支，使用 `git pull` ，如果有冲突，要先要先处理冲突。
