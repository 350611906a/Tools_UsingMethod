

# ubuntu中安装samba服务  

安装步骤如下：

- 安装samba服务器：apt-get install samba

- 设置参数：

  ```shell
  1、编辑配置文件
  vi /etc/samba/sbm.conf
  在该sbm.conf文件最后添加如下内容：
  [code]
  path=/code
  writeable=yes
  browseable=yes
  guest ok=yes
  
  2、重启samba服务
  pkill smbd
  smbd
  
  3、查看smbd进程
  ps -ef |grep smbd
  
  4、创建路径
  mkdir code
  设置权限有如下两种方法：
  (1)chmod 777 /code
  (2)chown nobody:nogroup /code
  ```

- 经过以上设置后便可连接，如下
  - 查看虚拟机ip地址，ifconfig
  - win+r，输入：\\\ip地址即可