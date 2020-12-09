# Linux 命令



## iproute2



### 简介

Linux 中 iproute2 旨在取代 net-tools，功能强大，能轻松地执行一些网络管理任务。

<img src="Linux 命令.assets/1089507-20170205152617089-1009718040.jpg" alt="1089507-20170205152617089-1009718040"  style="float: left;" />



### 常用命令

| 选项      |                             作用                             |
| --------- | :----------------------------------------------------------: |
| link      | 网络设备配置命令，可以开启/禁用某个网络设备，改变mtu及mac地址等。 |
| addr      | 用于管理某个网络设备的协议有关的地址，与ip link 类似，不过增加了协议有关的管理（例如：增加IP） |
| addrlabel | IPV6的地址标签，主要用于FRC3484中描述的IPV6地址的选择。RFC3484主要介绍了两个算法，用于IPV6地址的选择策略。 |
| rule      |                   管理路由，添加与删除等。                   |
| neigh     |       用于neighbor/ARP表的管理。如显示，插入，删除等。       |
| tunnel    | 隧道配置，隧道的作用是将数据封装成IP包然后在互联网上将包发出。 |
| maddr     |                         多播地址管理                         |
| mroute    |                         多播路由管理                         |
| monitor   |         状态监控，如可以支持监控IP地址何路由的状态。         |
| xfrm      | 设置xfrm。xfrm是一个IP框架，可以转换数据报的格式，如用某个算法对数据包进行加密。 |
|           |                                                              |



#### 设置 IP

```shell
#添加IP地址
ip addr add 192.168.17.30/24 dev eth0 
ip addr del 192.168.19.30/24 dev eth0
```

注意 IP 地址是CIDR（无类别域间路由选择，有时也被称作“超网络”。是一种分派和指定网络地址的方法。）格式 用于显示所用的子网掩码。



#### 路由查询

路由表说明：

默认情况下，系统有三个路由表：

**local**: 路由表local包含本机路由及广播信息。例如，在本机上执行ssh 127.0.0.1时，就会参考这份路由表的内容。在正常情况下，只要配置好网卡的网络设置，就会自动生成local路由表的内容，不用我们关注。

**main**：使用ip r 看到的路由表就是main的内容了。Linux系统在默认情况下使用这份路由表的内容来传输数据包。因此，排查问题主要是看此表。

**default**： 最后是default 表，这个路由表在默认情况下内容为空，一般不使用。

```shell
ip r
ip route show
ip route show table main
ip route get 192.168.12.120
```

#### 添加路由

```shell
ip route add default via 192.168.1.1
ip route add 192.168.2.0/24 via 192.168.2.1 table main
#查看当前的RPDB
ip rule show

ip route add from 192.168.3.0/24 table 10
```







## sed





# Linux 常用操作



## 用户相关操作

#### 1、添加用户

```shell
#
useradd wyj
useradd -g root wyj #.....................添加用户并指定用户组
useradd -d /home/wyj wyj #................添加用户并指定home目录
useradd wyj -u 666 #......................添加用户并指定user ID
useradd -r wyj #..........................添加系统用户
```

#### 2、修改用户

```shell
#
passwd wyj
echo 'wyj:520314' | chpasswd #............强制修改密码
usermod -g root wyj #.....................修改用户组属性（usermod 修改参数同useradd）
```

#### 3、查看用户密码有效期

```shell
#
[root@control-plane ~]# chage -l wyj
最近一次密码修改时间                                    ：9月 17, 2018
密码过期时间                                    ：从不
密码失效时间                                    ：从不
帐户过期时间                                            ：从不
两次改变密码之间相距的最小天数          ：0
两次改变密码之间相距的最大天数          ：99999
在密码过期之前警告的天数        ：7

# chage的参数包括：
# -m 密码可更改的最小天数。为零时代表任何时候都可以更改密码。
# -M 密码保持有效的最大天数。
# -W 用户密码到期前，提前收到警告信息的天数。
# -E 帐号到期的日期。过了这天，此帐号将不可用。
# -d 上一次更改的日期
# -i 停滞时期。如果一个密码已过期这些天，那么此帐号将不可用。
# -l 例出当前的设置。由非特权用户来确定他们的密码或帐号何时过期。

#修改密码不过期：
chage -M 99999
#或者修改shadow文件：
[root@control-plane ~]# cat /etc/shadow | grep wyj
wyj:$6$mY0gtbQH$uQZvqQ/lFfs42sVetupO0fPhhdsqSq7WUoSXgt1E81Yy053qjQ7WiEb1o6OaC.MHHhVcm21o0066Q.O/vaIAp0:17791:0:99999:7:::
# shadow 文件中 99999 表示密码不过期，或者为空也不过期
```

