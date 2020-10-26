# git的使用  

## 1、设置git参数  

（1）显示当前的git配置

​		git config --list  

（2）设置提交仓库时的用户名信息

​		git config --global user.name "王文杰"

（3）设置提交仓库时的邮箱信息git

​		git config --global user.email "beaconwwj@163.com"



## 2、命令(上)——对本地仓库的操作  

（1）解决git中无法使用中文的方法

​		左上角 --> 右击 --> options --> Text --> local ，选择zh_CN，character set中选择 UTF-8

（2）本地仓库的操作

​	1) 创建文件夹demo，进入后，执行**git init**(会出现master，表示主分支)，------> ls -a  ----->可查看 ls 可查看到./git

​	2) git status(查看状态)

​		vim README.md ，编辑 ------>  save,  再次git status

​	3) git add README.md 

​	4) git commit -m "第一次提交"

​	5) 重新编辑后，提交

​		git commit **-a** -m "增加一些修改"

​	6) git log

​		查看版本信息

​	7) 查看同一个文件的不同版本

​		git show  前面不同版本的harsh值。



## 3、命令(下)——对远程仓库的操作  

（1）查看远程信息，确定本地仓库与远程仓库的关联情况

​	 `git remote -v`

 （2）将本地仓库与远程仓库关联与断开

关联方法：

`git remote add origin https://github.com/350611906a/Linux`

断开方法：

`git remote remove origin`

 （3）将本地**相同**日志的仓库上传到远程仓库

​	 `git push origin master`

 （4）创建并上传文件夹

```c++
mkdir Doc
cd Doc
touch a.cpp
cd ..
git add .
git commit -m "上传一个文件夹"
git push origin master
```

 （5）删除仓库中的文件或者文件夹

```c++
git rm -r --cached Doc
git commit -m "删除文件夹Doc"
git push origin master
```

 （6）当建立的本地仓库与远程仓库日志不一致时，在push时，会出现如下错误`fatal: refusing to merge unrelated histories`，处理方法：

```c++
git pull origin master --allow-unrelated-histories
作用：最新的版本需要添加 --allow-unrelated-histories 告诉 git 允许不相关历史合并
```

（7）一般为了避免上面（6）叙述的问题，一般在修改仓库之前，先将远程仓库pull下来，在进行修改。

（8）使用clone

```c++
新建一个任意名字的文件夹，进入后，右键 --> Git Bash Here --> 执行下面命令
git clone https://github.com/350611906a/Linux
--> clone下来后，远程连接就已经建立了，因为clone下来的仓库已经有.git文件了。
```

（9）git -a -m "xxx"时报错：

```shell
OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443
```

对应解决方法：

```shell
git config --global --unset http.proxy
```

（10）git push origin master时报错：

```shell
! [rejected] master -> master (fetch first)
```

解决方法：

```shell
git pull --rebase origin master
```



## 4、git的进阶使用  

### 4.1 gitignore的使用  

1、为什么需要gitignore功能？

​    很多时候的工程中存在一些中间文件，在上传github时不需要上传，在不删除这些文件的情况下，如何做到一次性上传所有文件，那么，就需要.gitignore文件来起作用了。

2、gitignore的作用？

   创建.gitignore文件，并编辑进相应的文件名或正则的通配符表达式等后，在上传文件时，便可以忽略这些文件，当然该.gitignore文件需要上传。

3、使用方法：

​	（1）在本地仓库中创建.gitignore文件；

​	（2）编辑改文件，写入要忽略文件名。比如，要忽略a.txt和所用的.o文件，则.gitignore文件编辑如下：

```shell
a.txt
*.o
```

​	（3）上传该文件，并push至github上；

​	（4）查看被忽略目标忽略的规则：

```shell
git check-ignore -v <file name>
```

​	（5）强制添加被忽略掉得文件

```shell
git add -f <file name>
```

​	（6）将上传到暂存区的文件回退到临时区：

```shell
git reset HEAD
```

### 4.2 换行符使用

CR: carriage return 回车，光标到首行，‘\r’ = return

LR: line feed 换行，光标下移一行，'\n' = newline

linux中：换行为\n

windows中：换行为\r\n

mac os中：换行为\r

#提交时转换为LR，检出时转化为CRLF，默认设置不用修改，Git是linux配置

#允许提交包含混合换行符合的条件：

```shell
git config --global core.safecrlf false
```

### 4.3 命令别名的设置

设置方法1：直接命令设置如下：

```shell
git config --global alias.ci commit    #将commit的别名设置为ci
```

设置方法2：编辑配置文件如下：

- vi ~/.gitconfig

- 添加如下：

  ```shell
  [alias]
          ci = commit
          ad = add .
          br = branch
          hi = log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
          st = status
  ```

### 4.4 凭证的设置

在push时有时会存在要求输入账号和密码，为了解决这个问题，需要存储凭证。

#存储凭证

```shell
git config --global credential.helper wincred
```

## 5、git协议

### 5.1 本地协议  

1、克隆本地仓库

```shell
git clone /d/github/test.git
```

**注：**不建议添加file，如：git clone file:///c/github/test.git

