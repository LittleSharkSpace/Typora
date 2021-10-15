# Git学习

![](https://img2018.cnblogs.com/blog/1348316/201905/1348316-20190506152518565-55239077.png)

![Git中的文件状态](https://img2018.cnblogs.com/blog/1348316/201905/1348316-20190506185510825-1791389472.png)

## 版本追踪

```bash
# 添加追踪文件
git add example.txt

# 查看状态
git status

# 取消文件追踪
git rm --cached example.txt

# 针对被追踪文件同时add+commit操作
git commit -am example.txt

# 查看最近两次提交内容的差异
git log -p -2

# 一行显示
git log --oneline

# 查看版本更新路线图
git log --graph

# 格式化输出(版本hash值、操作人、操作时间、描述)
git log --pretty=format:"%h - %an, %ar : %s"

# 显示指定操作人提交的版本值
git log --author="example"

## commit前的操作
# 查看追踪文件刚修改前后的区别（modified状态下，即add之前查看）
git diff 

# 查看追踪文件追加修改前后的区别(staged状态下，即add之后查看)
git diff --staged

# 文件删除
git rm example.txt
git commit -m 'remove example.txt'

# 文件重命名
git mv example.txt rename.txt
git commit -m 'move and rename example.txt'

# 文件移动
git mv example.txt move/move.txt
git commit m- 'move and rename'


# 文件忽略.gitignore
/node_modules 				 # 忽略node_modules文件夹下所有文件
*.log 					    # 忽略.log结尾的文件
*.zip 						# 忽略.zip结尾的文件

# 值得注意的是：使用git status命令时如果一个目录下为空(包括一个目录下存在的文件都是被忽略文件，则也可以认为是空)，则不会获取该目录

# 对于已经被追踪文件再添加到忽略文件中则无效，可以使用如下命令：删除缓存中已经被追踪的文件使其变成未被追踪的文件
git rm -r --cached .

```

## 变更还原

```bash
# 一键还原指定文件(未添加add之前有效)
git checkout -- [filename]	# 将文件filename恢复到上一次的状态

# 撤销追踪操作与文件还原(该文件已经add)
git reset HEAD [filename]
git checkout -- [filename]

# 版本回退(该版本回退后再此版本后的操作记录均会不存在！不同于回到旧版本)
git reset --hard HEAD^ 				# 强制回退到上一个版本
git reset --hard HEAD^^ 			# 强制回退到上上个版本
git reset --hard HEAD [hash号] 		# 强制回退到指定hash的版本
git reflog 						   # 查看指针指向的日志记录


# 回到指定旧版本
git log
git checkout [版本hash] --[filename] 		# git checkout b52e327 -- .

```

## 分支合并

```bash
# 创建/罗列分支
git branch [name]
# 切换分支
git checkout [branch name]
# 建立并切换分支
git checkout –b [branch name]
# 删除分支
git branch [name] -d
# 强制删除分支
git branch [name] -D

# 如何正确的合并分支
git merge [branch name] # 将dev的更新合并到master，则在master分支下执行：git merge dev

# 如何解决合并时发生的冲突
git status 					# 查看冲突原因 
git merge --abort 			 # 忽略合并
#手动选择正确内容#

# 如何通过命令查看版本线图
git log
git log --oneline
git log --oneline --graph
git log --oneline --graph --all
git log --oneline --graph -[number]

# 快转机制的意义：快转实际就是当前master的将来时
git merge branchname -no-ff

# 更多的合并方法
git merge --no-ff --no-commit [branchname]
git merge --no-ff [branchname]
git merge -squash [branchname]
git reset –hard ORIG_HEAD


# 一次性删掉所有不想要的分支（对于未合并的分支需要使用D来强制删除）
git branch --merged | egrep -v "(^*|master|develop)" | xargs git branch -d  
git branch --no-merged | egrep -v "(^*|master|develop)" | xargs git branch -D

$ egrep: 用于在文件内查找指定的字符串
$ xargs: 能够捕获一个命令的输出，然后传递给另外一个命令
```

## GitHub详解

<ol>
    <li>注册账户</li>
    <li>仓库信息</li>
	<ul>
        <li>创建仓库</li>
        <li>删除仓库</li>
    </ul>
</ol>

```bash
###本地仓库推送到远端仓库###
# 添加远端地址（下面语句的origin可以理解为'https://github.com/xxxxxxx/example.git'的别名）
git remotes add origin https://github.com/xxxxxxx/example.git

# 查看远端地址名称(如执行了上一句，则输出origin)
git remote 
git remote -v

# 修改远端仓库地址
git remote --set-url origin https://github.com/xxxxxxx/example.git

# 推送到远端仓库(-u  <==> --set-upstream)
git push -u origin master


###主仓库作为服务器###
仓库配置：需要建立一个与github账号名相同的仓库名


###如何获取远端项目###
# 注意download与clone的区别:下载到指定路径时不会携带.git，而clone会
git clone [远端仓库地址] 			
git clone --no-checkout					  # 快速克隆，如果想要分支文件信息需要执行'git checkout'
git clone --bare [远端仓库地址] [裸仓名称]	# 裸克隆，单纯的克隆一份.git仓库；如果想要分支文件信息需要执行'git clone /path/xxx/裸仓名称'
```

## 多人协同开发

```bash
###本地分支push到远端分支###
git push
git push -u origin
git push --set-upstream origin [branch name]	# 主要是第一次提交需要使用
git push --all								 # 一次性提交多个分支


###pull操作详解###
git pull == git fetch + git merge


###删除远端分支（远端追踪分支）###
git push origin --delete [branch name]


###仓库迁移(即修改远端仓库的地址)###
git remote set-url origin [远端仓库的地址]



```

## 服务器自动部署

```bash
### 使用SSH连接Github ###
# 本地生成公钥、密钥
ssh-keygen 

# 将公钥复制到github仓库

# 使用ssh连接
ssh git@github.com

# 连接成功后使用ssh协议clone仓库
git clone git@github.com:xxxxxx/shopping_cart


### github手动部署流程 ###
①创建项目
②提交到阿里云服务器


### github自动部署流程 ###
①提交到github
②配置action自动部署
③选用插件
④配置公钥和秘钥

```

## Gitlab详解

```bash
### fork使用场景 ###


### GITLAB CI/CD ### 

### GITLAB Runner ### 


### 实现Gitlab CI/CD ###

```







 