#### 4、切换群组

```shell
newgrp root 
#newgrp 指令类似 login 指令，当它是以相同的帐号，另一个群组名称，再次登入系统。欲使用 newgrp 指令切换群组，您必须是该群组的用户，否则将无法登入指定的群组。单一用户要同时隶属多个群组，需利用交替用户的设置。若不指定群组名称，则 newgrp 指令会登入该用户名称的预设群组。
```

#### 5、/etc/sudoers 文件

```shell
root    ALL=(ALL)  ALL                 # 系统默认
user1   ALL=(ALL)  ALL                 # user1 可以使用最高权限 需要输入用户密码
user2   ALL=(ALL)  NOPASSWD : ALL      # user2 可以使用最高权限 不需要输入用户密码

# 如果设置用户组前面加 %

%group1  ALL=(ALL)  ALL                # 用户组group1下所有用户可以使用最高权限
```

##### sudo使用 　

​		Linux是多用户多任务的操作系统, 共享该系统的用户往往不只一个。出于安全性考虑, 有必要通过useradd创建一些非root用户, 只让它们拥有不完全的权限; 如有必要，再来提升权限执行。
　　sudo就是来解决这个需求的: 这些非root用户不需要知道root的密码，就可以提权到root，执行一些root才能执行的命令。
　　执行**sudo -u <用户名> <命令>,** 将允许当前用户，提权到<用户名>的身份，再执行后面的<命令>,  即使<命令>原本需要root权限。提权到<用户名>身份时，是以<用户名>的身份来执行命令的，因此创建的文件默认属于<用户名>用户。
　　因此，当userB执行如下命令时: 

``` shell
sudo -u userA touch /tmp/belong-to-who.tmp #创建的/tmp/belong-to-who.tmp 文件属于用户userA
```

​		如果不带-u, 则默认使用root用户，而大多数时候sudo都是要提权到root的，所以-u <用户名>可以省略为: `sudo <命令>` 　　

​		需要注意的是: 执行sudo时输入的密码是当前用户的密码, 并非<用户名>的密码。 　

##### sudo 与 su

​	sudo -u <用户名>：需要输入当前用户的密码，提权到<用户名>身份执行命令后返回当前用户;
​	su - <用户名>：则是输入目标用户的密码，切换到目标用户。  

#####  sudo命令执行过程 

　	将当前用户切换到超级用户下，或切换到指定的用户下，然后以超级用户或其指定切换到的用户身份执行命令，执行完成后，直接退回到当前用户。 
　　具体工作过程如下： 
　　当用户执行sudo时，系统会主动寻找/etc/sudoers文件，判断该用户是否有执行sudo的权限 
　　–>确认用户具有可执行sudo的权限后，让用户输入用户自己的密码确认 
　　–>若密码输入成功，则开始执行sudo后续的命令 赋予用户sudo操作的权限 　通过useradd添加的用户，并不具备sudo权限。在ubuntu/centos等系统下, 需要将用户加入admin组或者wheel组或者sudo组。以root用户身份执行如下命令, 将用户加入wheel/admin/sudo组: 

```shell
usermod -a -G wheel <用户名> #如果提示wheel组不存在, 则还需要先创建该组
groupadd wheel 
```

#####  /etc/sudoers内容详解 　

sudo的权限控制可以在/etc/sudoers文件中查看到。一般来说，通过cat /etc/sudoers指令来查看该文件, 会看到如下几行代码: 

```shell
root ALL=(ALL:ALL) ALL
%wheel ALL=(ALL) ALL
%sudo  ALL=(ALL:ALL) ALL   　
```

对/etc/sudoers文件进行编辑的代码公式可以概括为: 

**授权用户/组  主机=[(切换到哪些用户或组)] [是否需要输入密码验证] 命令1,命令2,...**   

凡是[ ]中的内容, 都能省略; 命令和命令之间用,号分隔，为了方便说明, 将公式的各个部分称呼为字段1 - 字段5:

 授权用户/组 主机  =[(切换到哪些用户或组)] [是否需要输入密码验证] 命令1,命令2,...
字段1      字段2  =[(字段3)] [字段4] 字段5
字段3、字段4，是可以省略的。   　

