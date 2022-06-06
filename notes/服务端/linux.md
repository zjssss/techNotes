### linux本质

一切皆文件——读文件，写文件，操作文件的权限

### linux入门概述

linux是一套类Unix的操作系统

继承了Unix**以网络为核心**的设计思想，是一个性能稳定的多用户操作系统

Kail linux：安全渗透测试使用（白帽子）

市面上比较有名的发行版有ubentu centos redhat（收费，红帽认证工程师）

### linux应用领域

linux系统从嵌入式设备到超级计算机，在服务器端确定了地位

通常服务器使用LAMP（linux+Apache+MySql+PhP）或者LNMP（linux+Nginx+MySql+PhP）组合

### linux环境搭建

本机电脑通过虚拟机vmware安装

1. 下载vmware软件
2. 下载centos镜像（linux磁盘分区的时候需要注意分区名即可！！/boot  /home）

购买云服务器

1. 购买阿里云/腾讯云服务器
2. 获取服务器的ip并且重置密码，进行远程登录（linux防火墙的端口开完之后，一定要在阿里云设置安全组规则！开放端口号，否则外界无法访问）
   端口说明：21/21(ftp)   443/443(https)   80/80(http)   22/22(ssh)   3306/3306(mysql)   8080/8080(tomcat)    6379/6389(Redis)
3. 下载xShell工具实现远程登录服务器，进行环境搭建

### 认识linux系统

#### 开机登录

开机会启动许多程序，这些程序在window在服务，在linux叫做**守护进程**

用户登录的方式有三种：

1. 命令行登录（原生登录）
2. ssh登录（使用xShell工具远程登录）
3. 图形界面登录（虚拟机登录）

最高权限账户为root，可以操作一切！

#### 关机

一般linux服务器很少关机，除非遇到特殊情况

关机指令为：shutdown

```bash
sync  #将数据由内存同步到硬盘中
shutdown #关机指令，你可以man shutdown来看一下帮助文档
shutdown -h 10  #10分钟后关机
shutdown -h now  #现在关机
shutdown -h 20:25  #系统在今天的20:25分关机
shutdown -h +10  #10分钟后关机
shutdown -r now  #系统里面重启
shutdown -r +10  #系统10分钟后重启
reboot #系统重启指令
halt  #关闭系统===shutdown -h now/poweroff
```

**总结一下，不管是哪个关机重启指令，首先要运行sync命令，把内存中的数据写到磁盘中**

#### 系统目录结构

1. linux中一切皆文件

2. 根目录是`/`,所有文件在挂载在这个节点下面（文件目录是树状的）
   登录系统后，在当前命令窗口输入命令：

   ```bash
   #查看根目录下的文件
   ls /
   ```

3. 以下是对这些目录的解释：

   ./bin: bin是Binary的缩写，这个目录存放着最经常使用的命令。
   ·/boot：这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。
   ·/dev: dev是Device（设备）的缩写，存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。
   **·/etc：这个目录用来存放所有的系统管理所需要的配置文件和子目录。**
   **·/home：用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。**
   ·/lib：这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。
   ·/lost+found：这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。
   ·/media:  linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。
   ·/mnt：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。
   **·/opt：这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。**
   ·/proc：这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。
   **·/root：该目录为系统管理员，也称作超级权限者的用户主目录。**
   ·/sbin:s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。
   ·/srv：该目录存放一些服务启动之后需要提取的数据。
   ·/sys：这是inux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统sysfs。
   **·/tmp：这个目录是用来存放一些临时文件的。**
   **·/usr：这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。**
   ·/usr/bin：系统用户使用的应用程序。
   ·/usr/sbin：超级用户使用的比较高级的管理程序和系统守护程序。
   ·/usr/src：内核源代码默认的放置目录。
   **·/var：这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。**
   ·/run：是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。
   ./www  存放服务器网站相关的资源

   

### 常用的基本命令

#### 目录管理

相对路径-——绝对路径

绝对路径是指路径的全称：C:\program\

> cd：切换目录

cd 目录名（绝对路径都是以`/`开头，对于当前目录该如何查找`../../`）

```bash
cd home  # 进入home目录
cd ../user #  相对路径进入user目录
cd / 
cd /home/kuangshen  # 绝对路径跳转
cd ~ # 回到当前的用户目录
```

> ls ：列出目录

-a参数：all，查看全部文件，包括隐藏文件
-l参数：列出所有的文件，包含文件的属性和权限，没有隐藏文件

> pwd：显示当前用户所在的目录

```bash
pwd
```

> mkdir：创建一个目录

```bash
mkdir test1  # 创建目录
nkdir -p test2/test3/test4  #递归创建目录
```

