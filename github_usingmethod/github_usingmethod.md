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

