# Git 团队协作开发指南
## 1.前置知识（先花5分钟看一遍，使用时再查阅）
### 常用 Git 命令清单
> 原文：[阮一峰的网络日志-常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)  
> 作者： 阮一峰  
> 日期： 2015年12月9日  

一般来说，日常使用只要记住下图6个命令，就可以了。但是熟练使用，恐怕要记住60～100个命令。  

![](//git.michaelxu.cn/classroom/guide/team-work/raw/develop/assets/bg2015120901.png)  

下面是我整理的常用 Git 命令清单。几个专用名词的译名如下。

```
Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库
```  

1.新建代码库  
```
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]
```  

2.配置  
Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。
```
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```  

3.增加/删除文件
```
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

4.代码提交
```
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

5.分支
```
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```

6.标签
```
# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
```

7.查看信息
```
# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat

# 搜索提交历史，根据关键词
$ git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件相关的每一次diff
$ git log -p [file]

# 显示过去5次提交
$ git log -5 --pretty --oneline

# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个commit的差异
$ git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示某次提交发生变化的文件
$ git show --name-only [commit]

# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]

# 显示当前分支的最近几次提交
$ git reflog
```

8.远程同步
```
# 下载远程仓库的所有变动
$ git fetch [remote]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all
```

9.撤销
```
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```  

10.其他
```
# 生成一个可供发布的压缩包
$ git archive

# 指定文件/文件夹 git clone/pull
$ git init toyshop && cd toyshop
$ git config core.sparsecheckout true
$ git remote add origin git@git.michaelxu.cn:tdreamer/wx-tdreamer-xin/2018/toyshop.git
$ echo "/dist/*" >> .git/info/sparse-checkout
$ git pull
$ git checkout develop
$ git pull
```

### 2.分支约定（再花2分钟浏览一遍，实际使用中理解）
**git 项目会分成 master、develop、 feature、 hotfix 以下几种分支类型：**   
* mater 为主分支，主要用于发布，代码永远处于稳定可产品化发布的状态；  
* develop 为开发分支，主要记录开发状态下相对稳定的版本；  
* feature 为功能分支，从 develop 上拉取代码，开发完成后再合并到develop分支上。经常用于一个大版本 develop 拆分成几个 feature 的场景，便于多个开发人员在同一版本迭代中开发各自不同的功能点，避免代码冲突，在开发完成后再合并到 develop 分支中进行测试；  
* hotfix 为紧急线上修复分支，需要从 master 上拉取分支进行bug修复，修复完成后分别并入 master 和 develop 分支。  

### 3.操作步骤（重中之重，开始你的团队开发之旅）
```powershell
# 注册账号 http://git.michaelxu.cn/

# git 本地全局设置
git config --global user.name "michael" // michael 换成自己注册到 git.michaelxu.cn 上的账号
git config --global user.email "michael.xu1983@qq.com" // michael.xu1983@qq.com 换成自己注册到 git.michaelxu.cn 上的 email

# 从头克隆新的项目仓库到本地进行开发
git clone https://git.michaelxu.cn/classroom/guide/team-work.git //下载项目和它的整个代码历史（如果需要使用 ssh 管理远程代码，参考这篇配置[如何配置github中的SSH key值](https://jingyan.baidu.com/article/dca1fa6f756777f1a44052e3.html)）
git branch local // 新建本地开发分支，文件新增/修改都在 local 分支进行
git checkout local // 切换到本地开发分支，文件新增/修改都在 local 分支进行
vi index.html // 编辑文件代码

# 开发完成了，添加新增/修改文件到远程仓库
git add . // 编辑完成后，添加所有新增/更改文件到暂存区
git commit -m "feat:此处填写本次修改日志便于日后大家知道" // 提交暂存区到仓库区，feat代表新增文件或功能，fix代表在原文件或功能上做修改或bug修复
git checkout develop // 切换到远程 develop 开发分支  
git pull // 此处一定先取回远程仓库的变化，避免代码冲突
git merge --no-ff -m "merge with no-ff" local // 合并 lcoal 中 commit的文件到 develop ，如果有文件冲突，就查看冲突描述，手动根据提示解决
git push //上传本地 develop 分支到远程仓库

# 按照提示输入自己注册的账号和密码

# 上传成功
```
> 总结：新建 lcoal 分支=>新增/修改文件=>合并到 develop 分支，并提交到远程仓库

