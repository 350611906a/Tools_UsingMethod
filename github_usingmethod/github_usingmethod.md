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