在上面的默认例子中： 

   　**"字段1"**不以%号开头的表示"将要授权的用户", 比如例子中的root；以%号开头的表示"将要授权的组", 比如例子中的%wheel组 和 %sudo组。
　　**"字段2"**表示允许登录的主机, ALL表示所有; 如果该字段不为ALL,表示授权用户只能在某些机器上登录本服务器来执行sudo命令. 比如: 

```shell
jack mycomputer=/usr/sbin/reboot,/usr/sbin/shutdown 
#表示: 普通用户jack在主机(或主机组)mycomputer上, 可以通过sudo执行reboot和shutdown两个命令。"字段3"和"字段4"省略。
```

   　**"字段3"**如果省略, 相当于(root:root)，表示可以通过sudo提权到root; 如果为(ALL)或者(ALL:ALL),  表示能够提权到(任意用户:任意用户组)。请注意，"字段3"如果没省略,必须使用(  )双括号包含起来。这样才能区分是省略了"字段3"还是省略了"字段4"。
　　**"字段4"**的可能取值是NOPASSWD:。请注意NOPASSWD后面带有冒号:。表示执行sudo时可以不需要输入密码。比如: 

```shell
lucy ALL=(ALL) NOPASSWD: /bin/useradd
#表示: 普通用户lucy可以在任何主机上, 通过sudo执行/bin/useradd命令, 并且不需要输入密码.
peter ALL=(ALL) NOPASSWD: ALL
#表示: 普通用户peter可以在任何主机上, 通过sudo执行任何命令, 并且不需要输入密码。
```

　   **"字段5"**是使用逗号分开一系列命令,这些命令就是授权给用户的操作; ALL表示允许所有操作。
　　**命令都是使用绝对路径**, 这是为了避免目录下有同名命令被执行，从而造成安全隐患。如果你将授权写成如下安全性欠妥的格式: 

```shell
lucy ALL=(ALL) chown,chmod,useradd
#那么用户就有可能创建一个他自己的程序, 也命名为userad, 然后放在它的本地路径中, 如此一来他就能够使用root来执行这个"名为useradd的程序"。这是相当危险的!
```

命令的绝对路径可通过which指令查看到: 比如

```shell
which useradd
#可以查看到命令useradd的绝对路径: /usr/sbin/useradd
```

##### 编辑/etc/sudoers 　

​		在实践中,去编辑/etc/sudoers文件，系统提示我没权限啊，怎么办?这是因为/etc/sudoers的内容如此敏感，以至于该文件是只读的。所以，编辑该文件前，请确认清楚你知道自己正在做什么。
　　强烈建议通过visudo命令来修改该文件，通过visudo修改，如果配置出错，会有提示。
　　不过，系统文档推荐的做法，不是直接修改/etc/sudoers文件，而是将修改写在/etc/sudoers.d/目录下的文件中。如果使用这种方式修改sudoers，需要在/etc/sudoers文件的最后行，加上#includedir /etc/sudoers.d一行(默认已有): 

```shell
#includedir /etc/sudoers.d
```

 　　注意了，这里的指令#includedir是一个整体, 前面的#号不能丢，并非注释，也不能在#号后有空格。任何在/etc/sudoers.d/目录下，不以~号结尾的文件和不包含.号的文件，都会被解析成/etc/sudoers的内容。
　　文档中是这么说的:

```shell
# This will cause sudo to read and parse any files in the /etc/sudoers.d # directory that do not end in ‘~‘ or contain a ‘.‘ character. # Note that there must be at least one file in the sudoers.d directory (this # one will do), and all files in this directory should be mode 0440. # Note also, that because sudoers contents can vary widely, no attempt is # made to add this directive to existing sudoers files on upgrade. # Finally, please note that using the visudo command is the recommended way # to update sudoers content, since it protects against many failure modes.
```

#####  命令作用域 

```shell
papi ALL=(root) NOPASSWD: /bin/chown,/usr/sbin/useradd
#表示: 用户papi能在所有可能出现的主机上, 提权到root下执行/bin/chown, 不必输入密码; 但运行/usr/sbin/useradd 命令时需要密码.
```

　　   这是因为NOPASSWD:只影响了其后的第一个命令: 命令1。上面给出的公式只是简化版，完整的公式如下:

​		  授权用户/组 主机=[(切换到哪些用户或组)] [是否需要输入密码验证] 命令1, [(字段3)] [字段4] 命令2, ...   　

​		  在具有sudo操作的用户下, 执行sudo -l可以查看到该用户被允许和被禁止运行的命令.

