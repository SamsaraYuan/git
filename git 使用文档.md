![](https://i.imgur.com/1p6q7xf.png)
**注意：**不要用 windows 自带的记事本编辑任何文件

	git init								// 将所在的目录变为一个本地仓库，不管目录中有没有文件
	git add filename						// 将文件或文件夹添加到本地仓库中
	git commit -m “wrote a readme file”		// 将文件提交到仓库
	// 可以多次 add 后一次 commit 所有
	git status 								// 查看本地仓库的状态
	git diff filename						// 查看对 filename 做了什么修改
	git diff HEAD -- readme.txt				// 将当前版本中的 readme.txt 文件和工作区中的文件进行比较	
	git log	[--graph] [--pretty=oneline]    // 显示最近到最远的提交日志，--graph参数可以看到分支合并图，--pretty=oneline参数可以减少一些显示的信息
	git reset --hard HEAD^ 					// 回退到上一版本，或者将 HEAD^ 替换为 HEAD~1
	git reset --hard commintid				// 回退到上一版本后，再想回退到后来的版本，需要后来版本的commitid就可以，不需要全部，前几位就可以
	git reflog 								// 提交和回退记录，里面有commitid
	// git 管理的是修改不是文件，修改后需要 add 然后 commit 才能到仓库
	git checkout -- file 					// 可以丢弃工作区中对 file 的修改，其实就是版本库里的版本替换工作区里的版本
	git reset HEAD file						// 把暂存区的修改撤销掉，重新放回工作区
	git push -u origin master				// 远程仓库是空的， -u 参数可以将本地的 master 分支与远程的 master 分支关联起来
	git remote add origin git@github.com:michaelliao/learngit.git
	
	

删掉文件：
	
	// 先在工作区删掉文件filename
	git add filename
	git rm filename							// 从版本库中删掉
	git commit -m “”	
	git checkout -- filename 				// 可以用版本库中的文件提花工作区的文件来找回删除的文件，如果已经提交到版本库了，应该可以回退版本找回	

本地仓库与远程仓库之间是加密的：
	
	ssh-keygen -t rsa -C "youremail@example.com"	// 创建 SSH Key，创建成功后在用户主目录中有 .ssh 文件夹，里面有 id_rsa 和 id_rsa.pub 文件。将 id_rsa.pub 中的内容添加到 Git 服务器

分支：
  
    // 冲突时需要手动合并
    git merge --no-ff -m "merge with no-ff" dev         // --no-ff 禁用 fast-forward， 合并后会创建一个新的 commit
    // 分支策略，master 分支只是用来发布新版本，dev 分支用来干活，个人分支经常合并到 dev 分支就可以了
    
   
	
	
	
