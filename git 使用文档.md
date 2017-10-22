# Git 使用说明
**注意：**

* 不要用 windows 自带的记事本编辑任何文件
* 具体详细的使用可以参考 [Git 使用手册](https://git-scm.com/book/zh/v2)
* git 管理的是修改不是文件，修改后需要 add 然后 commit 才能到仓库
* http 速度慢，而且每次需要输入口令
* [git 官网](https://git-scm.com/)
* 管理公钥的工具[Gitosis](https://github.com/res0nat0r/gitosis)
* 严格的权限控制[Gitolite](https://github.com/sitaramc/gitolite)
* 分支策略，master 分支只是用来发布新版本，dev 分支用来干活，个人分支经常合并到 dev 分支就可以了
## 简介
![本地仓库框架图](https://i.imgur.com/1p6q7xf.png)
## 1、常用命令
	git help <command>
### 密钥
	ssh-keygen -t rsa -C "youremail@example.com"	// 创建 SSH Key，创建成功后在用户主目录中有 .ssh 文件夹，里面有 id_rsa 和 id_rsa.pub 文件。将 id_rsa.pub 中的内容添加到 Git 服务器
	ssh -T git@github.com
### 创建

	git init							// 将当前目录初始化为一个本地仓库
	git clone remote-repository			// 将一个远程仓库克隆到本地

### 本地修改

	git status 								// 查看本地仓库的状态
	git diff filename						// 查看对 filename 做了什么修改
	git diff HEAD -- readme.txt				// 将当前版本中的 readme.txt 文件和工作区中的文件进行比较	
	
	git add .								// 添加所有的当前改变到暂存区
	git add filename						// 将文件或文件夹添加到本地仓库的暂存区

	git commit -m “wrote a readme file”		// 将文件提交到仓库

### 查看提交历史
	
	git log	[--pretty=oneline]				// 显示最近到最远的提交日志，后面的参数可以减少一些显示的信息

### 分支和标签
	
	git checkout -b dev						// 创建并切换 dev 分支， -b 表示创建并切换
	git branch dev							// 创建 dev 分支
	git checkout dev						// 切换到 dev 分支
	git checkout --track [remote/branch]	// 基于远程分支创建
	git branch [-av]						// 查看当前分支, 参数可以显示提交信息
	git checkout -b dev origin/dev			// 创建远程的 dev 分支到本地
	git branch -d dev						// 删除 dev 分支

	git tag v1.0							// 最新提交的 commit 打上标签
	git tag 								// 查看所有标签

	git log --graph --pretty=oneline --abbrev-commit	// 查看历史提交
	git tag v0.9 commitid						// 给 commitid 打标签
	git tag
	git show tagname							// 查看标签信息
	
	// 创建带说明的标签， -a 指定标签名， -m 指定说明文字
	git tag -a v0.1 -m "version 0.1 released" 38976	
	git tag v0.1

	// -s 用私钥签名一个标签
	git tag -s v0.2 -m "signed version 0.2 released" 897654		// 签名采用 PGP 签名， 如果没有安装 gpg 就会报错
	git tag v0.2
	
	git tag -d v0.1								// 删除标签
	git push origin tagname						// 推送某个标签到远程
	git push origin --tags					// 一次性推送全部标签
	
	// 删除远程标签
	git tag -d v0.9
	git push origin :refs/tags/v0.9

### 推送到远程
	git remote [-v]								// 查看当前已经配置的远程仓库
	git remote show remote						// 显示关于 remote 的信息
	git remote add <shortname> <url>			// 添加一个远程仓库的 shortname
	git remote rm origin						// 删除 origin
	
	git fetch <remote>							// 从远程下载所有改变，但是不合并到当前版本中
	git pull <remote> <branch>					// 从远程下载 branch 分支的所有改变并合并到当前版本
	git pull 	
	git branch --set-upstream dev origin/dev	// pull 失败后会有提示no tracking information, 需要建立本地分支与远程分支的链接
	
	git push <remote> <branch>  				// 推送本地 master 分支到远程仓库的 master 分支
	git push -u origin master					// 远程仓库是空的， -u 参数可以将本地的 master 分支与远程的 master 分支关联起来


	git branch -dr <remote/branch>				// 删除远程的分支
	
### 冲突合并
	git merge dev								// 将 dev 分支合并到当前分支中
	git rebase
	git mergetool
	// 冲突时需要手动合并
    git merge --no-ff -m "merge with no-ff" dev         // --no-ff 禁用 fast-forward， 合并后会创建一个新的 commit
		
### 撤销
	git reset --hard HEAD						// 放弃当前工作区的所有修改
	git reset --hard HEAD^ 						// 回退到上一版本，或者将 HEAD^ 替换为 HEAD~1
	git reset --hard commintid					// 回退到以前提交的 commitid 版本

	git checkout HEAD <file>					// 放弃工作区中对 file 的修改
	git checkout -- file 						// 可以丢弃工作区中对 file 的修改，其实就是版本库里的版本替换工作区里的版本
	git reset HEAD file							// 把暂存区的修改撤销掉，重新放回工作区
	git revert									// 

	git reflog 									// 提交和回退记录，里面有commitid

## 2、简单教程
### 删掉文件：
	
	// 先在工作区删掉文件 filename
	git add filename
	git rm filename								// 从版本库中删掉
	git commit -m “”	
	git checkout -- filename 					// 可以用版本库中的文件提花工作区的文件来找回删除的文件，如果已经提交到版本库了，应该可以回退版本找回	
### 开发新功能
	git checkout -b feature-func				// 新建分支
	// 开发完毕
	git add func.c
	git commit -m "add feature func"
	git checkout dev
	git merge --no-ff feature-func
	// 如果操作过程中新功能取消
	git branch -d feature-func
	// 如果没有提交
	git branch -D feature-func

## 3、参数配置
### 全局参数
	git config --global color.ui true		// 显示颜色
	git status
	git config --global alias.st status		// 用 st 代替 status
	git st
	git config --global alias.co checkout
	git config --global alias.ci commit
	git config --global alias.br branch
	// --global 表示全局参数，表示在此电脑的所有 git 仓库下都可用
	git config --global alias.unstage 'reset HEAD'
	git config --global alias.last 'log -1'
	git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

配置文件在 .git/config 或者 .gitconfig 中
	
### 忽略特殊文件
在工作区创建一个 .gitignore 文件，将忽略文件名填进去就可以了，不用从头写，只需将这里面的组合就可以了[GitHub提供的 .gitignore 文件](https://github.com/github/gitignore)
也要将 .gitignore 这个文件提交到远程。
当由于在这个文件中忽略了导致：
	
	git add app.class 		// 由于忽略了，导致失败就加上 -f
	git add -f app.class
	git check-ignore -v app.class		// 检查忽略规则

忽略文件的原则是：
忽略操作系统自动生成的文件，比如缩略图等；
忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

## 4、搭建 git 服务器：
	// 安装
	sudo apt-git install git
	// 创建 git 用户
	sudo adduser git
	// 创建证书登录，将需要登录的公钥导入到/home/git/.ssh/authorized_keys文件，一行一个
	// 初始化git 仓库
	sudo git init --bare sample.git
	// 修改仓库的所有者
	sudo chown -R git:git sample.git
	// 禁用 shell 登录
	编辑 /etc/passwd， 将
	git:x:1001:1001:,,,:/home/git:/bin/bash 改为
	git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
	// 克隆远程仓库
	git clone git@server:/srv/sample.git
	
## 5、其他	
* 码云和 github 如何做双向同步？？
* github 上的开源项目可以 fork 到自己的仓库，然后clone到本地，修改后提交，然后可以 pull request 到原仓库，如下图流程：
![](https://i.imgur.com/EtXWbng.png)



	