##### 通配符以及取消命令 

```shell
papi ALL=/usr/sbin/*,/sbin/*,!/usr/sbin/fdisk
#命令前面加上!号表示取消该命令。该例子的意思是: 用户papi在所有可能出现的主机上, 能够运行目录/usr/sbin和/sbin下所有的程序, 但fdisk除外。
```

##### 输入密码时有反馈 　

​		当使用sudo后输入密码，并不会显示任何东西 —— 甚至连常规的星号都没有。有个办法可以解决该问题。打开/etc/sudoers文件找到下述一行: 

```shell
Defaults env_reset
```

　修改成: 

```shell
Defaults env_reset,pwfeedback
```

#####  修改sudo会话时间 

　	如果你经常使用sudo  命令，你肯定注意到过当你成功输入一次密码后，可以不用再输入密码就可以运行几次sudo命令。但是一段时间后，sudo  命令会再次要求你输入密码。默认是15分钟，该时间可以调整。添加timestamp_timeout=分钟数即可。时间以分钟为单位，-1表示永不过期，但强烈不推荐。
　　比如我希望将时间延长到1小时，还是打开/etc/sudoers文件找到下述一行: 

```shell
Defaults env_reset
```

　　

修改成: 

```shell
Defaults env_reset,pwfeedback,timestamp_timeout=60
```



##### 实践 　

针对MySQL数据库的设置，让test组中的test用户具备/etc/init.d/mysqld的权限： 

```shell
[root@test ~]# groupadd test 
[root@test ~]# useradd -g test -m -d /home/test -s /bin/bash test 
[root@test ~]# passwd test 
[root@test ~]# visudo 
# test ALL=(ALL) NOPASSWD: /etc/init.d/mysqld 
test ALL=(ALL) /etc/init.d/mysqld 
#start mysql  
[root@test ~]# su test 
[test@test ~]$ sudo /etc/init.d/mysqld start 
#stop mysql 
[root@test ~]# su test 
[test@test ~]$ sudo /etc/init.d/mysqld stop
```

　

针对tomcat的设置，让test组中的test用户具备tomcat操作的权限：

```shell
[root@test ~]# groupadd test 
[root@test ~]# useradd -g test -m -d /home/test -s /bin/bash test 
[root@test ~]# passwd test 

[root@test ~]# visudo 
# test ALL=(ALL) /usr/local/tomcat/bin/shutdown.sh,/usr/local/tomcat/bin/startup.sh 
test ALL=(ALL) NOPASSWD: /usr/local/tomcat/bin/shutdown.sh,/usr/local/tomcat/bin/startup.sh [root@test ~]# vim /usr/local/tomcat/bin/catalina.sh 
### JDK  
export JAVA_HOME=/usr/local/jdk export JRE_HOME=$JAVA_HOME/jre 
#start tomcat  
[root@test ~]# su test  
[test@test ~]$ sudo /usr/local/tomcat/bin/startup.sh [test@test ~]$ ss -ntlup | grep Java 
[test@test ~]$ curl -I http://localhost:8080 
#stop tomcat  
[root@test ~]# su test  
[test@test ~]$ sudo /usr/local/tomcat/bin/shutdown.sh
```

#####  **附录：（sudoers文件详解）**