2、添加本地远程仓库的链接

```shell
git remote add origin /d/github/test.git
```

3、报错：

```shell
![remote rejected] master -> master(branch is currently checked out)
```

解决方法：

```shell
方法1：
修改~/.gitconfig文件，添加如下字段：
[receive]
	denyCurrentBranch = ignore
方法2：
	git config --global receive.denyCurrentBranch ignore #待验证
```

4、将仓库更新到指版本

- 查看所有版本号：

```shell
git reflog
```

- 更新到指定版本：

```shell
git reset --hard 版本号
```

5、下载指定版本的仓库

```shell
git checkout 版本号
```



### 5.2 git协议 (一般用在只读)

1、克隆远程仓库

```shell
git clone git://server_ip/test.git
```

2、添加远程仓库

```shell
git remote add origin git:://server_ip/test.git
```

3、优缺点

- 优点：4个协议中速度最快
- 缺点：缺乏授权协议，9418端口防火墙一般不开



### 5.3 HTTP协议

1、克隆远程仓库：

```shell
git clone https://github.com/350611906a/test.git
```

2、添加远程仓库

```shell
git remote add origin https://github.com/350611906a/test.git
```

3、优缺点：

- 优点：简单上手、走80端口(为防火墙开放的端口)
- 缺点：公司内部部署较为复杂、效率低下



### 5.4 SSH协议  

1、克隆远程仓库

```shell
git clone ssh://git@github.com/350611906a/test.git
git clone git@github.com:350611906a/git.git   #为缩写形式，更常用
```

2、添加远程仓库链接

```shell
git remote add origin git@github.com:350611906a/test.git
```

3、优缺点：

- 优点：安全级别与https相当、本地部署容易、压缩最大，传输速度快
- 缺点：可能有些企业不能使用

4、配置SSH的方法：

（1）生成RSA密钥对

```shell
ssh-Keygen -t rsa -C "beaconwwj@163.com"
# 生成对应公钥和私钥，会提示保存位置。一般在 ~/.ssh/ 路径下。
```

（2）在github网站上添加公钥

```shell
拷贝公钥id_rsa.pub文件中内容 --> github主页 --> setting --> SSH and GPGkeys --> New SSH Key --> 命名为win0_pc --> 粘贴拷贝的公钥 --> 保存
```

（3）优先使用SSH协议

​	查看方法：**git remote -v**

（4）完成以上部署，之后电脑重装系统或者换了电脑，需重新部署

​		  为了不重复动作，可以将~/.ssh/下的文件备份一份到百度网盘上，重装系统后，直接拷贝下来即可。



## 6、git进阶的命令  

### 6.1 几个git的新命令  

- git blame

  ```shell
  1、逐行查看修改历史及修改人：
  	git blame <file name>
  2、查看从第100行到第110行的修改历史及修改人：
  	git blame -L 100,110 <file name>
  ```

- git clean

  ```shell
  1、选中打算清除的文件：
  	git clean -n
  2、删除选中的文件：
  	git clean -f
  3、选中打算清处理的文件，连同.gitignore中的文件一并删除
  	git clean -x
  注意：都是先选中，在删除。即1->2，或者3->2
  ```

- git  rm 和 rm

  ```shell
  1、直接rm文件
  	git add .
  	git commit -m "message"
  2、git rm 文件
  	git commit -m "message"
  ```

### 6.2 git add的使用  

- 修改后的文件的提交

  ```shell
  git ci -am "update file"
  ```

- 添加文件夹

  - mkdir dir   --> 添加文件夹，此时，git st时试没有变化的
  -  进入dir，然后，touch文件a.txt
  - 回到父路径，git st可以看到需要的修改
  - git add .
  - git cmmit

- git add .

  ​	表示将当前路径下的所有文件都提交，也可以指定文件：git add <file name>

- git add -p <file name>

  对代码进行分步提交。

  - y --> yes
  - n-->no
  - s-->split分割



### 6.3 git commit深入讲解 

1、对于已经提交到暂存区的文件进行修改并提交

- 方法1：

  ```shell
  git add .
  git commit -am "message"
  ```

- 方法2：

  ```shell
  git commit -a -m "message"
  ```

- 方法3：

  ```shell
  git commit -am "message"
  ```

2、message的书写格式

```shell
<type>(<scope>): <subject>
其中，
type为标签：
	feat: 新功能
	fix：修补bug
	docs: 文档
	style: 格式
	test：增加测试
	chore：构建过程或者辅助工具的变化
	
subject为修改内容，请用英文。
scope为选填。
```

3、查看详细修改

```shell
git show HEAD
```



### 6.4 git信息查看  

1、简短查看信息：

```shell
git status -sb
其中sb：short and branch
```

2、查看某个提交信息：git show

```shell
git show HEAD       //最新提交信息
git show HEAD^      //第二新信息
git show 版本哈希值   //查看对应版本的信息
git show HEAD~2     //前两个提交的详细信息
```

3、查看日志：git log

```shell
git log <file name>           //查看某个文件的日志
git log --grep <message>      //过滤查看日志
git log -n                    //表示查看多少条日志
```



