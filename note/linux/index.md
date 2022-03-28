# Linux


# Linux学习

输入ifconfig 查询ip地址,以后用于远程连接,我的192.168.122.128,使用CRT去远程连接服务器.

## 常用命令

文件里有文档可以查

**cd**

**pwd** 

**li** 

**li -a**

**li -l  == ll**

**按tab可以补文件名称**

**cd .. 退回上一级**

**cd - 放回上一次的目录**

**mkdir  加上-p可以创建多级目录**

**rmdir  只能删除空文件**

**cat  查看全部**

**more 空格满屏 回车一行**

**less 上下键翻动 q退出**

**tail -f 动态查看实时监控**

**cp (copy) cp 文件名  粘贴的路径,可改名**

**mv (move)**

**./ 当前目录   ../上一级目录**

**rm 删除文件  回复y或者n**    

**rm -r 删除文件夹**

**rm -rf 删除且不询问**

**rm -f * 删除所有文件**

**rm -f /* root目录下所有文件**

**tar  打包或者解压**

![image-20211017094124374](https://i.loli.net/2021/10/17/vlfiUWw8JVzMF1Y.png)

**【find】命令**

find指令用于查找符合条件的文件

示例：

find / -name “ins*” 查找文件名称是以ins开头的文件

find / -name “ins*” –ls 

find / –user itcast –ls 查找用户itcast的文件

find / –user itcast –type d –ls 查找用户itcast的目录

find /-perm -777 –type d-ls 查找权限是777的文件

**【grep】命令**

查找文件里符合条件的字符串。

用法: grep [选项]... PATTERN [FILE]...示例：

grep lang anaconda-ks.cfg  在文件中查找lang

grep lang anaconda-ks.cfg –color 高亮显示

![img](https://i.loli.net/2021/10/17/CZV4kvQNtzW6cl3.jpg) 

 

![img](https://i.loli.net/2021/10/17/dF9vgZAnQrLMRGo.jpg) 

 

![img](https://i.loli.net/2021/10/17/NRqaSmY9dMA7svy.jpg)

**【touch】**

创建一个空文件

\* touch a.txt

**【clear/ crtl + L】**

清屏

## Vi和Vim编辑器

## **1．** ***\*Vim\*******\*编辑器：\****

在Linux下一般使用vi编辑器来编辑文件。vi既可以查看文件也可以编辑文件。三种模式：命令行、插入、底行模式。

切换到命令行模式：按Esc键；

切换到插入模式：按 i 、o、a键；

  i 在当前位置前插入

  I 在当前行首插入

  a 在当前位置后插入

  A 在当前行尾插入

  o 在当前行之后插入一行

  O 在当前行之前插入一行

 

切换到底行模式：按 :（冒号）；更多详细用法，查询文档《Vim命令合集.docx》和《vi使用方法详细介绍.docx》

 

打开文件：vim file

退出：esc à :q

修改文件：输入i进入插入模式

保存并退出：escà:wq

 

不保存退出：escà:q!

 

三种进入插入模式：

i:在当前的光标所在处插入

o:在当前光标所在的行的下一行插入

a:在光标所在的下一个字符插入

 

快捷键：

dd – 快速删除一行

yy - 复制当前行

nyy - 从当前行向后复制几行

p - 粘贴

R – 替换

## **2．** ***\*重定向输出\*******\*>\*******\*和\*******\*>>\****

\>  重定向输出，覆盖原有内容；>> 重定向输出，又追加功能；示例：

cat /etc/passwd > a.txt  将输出定向到a.txt中

cat /etc/passwd >> a.txt  输出并且追加

 

ifconfig > ifconfig.txt

## **3．** ***\*系统管理命令\****

ps 正在运行的某个进程的状态

ps –ef  查看所有进程

ps –ef | grep ssh 查找某一进程

kill 2868  杀掉2868编号的进程

kill -9 2868  强制杀死进程

 

## **4．** ***\*管道\****  |  

管道是Linux命令中重要的一个概念，其作用是将一个命令的输出用作另一个命令的输入。示例

ls --help | more  分页查询帮助信息

ps –ef | grep java  查询名称中包含java的进程

 

ifconfig | more

cat index.html | more

ps –ef | grep aio

# **一、** ***\*Linux\*******\*的权限命令\****

## **1．** ***\*文件权限\****

![img](https://i.loli.net/2021/10/17/yDOh3Lq1XWvw6jn.jpg) 

![image-20211017101039595](https://i.loli.net/2021/10/17/wsumHAktePjMgDi.png)

| 属主（user） | 属组（group） | ***\*其他用户\**** |      |      |      |      |      |      |
| ------------ | ------------- | ------------------ | ---- | ---- | ---- | ---- | ---- | ---- |
| r            | w             | x                  | r    | w    | x    | r    | w    | x    |
| 4            | 2             | 1                  | 4    | 2    | 1    | 4    | 2    | 1    |

r:对文件是指可读取内容 对目录是可以ls

 

w:对文件是指可修改文件内容，对目录 是指可以在其中创建或删除子节点(目录或文件)

 

x:对文件是指是否可以运行这个文件，对目录是指是否可以cd进入这个目录

 

## **2．** Linux三种文件类型：

普通文件： 包括文本文件、数据文件、可执行的二进制程序文件等。 

目录文件： Linux系统把目录看成是一种特殊的文件，利用它构成文件系统的树型结构。 

设备文件： Linux系统把每一个设备都看成是一个文件

## **3．** 文件类型标识

普通文件（-）目录（d）符号链接（l）

\* 进入etc可以查看，相当于快捷方式字符设备文件（c）块设备文件（s）套接字（s）命名管道（p）

## 4．文件权限管理：

​	chmod 变更文件或目录的权限。

chmod 755 a.txt 

chmod u=rwx,g=rx,o=rx a.txt

 

# 二、Linux上常用网络操作

## 1．主机名配置

hostname 查看主机名

hostname xxx 修改主机名 重启后无效

如果想要永久生效，可以修改/etc/sysconfig/network文件

## 2．IP地址配置

ifconfig 查看(修改)ip地址(重启后无效)

ifconfig eth0 192.168.12.22 修改ip地址

如果想要永久生效

修改 /etc/sysconfig/network-scripts/ifcfg-eth0文件

DEVICE=eth0 #网卡名称
BOOTPROTO=static #获取ip的方式(static/dhcp/bootp/none)

HWADDR=00:0C:29:B5:B2:69 #MAC地址
IPADDR=12.168.177.129 #IP地址
NETMASK=255.255.255.0 #子网掩码
NETWORK=192.168.177.0 #网络地址
BROADCAST=192.168.0.255 #广播地址
NBOOT=yes # 系统启动时是否设置此网络接口，设置为yes时，系统启动时激活此设备。

 

 

## **3．** 域名映射

/etc/hosts文件用于在通过主机名进行访问时做ip地址解析之用,相当于windows系统的C:\Windows\System32\drivers\etc\hosts文件的功能

![img](https://i.loli.net/2021/10/17/H8Z7iCofhbxgavc.jpg) 

## 4．网络服务管理

service network status 查看指定服务的状态

service network stop 停止指定服务

service network start 启动指定服务

service network restart 重启指定服务

 

service --status–all 查看系统中所有后台服务

netstat –nltp 查看系统中网络进程的端口监听情况

 

防火墙设置

防火墙根据配置文件/etc/sysconfig/iptables来控制本机的”出”、”入”网络访问行为。

service iptables status 查看防火墙状态

service iptables stop 关闭防火墙

service iptables start 启动防火墙

chkconfig  iptables off 禁止防火墙自启

 

 

# 三、Linux上软件安装

l Linux上的软件安装有以下几种常见方式介绍

\1. 二进制发布包

软件已经针对具体平台编译打包发布，只要解压，修改配置即可

\2. RPM包

软件已经按照redhat的包管理工具规范RPM进行打包发布，需要获取到相应的软件RPM发布包，然后用RPM命令进行安装

\3. Yum在线安装

软件已经以RPM规范打包，但发布在了网络上的一些服务器上，可用yum在线安装服务器上的rpm软件，并且会自动解决软件安装过程中的库依赖问题

\4. 源码编译安装

软件以源码工程的形式发布，需要获取到源码工程后用相应开发工具进行编译打包部署。

l 上传与下载工具介绍

\1. FileZilla

![img](https://i.loli.net/2021/10/22/znoXadAywsfJHlV.jpg) 

![img](https://i.loli.net/2021/10/17/Lo4qdvez3jcri59.jpg) 

\2. lrzsz

我们可以使用yum安装方式安装 yum install lrzsz

注意：必须有网络

可以在crt中设置上传与下载目录

![img](https://i.loli.net/2021/10/17/YX5btgJcV1BAs62.jpg) 

上传：

![img](https://i.loli.net/2021/10/17/dgnhluHJCVzb1vW.jpg) 

下载

![img](https://i.loli.net/2021/10/17/FZH7dGX2aKIW6sj.jpg) 

## **1．** 在Linux上安装JDK:

【步骤一】：上传JDK到Linux的服务器.

\* 上传JDK

\* 卸载open-JDK

 

java –version

rpm -qa | grep java

 

rpm -e --nodeps java-1.6.0-openjdk-1.6.0.35-1.13.7.1.el6_6.i686

rpm -e --nodeps java-1.7.0-openjdk-1.7.0.79-2.5.5.4.el6.i686

【步骤二】：在Linux服务器上安装JDK.

\* 通常将软件安装到/usr/local

\* 直接解压就可以

  tar –xvf  jdk.tar.gz  -C 目标路径 

![img](https://i.loli.net/2021/10/17/eBwyZVhuQfizpMA.jpg) 

【步骤三】：配置JDK的环境变量.

配置环境变量：

① vi /etc/profile

 

② 在末尾行添加

​	#set java environment

​	JAVA_HOME=/usr/local/jdk/jdk1.7.0_71

​	CLASSPATH=.:$JAVA_HOME/lib.tools.jar

​	PATH=$JAVA_HOME/bin:$PATH

​	export JAVA_HOME CLASSPATH PATH

保存退出

③source /etc/profile  使更改的配置立即生效

## **2．** 在Linux上安装Mysql:

【步骤一】：将mysql的安装文件上传到Linux的服务器.

 

![img](https://i.loli.net/2021/10/17/QIkxmy7tHFZjvGK.jpg) 

将mysql的tar解压

![img](https://i.loli.net/2021/10/17/69vy7YJFIiDLORA.jpg) 

将系统自带的mysql卸载

![img](https://i.loli.net/2021/10/17/6Z84xCvSVTwikUY.jpg) 

![img](https://i.loli.net/2021/10/17/VYOMU4qiPJ6nDxv.jpg) 

【步骤二】：安装MYSQL服务端

![img](https://i.loli.net/2021/10/17/wyao7OdcIeGAfbS.jpg) 

下面的提示是告诉我们root用户的密码第一次是随机生成的，它保存在/root/.mysql_secret中，第一次登录需要修改root密码

![img](https://i.loli.net/2021/10/17/YLnicHDZPeXGKWf.jpg) 



**server端安装不上的原因**:本来自带的mysql-lib未删除.

```
#  yum list | grep mysql
查询
# yum remove mysql-libs
然后就能安装上了
安装autoconf库
yum -y install autoconf
数据初始化
/usr/bin/mysql_install_db --user=mysql
在启动mysql
/usr/bin/mysqladmin -u root password 'new-password'设置新密码
```

【步骤三】：安装MYSQL客户端

![img](https://i.loli.net/2021/10/17/BLKSXEpedgmQoi9.jpg) 

查看生成的root密码

![img](https://i.loli.net/2021/10/17/coP7ZHWVxERuDAa.jpg) 

![img](https://i.loli.net/2021/10/17/y9FYlz3gDdRbIa6.jpg) 

报错:原因是没有启动mysql服务

需要开启mysql服务

![img](https://i.loli.net/2021/10/17/GlqQVjzErRkYXBn.jpg) 

执行下面操作报错，原因是第一次操作mysql必须修改root用户的密码

![img](https://i.loli.net/2021/10/17/6OfiWGvEZT5Bpgd.jpg) 

设置root用户的密码

![img](https://i.loli.net/2021/10/17/fEjWprAcXvgU2oh.jpg)  

 

l **Mysql****服务加入到系统服务并自动启动操作：**

chkconfig --add mysql

自动启动：

chkconfig mysql on

查询列表：

chkconfig

 

l 关于mysql远程访问设置

![img](https://i.loli.net/2021/10/17/y6c2aVTmbLUpu7N.jpg) 

在linux中很多软件的端口都被”防火墙”限止，我们需要将防火墙关闭

防火墙打开3306端口

/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT

/etc/rc.d/init.d/iptables save

/etc/init.d/iptables status

学习阶段我们也可以直接将防火墙关闭

service iptables stop;

## 3．在Linux上安装tomcat:

1.Tomcat上传到linux上

2.将上传的tomcat解压

3.在tomcat/bin目录下执行 startup.sh（注意防火墙）

4.查看目标 tomcat/logs/catalina.out

## **1．** ***\*在\*******\*Linux上安装redis\****

 

【步骤一】安装gcc-c++

redis是C语言开发，安装redis需要先将官网下载的源码进行编译，编译依赖gcc环境。

输入命令:

 yum install gcc-c++

 



|      |                                                           |
| ---- | --------------------------------------------------------- |
|      | ![img](https://i.loli.net/2021/10/17/2D4Hrwlbd63S9AO.jpg) |

 



 

​	输入y确认下载

![img](https://i.loli.net/2021/10/17/J6LbTMfjdEBPwYI.jpg) 

输入y确认安装

 



|      |                                                           |
| ---- | --------------------------------------------------------- |
|      | ![img](https://i.loli.net/2021/10/17/yizHQM1uK3gLtsS.jpg) |

 



安装 gcc 成功！



|      |                                                           |
| ---- | --------------------------------------------------------- |
|      | ![img](https://i.loli.net/2021/10/17/a6CtrEXLWSqmDZM.jpg) |

 



 

【步骤二】安装redis

\1. 下载redis

wget http://download.redis.io/releases/redis-3.0.4.tar.gz

 

\2. 解压

tar -xzvf redis-3.0.4.tar.gz

\3. 编译安装、

切换至程序目录，并执行make命令编译：

cd redis-3.0.4

make

 

执行安装命令

make PREFIX=/usr/local/redis install  

make install安装完成后，会在/usr/local/bin目录下生成下面几个可执行文件，它们的作用分别是：

redis-server：Redis服务器端启动程序

redis-cli：Redis客户端操作工具。也可以用telnet根据其纯文本协议来操作

redis-benchmark：Redis性能测试工具

redis-check-aof：数据修复工具

redis-check-dump：检查导出工具

【步骤三】配置redis

\1. 复制配置文件到/usr/local/redis/bin目录：

cd redis-3.0.4

cp redis.conf /usr/local/redis/bin

 

【步骤四】启动redis

\1. 进入redis/bin目录

cd redis/bin

 

启动redis服务端

./redis-server redis.conf

 

\2. 克隆新窗口，启动redis客户端

./redis-cli

 

## **2．** ***\*部署\*******\*项目到Linux\****

 

\1. 修改pom配置

在pom.xml中添加<finalName>

![img](https://i.loli.net/2021/10/17/PuCZkfL1jYSJI4B.jpg) 

 

 

 

 

 

​	修改jdk版本1.7

 

​	

  \2. 修改项目

​	2.1 druid.properties 

![img](https://i.loli.net/2021/10/17/b7S9jNBw5QgH4En.jpg)	

\3. 将war包上

​	2.2 header.html

![img](https://i.loli.net/2021/10/17/fwE2YoLsClWzVOp.jpg) 

 

 

1.3 route_de![img](https://i.loli.net/2021/10/17/bS8ojceFTnprHYx.jpg)2.3 route_detail.html

 

 

 

\4. 将war包上传到

 

\3. 使用package命令打包

![img](https://i.loli.net/2021/10/17/aeHcuRCDMNG1ZYy.jpg)  

\4. 将travel.war上传到tomcat中的webapps目录

\5. 重启tomcat

  \6. 导出本地mysql数据，并导入linux中的mysql

```
 Failed to execute goal org.apache.maven.plugins:maven-war-plugin:2.2:war (default-war) on project travel: Execution default-war of goal org.apache.maven.plugins:maven-war-plugin:2.2:war failed: Unable to load the mojo 'war' in the plugin 'org.apache.maven.plugins:maven-war-plugin:2.2' due to an API incompatibility: org.codehaus.plexus.component.repository.exception.ComponentLookupException: Cannot access defaults field of Properties
 
 报错,在pom.xml中添加下列代码之后就能打包了
 <build>
    <!-- To define the plugin version in your parent POM -->
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.3.1</version>
        </plugin>
      </plugins>
    </pluginManagement>
    <!-- To use the plugin goals in your POM or parent POM -->
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.3.1</version>
      </plugin>
    </plugins>
  </build>

```


