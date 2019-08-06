# Git 笔记

	前言：所有的版本控制系统，只能跟踪文本文件的改动，比如txt文件，网页，所有程序的代码等，图片，视频这些二进制文件，虽能也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是知道图片从1kb变成2kb，但是到底改了啥，版本控制也不知道。
	原文地址： http://www.cnblogs.com/smuxiaolei/p/7484678.html

## 一、git 安装

	安装完后设置用户名跟邮箱
	$ git config --global user.name "zhoulingsong"
	$ git config --global user.email "1016072788@qq.com"
	
	$ git config --list ： 查看设置列表
	
	--global              当前用户全局：use global config file
	--system              系统全局：use system config file
	--local               当前目录：use repository config file
	-f, --file <file>     指定文件：use given config file
	--blob <blob-id>      read config from given blob object

## 二、git 的使用

### 0. git init ：把这个目录变成 git 管理的仓库

	在需要 git 管理的目录下执行该命令；系统会自动创建 .git 目录；

### 1. git add ：将文件加入暂存区
	1. git add xx	: 将xx文件加入到暂存区；
	2. git add -A	: 添加所有内容；
	3. git add .	: 添加新文件和编辑过的文件不包括删除的文件(测试像当前目录，而且删除的文件也包括);
	4. git add -A . : 仅添加当前目录及其子目录所有修改；	
	5. git add -u	: 添加编辑或者删除的文件，不包括新添加的文件;
	6. git rm xx	: 删除文件并将修改更新到缓存区(相当于手动删除文件后再 git add xx);

### 2. git commit : 把暂存区文件提交到当前分支

	eg: git commit -m "提交的注释文本"