### 6.5 git diff功能   

- git diff

  ​		表示工作区与暂存区的差异

- git diff --cached

  ​		表示暂存区与版本的差异

- git diff HEAD

  ​     	表示工作区与版本的差异

- git diff 哈希1 哈希2

​                 表示哈希1和哈希2两个版本的差异

- git diff  TT   

  ​		  表示工作区与大有TT标签的版本差异

  ​		  其中，git tag TT 哈希值/HEAD~4      //表示给哈希值版本/第4个版本打上TT的标签

- git diff --cached TT

​                 表示暂存区与TT版本的差异



### 6.5 git的回撤操作  

1、git reset HEAD

​			回撤暂存区内容到工作区（是对add的回撤）

2、git reset HEAD^ --soft

​			回撤提交到暂存区（将版本回撤到暂存区）

3、git reset HEAD --hard

​			回撤提交，放弃变更（直接放弃版本的提交，相应文件也会被删除）

4、git push -f

​			回撤远程仓库（先reset，再git push -f）

**注意：**回撤一定要在推送到远程仓库之前，推送后的回撤影响很大，影响别人的版本。

5、修改上一个版本，并将上一个提交的版本与本版本一并作为一个新版本提交，注：此时上一版本的日志没有了：

```shell
	git add .
	git commit --amend -m "message"
```

6、git rebase -i HEAD~3

​			基变操作，改写历史提交

```shell
eg:	提交5次，有5个日志，5次提交。将5次提交修改成2次，并将对应message也修改为2个。
```



### 6.6 标签操作   

1、git tag foo

​		在当前提交上，打上标签foo。

2、git tag foo -m "message"

​		在当前提交上打标签foo，并设置对应的标签message。

3、git tag foo HEAD~4

​		在当前提交之前的第4个版本上，打标签foo。

4、git tag

​		列出所有标签。

5、git tag -d TT

​		删除TT标签。

6、git push origin --tags

​		将多个标签一次性全部推送到远程仓库。

7、git push origin v0.1

​		把某个标签v0.1推送到远程仓库

8、git push origin :refs/tags/v0.1

​		将本地删除的标签推送到远程仓库

 

### 6.7 分支操作  

1、分支的作用：

​		为了配合多人并行协同开发。

2、长期分支和临时分支：

​	（1）长期分支：develop分支和master分支

​	（2）临时分支。

3、开分支的实例子

- git branch iss53

  ​	开个issues3分支

- git hi -10

  ​	可以看到HEAD->master，iss53两个分支。

- git checkout issues3

  ​	从master分子切换到iss53分支。

- touch b -> git  ad -> git ci -m "C3" -> gti hi -10 查看结果

- git checkout master -> git branch hotfix -> git branch -> git co hotfix -> touch c -> git ad -> git ci -m "C4" -> git hi -10

  ​	切换回master分之后，在创建一个分支hotfix，查看分支个数；在切换到hotfix分支上，创建文件c，并上传，最后查看历史。

- 要将hotfit分支合并到master上

  git checkout master

  git merge hotfix

- git branch -d hotfix

  ​	将hotfix分支删除。

- 进入iss53分支进行编辑

  ​	echo 111 >> b

  ​	git ci -am "C5"

  ​	git hi -10

- 将iss53全部合并到master分支上

  ​	git chechout master

  ​	git merge iss53 --> 提示冲突，直接q退出即可。

  ​	git hi -10

4、冲突的解决

- 产生冲突的原因：

  - 在不同的分支上，修改同一个文件

  - 不同的人修改了同一个文件

  - 不同的仓库修改了同一个文件

    冲突只在合并分支时才会发生。

- 解决方法：

  - 冲突发生并不可怕，冲突的代码不会丢失
  - 解决冲突，重新提交，commit时不要给message
  - 冲突信息的格式。

5、冲突后状态为（master|MERGING）

​	修改后：

​					git add a

​					git commit

​					:q

​					完成。

6、分支命令

- git branch foo

  ​	创建分支foo

- git checkout foo

  ​	切换到分支foo

- git checkout -b foo 

  ​	创建分支并同时切换到foo分支上，一步到位。

- git branch -m oldname newname

  ​	提示名字有重名

  git branch -M oldname newname

  ​	不提示名字有重名

- git branch -d foo

  ​	提示删除foo分支

  git branch -D foo

  ​	直接删除foo分支

- git branch -r 

  ​	列出远程分支

- git branch -r --merged

  ​	列出远程已合并的分支

- git checkout -t origin/foo

  ​	取出远程foo分支

- git push origin <remote branch>

  git fetch -p

  ​	删除分支

- git merger <branch name>

  ​	合并分支

- git merge --no-ff

  ​	合并分支，拒绝fast forward，产生合并commit

- git stash操作

  - 保存进度

    git stash   //修改了版本，但是不想提交，却又要切换到别的分支上，使用该命令将修改的暂存

  - 弹出进度

    git stash pop  //将暂存的内容弹出来

  - 查看stash列表

    git stash list

  - 删除stash列表

    git stash clear

    

    

    

    

    

    

    

    