> rmdir：删除目录

rmdir只能删除空的目录，如果文件夹里面存在文件需要先删除文件，递归删除多个目录，加上`-p`参数即可

> cp：复制文件或者目录

cp 原来的地方 to 新的地方 

```bash
cp install.sh kuangshen  #拷贝文件到狂神目录下面
```

> rm：移除文件或目录

-f 参数：忽略不存在的文件，强制删除
-r参数：递归删除目录
-i参数：删除询问是否删除

```bash
rm -rf /  #系统中所有的文件都被删除了，删库跑路的操作！！
```

> mv：移动文件或者目录 重命名文件

-f参数：强制移动
-u参数：只替换已经更新过的文件

```bash
mv install.sh kuangshen/  # 移动文件
mv kuangshenstudy kuangshenstudy2 # 重命名文件夹		
```

#### 文件属性查看修改

> 文件属性说明

Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，Linux系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。

在Linux中我们可以使用`ll`或者`ls –l`命令来显示一个文件的属性以及文件所属的用户和组

```bash
ls -l
```

在Linux中第一个字符代表这个文件是目录、文件或链接文件等等：

- 当为[ **d** ]则是目录
- 当为[ **-** ]则是文件；
- 若是[ **l** ]则表示为链接文档 ( link file )；
- 若是[ **b** ]则表示为装置文件里面的可供储存的接口设备 ( 可随机存取装置 )；
- 若是[ **c** ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标 ( 一次性读取装置 )。

接下来的字符中，以三个为一组，且均为『rwx』 的三个参数的组合。
其中，[ r ]代表可读(read)、[ w ]代表可写(write)、[ x ]代表可执行(execute)。

要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号[ - ]而已。
每个文件的属性由左边第一部分的10个字符来确定

从左至右用0-9这些数字来表示。
第0位确定文件类型，第1-3位确定属主（该文件的所有者）拥有该文件的权限。第4-6位确定属组（所有者的同组用户）拥有该文件的权限，第7-9位确定其他用户拥有该文件的权限。

> 修改文件属性

1. chgrp：更改文件属组

   ```bash
   chgrp [-R] 属组名 文件名
   # -R：递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改
   ```

2. chown：更改文件属主，也可以同时更改文件属组

   ```bash
   chown [–R] 属主名 文件名
   chown [-R] 属主名：属组名 文件名
   ```

3. chmod：更改文件9个属性

   ```bash
   chmod [-R] xyz 文件或目录
   ```

4. Linux文件属性有两种设置方法，一种是数字，一种是符号。
   Linux文件的基本权限就有九个，分别是owner/group/others三种身份各有自己的read/write/execute权限。

   先复习一下刚刚上面提到的数据：文件的权限字符为：『-rwxrwxrwx』， 这九个权限是三个三个一组的！其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下：

   ```bash
   r:4     w:2         x:1
   ```

   每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为：[-rwxrwx---] 分数则是：

   - owner = rwx = 4+2+1 = 7
   - group = rwx = 4+2+1 = 7
   - others= --- = 0+0+0 = 0

   ```bash
   chmod 770 filename
   ```

   

#### 文件内容查看

Linux系统中使用以下命令来查看文件的内容：

- cat 由第一行开始显示文件内容
- tac 从最后一行开始显示，可以看出 tac 是 cat 的倒着写！
- nl  显示的时候，顺道输出行号！
- more 一页一页的显示文件内容
- less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
- head 只看头几行
- tail 只看尾巴几行

#### linux链接概念

> 硬链接	

A——B，假设B是A的硬链接，那么他们两个指向的同一个文件！允许同一个文件拥有多个路径，用户可以用过这种机制建立硬链接到一些重要的文件上，防止误删！

> 软链接

类似window下的快捷方式，删除了源文件，快捷方式也无法访问

```bash
touch f1  # 创建一个文件
In f1 f2  #创建一个硬链接f2
In -s f1 f3  #创建一个软链接（符号链接）f3
```

#### vim编辑器

>  三种模式

基本上 vi/vim 共分为三种模式，分别是**命令模式（Command mode）**，**输入模式（Insert mode）**和**底线命令模式（Last line mode）**

> 命令模式

用户刚刚启动 vi/vim，便进入了命令模式

以下是常用的几个命令：

- **i** 切换到输入模式，以输入字符。
- **x** 删除当前光标所在处的字符。
- **:** 切换到底线命令模式，以在最底一行输入命令

> 输入模式

在命令模式下按下i就进入了输入模式。

在输入模式中，可以使用以下按键：

- **字符按键以及Shift组合**，输入字符
- **ENTER**，回车键，换行
- **BACK SPACE**，退格键，删除光标前一个字符
- **DEL**，删除键，删除光标后一个字符
- **方向键**，在文本中移动光标
- **HOME**/**END**，移动光标到行首/行尾
- **Page Up**/**Page Down**，上/下翻页
- **Insert**，切换光标为输入/替换模式，光标将变成竖线/下划线
- **ESC**，退出输入模式，切换到命令模式

> 底线命令模式

在命令模式下按下:（英文冒号）就进入了底线命令模式。

底线命令模式可以输入单个或多个字符的命令，可用的命令非常多。

在底线命令模式中，基本的命令有（已经省略了冒号）：

- q 退出程序
- w 保存文件

按ESC键可随时退出底线命令模式。

#### 账号管理

实现用户账号的管理，要完成的工作主要有如下几个方面：

- 用户账号的添加、删除与修改。
- 用户口令的管理。
- 用户组的管理

> 用户账号的管理

用户账号的管理工作主要涉及到用户账号的添加、修改和删除。

添加用户账号就是在系统中创建一个新账号，然后为新账号分配用户号、用户组、主目录和登录Shell等资源

> 添加账号 useradd

```
useradd 选项 用户名
```

参数说明：

- 选项 :

- - -c comment 指定一段注释性描述。
  - -d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
  - -g 用户组 指定用户所属的用户组。
  - -G 用户组，用户组 指定用户所属的附加组。
  - -m　使用者目录如不存在则自动建立。
  - -s Shell文件 指定用户的登录Shell。
  - -u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。

- 用户名 :

- - 指定新账号的登录名

```bash
 useradd -m kuangshen
 # 此命令创建了一个用户kuangshen，其中-m选项用来为登录名kuangshen产生一个主目录 /home/kuangshen
```

> 删除用户

如果一个用户的账号不再使用，可以从系统中删除。
删除用户账号就是要将/etc/passwd等系统文件中的该用户记录删除，必要时还删除用户的主目录。
删除一个已有的用户账号使用userdel命令，其格式如下：

```
userdel 选项 用户名
```

常用的选项是 **-r**，它的作用是把用户的主目录一起删除

```
userdel -r kuangshen
```

> 修改用户

修改用户账号就是根据实际情况更改用户的有关属性，如用户号、主目录、用户组、登录Shell等。

修改已有用户的信息使用usermod命令

```
usermod 选项 用户名
```

常用的选项包括-c, -d, -m, -g, -G, -s, -u以及-o等，这些选项的意义与useradd命令中的选项一样，可以为用户指定新的资源值

```bash
usermod -s /bin/ksh -d /home/z –g developer kuangshen
# 此命令将用户kuangshen的登录Shell修改为ksh，主目录改为/home/z，用户组改为developer
```


> 切换用户

1.切换用户的命令为：su username 【username是你的用户名哦】

2.从普通用户切换到root用户，还可以使用命令：sudo su

3.在终端输入exit或logout或使用快捷方式ctrl+d，可以退回到原来用户，其实ctrl+d也是执行的exit命令

4.在切换用户时，如果想在切换用户之后使用新用户的工作环境，可以在su和username之间加-，例如：【su - root】

$表示普通用户

\#表示超级用户，也就是root用户

> 用户的密码设置问题

我们一般通过root创建用户的时候，要配置密码

通过passwd指令

可使用的选项：

- -l 锁定口令，即禁用账号。
- -u 口令解锁。
- -d 使账号无口令。
- -f 强迫用户下次登录时修改口令。

如果默认用户名，则修改当前用户的口令。


> 锁定用户

需要root权限 

 比如张三辞职了，需要冻结这个账号

```bash
passwd -l userName  #锁定这个用户以后就不能删除了
passwd -d userName  # 删除密码，删除后也不能登录
```



#### 用户组管理

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同Linux 系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。

用户组的管理涉及用户组的添加、删除和修改。
组的增加、删除和修改实际上就是对/etc/group文件的更新。

> 创建一个用户组 groupadd

创建完一个用户组后可以得到一个组id，这个id是指定的，如果不指定就自增1

```bash
groupadd -g 520 kuangshen 
```

> 删除用户组 groupdel

```bash
groupdel kuangshen
cat /etc/group
```

> 修改用户组的权限信息和名字 groupmod

```bash
groupmod -g 666 -n newkuangshen kuangshen
# 修改用户组的id和名字
```

> 切换用户组

```bash
#登录当前用户
newgrp root
```

#### 磁盘管理

> df (列出文件系统整体的磁盘使用量)    du （检查磁盘空间使用量）

```
df/ df -h
```

#### 进程管理

> 基本概念

1. 在linux中，每一个程序都有自己的一个进程，每一个进程都有一个id号
2. 每一个进程，都会有一个父进程
3. 进程可以有两种方式存在，一种是前台一种是后台
4. 正常情况都是服务在后台，基本的程序在前台

> 命令 

ps：查看当前系统中正在执行的各种进程信息

参数：

1. -a：显示当前终端运行的所有进程信息
2. -u：以用户的信息显示进程
3. -x：显示后台运行进程的参数

```bash
ps -aux|grep 进程名
#过滤进程信息，查看指定的进程
```

```bash
ps -ef|grep mysql
#查看父进程的信息
```

### 环境安装

软件安装一般有三种方式，rpm安装，解压缩，yum在线安装

#### rpm安装jdk并上线项目

1. 下载jdk rpm（去oralce官网下载）
2. 安装java jdk

```bash
# 检测当前是否存在java环境
java -version
# 如果有的话需要卸载

# rpm -qa|grep jdk  检查jdk版本信息
# rpm -e --nodeps jdk  强制卸载

# 卸载完后就可以安装jdk
# rpm -ivk rmp包

#配置环境变量
# cd /etc/profile(所有关于环境变量的配置都在这个地方)
JAVA_HOME=/usr/java/jdk1.8.0_221-amd64
CLASSPATH=%JAVA_HOME%/lib:%JAVA_HOME%/jre/lib
PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
export PATH CLASSPATH JAVA_HOME

# 让新增的环境变量生效！
source /etc/profile
```

3. 发布一个java项目

```bash
# 开启防火墙端口
firewall-cmd--zone=public --add-port=9000/tcp --permanent
# 重启防火墙服务
systemctl restart firewalld.service
#查看所有开启的端口
firewall-cmd --list-ports
#把jar包打包后用xshell工具放到linux服务器上
#执行jar包
java -jar 包名(这样就在服务器上跑起来了)

#这时候就可以远程访问项目了
```



#### 解压缩安装tomcat并发布

ssm的war包就需要放到tomcat中运行

1. 下载tomcat（官网下载即可）
2. 解压这个文件

```
tar -zxvf apache-tomcat-9.022.tar.gz
```

3. 启动tomcat测试

```bash
#进入comcat的bin目录
#执行
./startup.sh
#停止
./shoutdown.sh
```

4. 如果防火墙的8080端口打开了并且阿里云安全组也放开了这个端口，这个时候就可以远程访问了

#### yum安装docker

yum属于在线安装，一定要联网

> 安装

1. 官网安装参考手册：https://docs.docker.com/install/linux/docker-ce/centos/
2. 确定你是CentOS7及以上版本

```bash
cat /etc/redhat-release
```

3. yum安装gcc相关（需要确保 虚拟机可以上外网 ）

```bash
yum -y install gcc
yum -y install gcc-c++
```

4. 卸载旧版本

```bash
yum -y remove docker docker-common docker-selinux docker-engine
```

5. 安装需要的软件包

```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
```

6. 设置stable镜像仓库(默认是国外的，这里请用国内的阿里云镜像)

```bash
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

7. 更新yum软件包索引

```
yum makecache fast
```

8. 安装Docker CE

```
yum -y install docker-ce docker-ce-cli containerd.io
```

9. 启动docker

```
systemctl start docker
```

10. 测试docker

```bash
docker version

docker run hello-world（从docker官网上拉去hello word的镜像）

docker images
```

#### 宝塔面板安装环境

可视化安装

#### 防火墙基本命令

```bash
# 查看firewall服务状态
systemctl status firewalld

# 开启、重启、关闭、firewalld.service服务
# 开启
service firewalld start
# 重启
service firewalld restart
# 关闭
service firewalld stop

# 查看防火墙规则
firewall-cmd --list-all    # 查看全部信息
firewall-cmd --list-ports  # 只看端口信息

# 开启端口
开端口命令：firewall-cmd --zone=public --add-port=80/tcp --permanent
重启防火墙：systemctl restart firewalld.service

命令含义：
--zone #作用域
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
--permanent   #永久生效，没有此参数重启后失效
```



> 域名解析

域名解析后，如果端口是80-http或者443-https，可以直接访问
如果是9000，8080，就需要通过Apache/nginx做一下反向代理

### 扩展 vmware使用

> 快照

保留当前系统信息为快照，随时可以恢复，防止未来系统被破坏
平时每配置好一个东西可以拍摄一个快照，保留信息



> 网络配置

需要保证虚拟机和本机处在同一个网段

```bash
#本地window网段
# 192.168.31.16

#虚拟机网段设置
# 192.168.31.1
```

### 后续知识

消息队列（kafka rabbitMQ rockeetMQ）
缓存（redis）
搜索引擎（ES）
大数据（hadoop）
容器docker