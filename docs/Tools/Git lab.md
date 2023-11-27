# Git

## 注册gitee账号

[gitee官网](https://gitee.com/)

## 生成ssh秘钥

Gitee -> 设置 -> SSH公钥

（点这个 **怎样生成公钥** ）

![截屏2023-11-25 12.43.11](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311251244980.png)

ssh keygen -> 在`~/.ssh`找到公钥 -> 传公钥到gitee

（命令建议复制粘贴网站上的代码）

1. 生成公钥和私钥，按三次回车确定，直到出现一个符号矩阵，如下图。

	> 私钥是保存到本地的，我们想用ssh服务连接到哪就把公钥给传过去。如此就可以用这一对匹配的钥匙实现单向免密登录

![截屏2023-11-26 11.15.47](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311261115953.png)

![截屏2023-11-26 11.19.12](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311261119072.png)

2. 查看生成的公钥和私钥

	>  保存在家目录`~` 的隐藏`.ssh`文件夹下
	>
	> 如此访问： 
	>
	> ls ~/.ssh/
	>
	> 便可以展示

3. 读取公钥文件，并且复制粘贴到gitee网站上

	> cat ~/.ssh/id_ed25519.pub
	>
	> 然后将终端的内容粘贴到网站上即可实现免密登录
	>
	> ![截屏2023-11-26 11.25.34](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311261125247.png)

* 补充：

	用这种生成sshkey的方式，可以实现和其他服务器之间的免密登录。这种方式是非常常见的,只需要把公钥粘贴添加到服务器端的`.ssh/authorized_keys`即可实现。

	> ssh 用户名@服务器的地址或者域名  
	>
	>  ![截屏2023-11-26 11.29.14](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311261129733.png)

## 创建仓库

![截屏2023-11-25 12.48.49](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311251248307.png)

输入仓库名称即可

![截屏2023-11-25 12.50.24](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311251250675.png)

## 上传

1. 设置你的信息

	> `git config --global user.name "你的学号-姓名" `
	>
	> `git config --global user.email "你的邮箱"`

2. 创建一个文件夹并且设置为`git`仓库

	> mkdir my_git_repository
	>
	> cd my_git_repository
	>
	> git init
	>
	> echo "first time" > README.md
	>
	> git add .
	>
	> git commit -m "first commit"

3. 添加远程连接，也就是连接到你的远程仓库上

	> git remote add origin 你创建完页面的ssh链接（复制粘贴过来）
	>
	> ![截屏2023-11-25 16.31.15](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311251632312.png)
	>
	> git push -u origin main (这边建议用main 而不是master)
	>
	> 成功上传如下图
	>
	> ![截屏2023-11-25 16.45.18](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311251645519.png)

	由此，我们可以观察到gitee仓库里面的内容已经更新了

	

4. 额外命令

	* `git status`: 随时查询当前`git`仓库的情况，有没有未添加到暂存区（add）的文件，有没有未提交（commit）的文件，执行`add`和`commit`是为了让`git`仓库追踪到这些文件，不加入是没法进入`git`追踪的。

	* `git log`: 查询整个`git`仓库的提交记录,下面我们会介绍如何根据提交记录，回退版本，甚至能回到第一次修改

		![截屏2023-11-25 16.52.46](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311251652204.png)

	

> <font color="blue" size=5>EXERCISE 1: </font>
>
> 实验要求：
>
> 1. 在本地创建的git仓库里面，创建一个`README.md`文件，任意修改内容，第一次提交，push到gitee仓库
>
> 2. 再添加内容到`README.md`，第二次提交，push到gitee仓库
>
> <font color=red>实验报告内容：</font>
>
> <font color=red>1. 请将你创建的gitee仓库页面截图到实验报告里</font>
>
> <font color=red>2. 将你终端执行的`git log`截图到实验报告里</font>
>
> 
>
> gitee仓库样例：（需要看到两次提交和一个`README.md`）
>
> ![截屏2023-11-25 17.08.34](https://raw.githubusercontent.com/ohhoo0/pictures/main/202311251708972.png)

## 分支创建 & 版本回退

* `git branch 分支名 ` ：在当前分支的基础上创建新的分支（分支名自己给定）

	`git`是一个版本管理工具，我们可以追踪任何时候commit的仓库情况。

	>  写方案的时候， "第一版" “第二版” “第三版” “最后一版” “究极最后一版”
	>
	> 结果最后甲方喜欢还是第一版

	用`git`管理仓库，我们可以随时回到过去（checkout）。~~走反方向的钟~~

	而用分支名，可以使得多个人在同一个仓库的编写变得可行。每个人基于一个源点发展自己的分支，`main`分支代表着官方版本，而分支就是民间版本。如果自己开发自己仓库，也可以用不同的分支代表开发不同的功能，开发好了再并入`main`分支。

	总之，分支是`git`中非常重要的概念。

	![截屏2023-11-25 17.32.12](/Users/fengyuxuan/Library/Application Support/typora-user-images/截屏2023-11-25 17.32.12.png)

	如图，在当前`main`创建了一个`newbranch`分支，并且进入到了这个新分支，我们在新分支上做的任何操作，不影响原来的`main`分支。新分支在创建的那一刻继承了`main`分支，之后便与它无关。

* `git checkout 分支名` 

	`checkout`就是版本选择命令，可以选择任何一个版本查看当时的情况。见上图。



> <font color="blue" size=5>EXERCISE2: </font>
>
> 实验要求：
>
> 1. 在`main`分支创建新的`newbranch`分支，并且在新的分支下添加内容到`README.md`（需要commit提交）。
>
> 2. 此时观察`git log`并且打印当前的`README.md`，再切换到`main`分支，打印当前的`README.md`。
> 3. 观察有什么不同。
>
> <font color=red>实验报告内容：</font>
>
> <font color=red>1.截图两个分支状态（新分支和`main`分支）的`git log`</font>
>
> <font color=red>2.将两个状态的`README.md`用`cat`命令打印出来，截图。</font>
>
> <font color=red>3. **指出** 两次的内容有什么不同？为什么会造成这样的不同呢？git这样的功能有什么作用？</font>



## 额外参照

1. `git config --global user.name xxx`：设置全局用户名，信息记录在~/.gitconfig文件中
2. `git config --global user.email xxx@xxx.com`：设置全局邮箱地址，信息记录在~/.gitconfig文件中

3. `git init`：将当前目录配置成git仓库，信息记录在隐藏的.git文件夹中

4. `git add XX`：将XX文件添加到暂存区

5. `git add .`：将所有待加入暂存区的文件加入暂存区

6. `git rm --cached XX`：将文件从仓库索引目录中删掉

7. `git commit -m `"给自己看的备注信息"：将暂存区的内容提交到当前分支

8. `git status`：查看仓库状态

9. `git diff XX`：查看XX文件相对于暂存区修改了哪些内容

10. `git log`：查看当前分支的所有版本

11. `git reflog`：查看HEAD指针的移动历史（包括被回滚的版本）

12. `git reset --hard HEAD^` 或 `git reset --hard HEAD~`：将代码库回滚到上一个版本

13. `git reset --hard HEAD^^`：往上回滚两次，以此类推

14. `git reset --hard HEAD~100`：往上回滚100个版本

15. `git reset --hard `版本号：回滚到某一特定版本

16. `git checkout — XX`或`git restore XX`：将XX文件尚未加入暂存区的修改全部撤销

17. `git remote add origin git@gitee.com:xxx/XXX.git`：将本地仓库关联到远程仓库

18. `git push -u (第一次需要-u以后不需要)`：将当前分支推送到远程仓库

19. `git push origin branch_name`：将本地的某个分支推送到远程仓库

20. `git clone git@gitee.com:xxx/XXX.git`：将远程仓库XXX下载到当前目录下
21. `git checkout -b branch_name`：创建并切换到branch_name这个分支

22. `git branch`：查看所有分支和当前所处分支

23. `git checkout branch_name`：切换到branch_name这个分支

24. `git merge branch_name`：将分支branch_name合并到当前分支上

25. `git branch -d branch_name`：删除本地仓库的branch_name分支

26. `git branch branch_name`：创建新分支

27. `git push --set-upstream origin branch_name`：设置本地的branch_name分支对应远程仓库的branch_name分支

28. `git push -d origin branch_name`：删除远程仓库的branch_name分支

29. `git pull`：将远程仓库的当前分支与本地仓库的当前分支合并

30. `git pull origin branch_name`：将远程仓库的branch_name分支与本地仓库的当前分支合并

31. `git branch --set-upstream-to=origin/branch_name1 branch_name2`：将远程的branch_name1分支与本地的branch_name2分支对应

32. `git checkout -t origin/branch_name` 将远程的`branch_name`分支拉取到本地

33. `git stash`：将工作区和暂存区中尚未提交的修改存入栈中

34. `git stash apply`：将栈顶存储的修改恢复到当前分支，但不删除栈顶元素
35. `git stash drop`：删除栈顶存储的修改

36. `git stash pop`：将栈顶存储的修改恢复到当前分支，同时删除栈顶元素

37. `git stash list`：查看栈中所有元素