```shell
[root@test ~]# cat /etc/sudoers 
## Sudoers allows particular users to run various commands as 
## the root user, without needing the root password. 
##该文件允许特定用户像root用户一样使用各种各样的命令，而不需要root用户的密码  
## 
## Examples are provided at the bottom of the file for collections ## of related commands, which can then be delegated out to particular ## users or groups. 
## 在文件的底部提供了很多相关命令的示例以供选择，这些示例都可以被特定用户或   
## 
## 用户组所使用   
## This file must be edited with the ‘visudo‘ command. 
## 该文件必须使用"visudo"命令编辑 
## Host Aliases 
#主机别名 
## Groups of machines. You may prefer to use hostnames (perhap using  ## wildcards for entire domains) or IP addresses instead. 
## 对于一组服务器，你可能会更喜欢使用主机名（可能是全域名的通配符）
## 或IP地址代替，这时可以配置主机别名 
# Host_Alias     FILESERVERS = fs1, fs2 
# Host_Alias     MAILSERVERS = smtp, smtp2 
## User Aliases 
#用户别名 
## These aren‘t often necessary, as you can use regular groups 
## (ie, from files, LDAP, NIS, etc) in this file - just use %groupname  
## rather than USERALIAS 
## 这并不很常用，因为你可以通过使用组来代替一组用户的别名   
# User_Alias ADMINS = jsmith, mikem 

## Command Aliases 
## These are groups of related commands... 
## 指定一系列相互关联的命令（当然可以是一个）的别名，通过赋予该别名sudo权限，   
## 可以通过sudo调用所有别名包含的命令，下面是一些示例 
## Networking 
#网络操作相关命令别名   
Cmnd_Alias NETWORKING = /sbin/route, /sbin/ifconfig, /bin/ping, /sbin/dhclient, /usr/bin/net, /sbin/iptables, /usr/bin/rfcomm, /usr/bin/wvdial, /sbin/iwconfig,  /sbin/mii-tool 
## Installation and management of software 
#软件安装管理相关命令别名   
Cmnd_Alias SOFTWARE = /bin/rpm, /usr/bin/up2date, /usr/bin/yum 
## Services 
#服务相关命令别名  
Cmnd_Alias SERVICES = /sbin/service, /sbin/chkconfig 
## Updating the locate database 
#本地数据库升级命令别名   
Cmnd_Alias LOCATE = /usr/sbin/updatedb 
## Storage 
#磁盘操作相关命令别名 
Cmnd_Alias STORAGE = /sbin/fdisk, /sbin/sfdisk, /sbin/parted, /sbin/partprobe, /bin/mount, /bin/umount 
## Delegating permissions 
#代理权限相关命令别名  
Cmnd_Alias DELEGATING = /usr/sbin/visudo, /bin/chown, /bin/chmod, /bin/chgrp 
## Processes 
#进程相关命令别名 
Cmnd_Alias PROCESSES = /bin/nice, /bin/kill, /usr/bin/kill, /usr/bin/killall 
## Drivers 
#驱动命令别名 
Cmnd_Alias DRIVERS = /sbin/modprobe 
#环境变量的相关配置 
# Defaults specification 
# 
# Disable "ssh hostname sudo <cmd>", because it will show the password in clear.  
#         You have to run "ssh -t hostname sudo <cmd>". 
# 
Defaults    requiretty 
Defaults    env_reset 
Defaults    env_keep = "COLORS DISPLAY HOSTNAME HISTSIZE INPUTRC KDEDIR                         LS_COLORS MAIL PS1 PS2 QTDIR USERNAME                         LANG LC_ADDRESS LC_CTYPE LC_COLLATE LC_IDENTIFICATION                         LC_MEASUREMENT LC_MESSAGES LC_MONETARY LC_NAME LC_NUMERIC                         LC_PAPER LC_TELEPHONE LC_TIME LC_ALL LANGUAGE LINGUAS                         _XKB_CHARSET XAUTHORITY" 
## Next comes the main part: which users can run what software on ## which machines (the sudoers file can be shared between multiple ## systems). 
## 下面是规则配置：什么用户在哪台服务器上可以执行哪些命令（sudoers文件可以在多个系统上共享） 
## Syntax: 
##语法 
##      user    MACHINE=COMMANDS 
##  用户 登录的主机=（可以变换的身份） 可以执行的命令   
## 
## The COMMANDS section may have other options added to it.
## 命令部分可以附带一些其它的选项   
## 
## Allow root to run any commands anywhere  
## 允许root用户执行任意路径下的任意命令  
root    ALL=(ALL)       ALL 
## Allows members of the ‘sys‘ group to run networking, software, 
## service management apps and more. 
# %sys ALL = NETWORKING, SOFTWARE, SERVICES, STORAGE, DELEGATING, PROCESSES, LOCATE, DRIVERS 
## 允许sys中户组中的用户使用NETWORKING等所有别名中配置的命令 

## Allows people in group wheel to run all commands 
# %wheel        ALL=(ALL)       ALL 
## 允许wheel用户组中的用户执行所有命令   
## Same thing without a password 
## 允许wheel用户组中的用户在不输入该用户的密码的情况下使用所有命令 
# %wheel        ALL=(ALL)       NOPASSWD: ALL 
## Allows members of the users group to mount and unmount the 
## cdrom as root 
## 允许users用户组中的用户像root用户一样使用mount、unmount、chrom命令  
# %users  ALL=/sbin/mount /mnt/cdrom, /sbin/umount /mnt/cdrom 
## Allows members of the users group to shutdown this system 
# %users  localhost=/sbin/shutdown -h now 
## 允许users用户组中的用户像root用户一样使用shutdown命令
```