### 3. git push : 推送到远程仓库

	eg：git push origin master(分支名称）

### 4. git diff : 比较

	git diff xx : 查看文件 xx 改动;
	git diff HEAD -- xxx.txt; 查看工作区与版本库 xxx.txt 文件的区别；

### 5. git log : 显示提交版本记录，不包含回退版本记录

	git log --pretty=oneline : 一行展示；
	git log -2	: 展示最新 2 条记录；
	git log --graph --pretty=oneline --abbrev-commit : 展示分支合并线，一行，版本号简写；

### 6. git reflog : 显示所有版本记录，包含回退版本记录

## 三、版本回退《时光穿梭机》

	HEAD	: 当前版本；（HEAD 指向当前分支，当前分支指向最新提交 commit）；
	HEAD^ 	: 上一个版本；
	HEAD^^ 	: 上两个版本，（以此类推，此处省略 N 个 ^）；
	HEAD~100: 上 100 个版本；

### git checkout ： 丢弃工作区修改文件内容和删除文件操作

	备注： 未 git add，即只能丢弃工作区的内容，无法丢弃暂存区内容；
	git checkout  -- xxfile 可以丢弃工作区 xxfile 文件的修改内容；
	git checkout xxx.txt ; 当没有同名分支时不加-- 也可以；
	注意： -- 很重要，如果没有 -- 变成切换分支优先；

### 1. git reset
	0. git reset HEAD 	: 撤销 add 操作；（任何版本回退都会将缓存区内容撤回工作区）
	1. git reset  --hard HEAD^ : 每个 ^ 代表上一个版本，如上上个版本 HEAD^^ 以此类推；
	2. git reset  --hard HEAD~100 : 上 100 个版本；
	3. git reset  --hard 版本号	: 回退到指定版本号(7位字符），可以是已回退的版本号；


## 四、工作区与暂存区

	工作区： 就是你在电脑上看到的目录(.git目录除外);
	版本库(Repository)： .git 目录，里面存了很多文件；
		stage（或者叫index）的暂存区;
		Git 自动创建了第一个分支 master,以及指向 master 的一个指针HEAD;


## 五、远程仓库

### 1. 关联 gitHub
	>>> 账号：zhoulingsong
	>>>	邮箱：1016072788@qq.com	
	>>>	密码：aa123123
		创建 ssh key 秘钥串; 
		ssh-keygen -t rsa –C "1016072788@qq.com" 
		直接回车直到生成 .ssh 目录及 id_rsa 和 id_rsa.pub 两文件；
	
	https://github.com/zhoulingsong/git.git
	git@github.com:zhoulingsong/git.git	（推荐）

### git remote : 关联本地到远程仓库

	git remote	: 查看所有远程仓库；
	git remote add origin https://github.com/zhoulingsong/git.git  （gitHub 仓库）
	git remote rm origin : 删除 origin 远程仓库;

### git push : 推送本地代码到远程

	git push -u origin master : 第一次加上 -u 则会关联本地跟远程相同分支名；
	以后推送则直接 git push 就会推送到相应分支；
	git push --set-upstream origin master; (也可以关联远程仓库）

### git clone ：将远程仓库克隆到本地库

	git clone https://github.com/zhoulingsong/git.git

>   GitHub 地址： Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议;
>   https://github.com/zhoulingsong/git.git
>   git@github.com:zhoulingsong/git.git
>   https 除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令;
>   但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https;

## 六、分支管理

### git checkout -b 分支名称 ： 创建分支并切换

	git checkout -b dev	: 创建 dev 分支，并切换到该分支；相当于以下两条命令；
	git branch dev;
	git checkout dev;

### git branch : 查看所有分支（* 表示工作分支）

### git branch 分支名称 ： 创建分支

	git branch dev : 创建 dev 分支;

### git checkout 分支名称 ： 切换分支

	git checkout dev : 切换到 dev 分支；
	git checkout –b dev origin/dev : 获取远程 dev 分支;

### git merge : 分支合并（将其他分支合并到当前分支）

	git merge dev ： 将 dev 分支合并到当前分支; 
	git merge --no-ff -m "message" 分支名称 : 


### git branch -d 分支名称 : 删除分支

	git branch -d dev : 删除 dev 分支;

### 解决冲突

	Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容;
	eg:
	<<<<<<< HEAD
	66666666666		// 当前分支修改的内容
	=======
	77777777777		// dev 分支修改的内容
	>>>>>>> dev

### 分支管理策略

	默认情况为 "Fast forward" 模式合并分支，该模式下，删除分支后，会丢掉被删除的分支信息；
	–no-ff : 禁用"Fast forward" 模式，该模式下，删除分支后，会丢掉被删除的分支信息;
	git merge --no-ff -m "merge no-ff" dev : 使用 no-ff 模式将 dev 分支合并到当前分支；
	
	master	： 稳定分支；
	dev		： 开发分支；
	bug		： 

###	git stash : 保存工作现场（保存暂存区）

	git stash list : 查看当前 stash 列表；
	恢复方式1：
	git stash apply : 恢复最新一条 stash 内容，但不删除;
	git stash drop	: 删除最新一条 stash 内容；
	恢复方式2：
	git stash pop	: 恢复并删除 stash 内容；

## 七、多人协作

	远程仓库的默认名称是 origin;

### 1. git remote : 查看远程库的信息

	git remote –v : 查看远程库的详细信息;

### 2. git push : 推送分支到远程

	git push origin master : 指定本地分支（本地分支跟远程分支自动对应）
	master 分支是主分支，因此要时刻与远程同步；
	bug 分支不需要推送到远程去，可以先合并到主分支上，然后把主分支推送到远程;

### 3. git clone : 将远程仓库克隆到本地

	git clone https://github.com/zls/test : （远程仓库地址）
	git checkout –b dev origin/dev : 获取远程 dev 分支;
	git push origin dev : 将 dev 分支推送到远程;
	
	git branch --set-upstream-to=origin/dev dev : 
	git branch --set-upstream dev origin/dev : 
	指定本地 dev 分支与远程 origin/dev 分支的链接，根据提示，设置dev和origin/dev的链接：

## 八、标签管理

	标签虽然是版本库的快照，但其实它就是指向某个 commit 的指针;
	注意：标签不是按时间顺序列出，而是按字母排序的;

###	1. git tag : 查看所有标签

### 2. git tag <标签名> <版本号> : 创建标签

	git tag v1.0	: 默认打在最新的 commit 上；
	git tag v1.1 版本号	: 指定打到某次 vommit 上；
	git show <tagname>	: 查看标签信息；
	git tag -a <tagname> -m "备注信息" ：可以指定标签信息；
	git tag -d tagname： 删除本地标签；
	git push origin <tagname> ：推送指定标签到远程；
	git push origin --tags ： 推送所有本地未推送的标签到远程；
	git push origin :refs/tags/v0.9 ： 删除远程标签 v0.9；

## 九、 git 常命令
	mkdir XX	： 创建一个空目录 XX 指目录名;
	pwd		： 显示当前目录的路径;
	cat XX	: 查看 XX 文件内容;
	git init	: 把当前的目录变成 git 仓库，生成隐藏.git文件;
	git add XX	: 把 xx 文件添加到暂存区;
	git commit –m “XX” : 提交文件 –m 后面的是注释;
	git status	: 查看仓库状态;
	git diff XX	: 查看 XX 文件修改了那些内容;
	git log	: 查看历史记录;
	git reset --hard HEAD^	: 回退到上一个版本;
	git reset --hard HEAD~ 	: 回退到上一个版本;
	git reset –-hard HEAD~100	: 回退到上 100 个版本;
	git reflog	: 查看历史记录的版本号id;
	git checkout -- XX	: 把XX文件在工作区的修改全部撤销;
	git rm XX	: 删除 XX 文件;
	git remote add origin https://github.com/zls/test	: 关联一个远程库;
	git push –u(第一次要用-u 以后不需要) origin master	: 把当前master分支推送到远程库;
	git clone https://github.com/zls/test	: 从远程库中克隆;
	git checkout –b dev  创建dev分支 并切换到dev分支上
	git branch  查看当前所有的分支
	git checkout master 切换回master分支
	git merge dev    在当前的分支上合并dev分支
	git branch –d dev 删除dev分支
	git branch name  创建分支
	git stash 把当前的工作隐藏起来 等以后恢复现场后继续工作
	git stash list 查看所有被隐藏的文件列表
	git stash apply 恢复被隐藏的文件，但是内容不删除
	git stash drop 删除文件
	git stash pop 恢复文件的同时 也删除文件
	git remote 查看远程库的信息
	git remote –v 查看远程库的详细信息
	git push origin master  Git会把 master 分支推送到远程库对应的远程分支上


## 十、海龟（TortoiseGit)

	安装 TortoiseGit，然后就能看到可爱的文件夹及文件的 git 提示 icon 了；
	汉化包：直接官方下载安装，然后在设置页可选择语言；


## 十一、答疑

### 1. 如何修改远程仓库 url？

	方式一：先删除原来的 url，再创建新的 url；
	git remote rm origin
	git remote add origin git@github.com:Remember/text.git
	
	方式二：修改 url 命令
	git remote set-url origin git@github.com:Remember/text.git
	
	方式三：手动修改 .git/config 文件，找到url进行相应的修	
	url = git@github.com:zhoulingsong/git.git

### 2. TortoiseGit 安装完后无法进行远程操作？

	需要修改 TortoiseGit 的 SSH client；
	setting -> NetWork -> SSH client -> git 下面的 ssh.exe(去 git 安装目录下找）

### 3. 设置代理

~~~ 
git config --global http.proxy host:port
eg: 设置全局的: git config --global http.proxy 192.168.4.12:8080
~~~





---
---
