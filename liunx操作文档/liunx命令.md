# liunx

liunx查看ip

`ip addr`

`ifconfig`



**1.Linux服务启动命令**

service tomcat start 或 systemctl start tomcat

**2.Linux服务停止命令**

service tomcat stop 或 systemctl stop tomcat 此时tomcat需要做成服务启动

**3. Linux服务重启命令**

service tomcat restart 或 systemctl restart tomcat



**vi编辑**

Esc 退出编辑模式，输入以下命令：

:wq  保存后退出vi（常用）

:wq! 则为强制储存后退出

:w    保存但不退出（常用）

:w!   若文件属性为『只读』时，强制写入该档案

:q    离开 vi （常用）

:q!   若曾修改过档案，又不想储存，使用 ! 为强制离开不储存档案。

:e!   将档案还原到最原始的状态！

G	跳至最后一行

# root权限篇

1. 执行“sudo passwd -u root”，然后输入当前账户的密码

2. 行“sudo passwd root”，然后输入两次欲设置的root密码

3. 此时，新的root密码就已经设置好了。
   执行“su”后输入新的root密码，就可以获得root权限了。

4. root账户开启成功 ，退出root账户 exit

5. 设置登录面板，使其实现root登录

   进入 /usr/share/lightdm/lightdm.conf.d/

6. 编辑: 50-unity-greeter.config

   添加如下代码,保存退出

   user-session=ubuntu

   greeter-show-manual-login=true

   all-guest=false

7. 重启ubuntu-kulin，成功实现root登录，终端显示



# 防火墙篇

**一.Linux下开启/关闭防火墙命令**

1) 永久性生效，重启后不会复原

```
开启： chkconfig iptables on

关闭： chkconfig iptables off
```

2) 即时生效，重启后复原

```
开启： /etc/init.d/iptables start

关闭： /etc/init.d/iptables stop
```

3)开启相关端口

修改/etc/sysconfig/iptables 文件，添加以下内容：

```
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
```



**二.UBuntu关闭防火墙**

iptables -A INPUT -i !   PPP0   -j ACCEPT



**三.CentOS Linux 防火墙配置及关闭**

```
/sbin/iptables -I INPUT -p tcp –dport 80 -j ACCEPT

/sbin/iptables -I INPUT -p tcp –dport 22 -j ACCEPT

/etc/rc.d/init.d/iptables save
```

这样重启计算机后,防火墙默认已经开放了80和22端口

这里应该也可以不重启计算机：

```
/etc/init.d/iptables restart
```

查看防火墙信息：

```
/etc/init.d/iptables status
```

关闭防火墙服务：

```
/etc/init.d/iptables stop
```

永久关闭

```
chkconfig --level 35 iptables off
```



**四.centos 7 防火墙**

```
systemctl stop firewalld.service
systemctl disable firewalld.service
```

*centos 7 查看防火墙状态*

firewall-cmd --state





## Linux 下安装 Redis

下载并安装：

```
$ wget http://download.redis.io/releases/redis-2.8.17.tar.gz
$ tar xzf redis-2.8.17.tar.gz
$ cd redis-2.8.17
$ make
```

```
需要安装gcc工具；
安装gcc前执行`$sudo apt-get update`,若不成功再执行`$sudo apt-get clean：
$sudo apt-get clean
$sudo apt-get update
$sudo apt-get build-dep gcc`
按照上面处理就可以使用make命令了。
```

启动redis服务：

```
$ cd src
$ ./redis-server
```

通过启动参数告诉redis使用指定配置文件使用下面命令启动

```
$ cd src
$ ./redis-server ../redis.conf
```

启动redis服务进程后，就可以使用测试客户端程序redis-cli和redis服务交互了

```
$ cd src
$ ./redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```



## Linux 下安装 zookeeper

下载zookeeper源码包

```
$ wget http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz
$ tar -zxvf zookeeper-3.4.14.tar.gz
```

配置

```
进入conf目录：
$  cd zookeeper-3.3.6/conf/
$  ls
$ configuration.xsl  log4j.properties  zoo_sample.cfg

拷贝zoo_samle.cfg为zoo.cfg：
$  cp zoo_sample.cfg zoo.cfg
$  ls
$ configuration.xsl  log4j.properties  zoo.cfg  zoo_sample.cfg
```

编辑zoo.cfg

```
单机模式:不做集群
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
dataDir=/usr/local/mycrosoftware/zookeeper/zookeeper-3.4.14/data
dataLogDir=/usr/local/mycrosoftware/zookeeper/zookeeper-3.4.14/log
# the port at which the clients will connect
clientPort=2181

集群模式:要做集群，内容如下(dataDir目录和server地址需改成你真实部署机器的信息)
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/root/zookeeper-3.3.6/data
clientPort=2181
server.0=192.168.0.109:2555:3555  
server.1=192.168.0.110:2555:3555  
server.2=192.168.0.111:2555:3555
```

启动

```
$ ./zkServer.sh start
JMX enabled ``by` `default
Using config: /root/zookeeper-3.3.6/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```

测试

```
$ ./zkCli.sh -server 127.0.0.1:2181
```

查看

```
$ ps -aux | grep 'zookeeper'    			#查看进程
$ netstat -anp|grep 2181              #查看zookeeper的端口号命令
$ bin/zkServer.sh stop                #zookeeper 的停止命令
$ bin/zkServer.sh status              #zookeeper 的状态查看命令
```