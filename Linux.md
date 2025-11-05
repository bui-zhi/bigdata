# Linux

## 一 内容回顾

## 二 教学目标

```
1. VMWare与CentOS7的安装
2. 虚拟机网络的配置与远程连接工具的使用
3. 了解linux系统及目录结构
4. 熟悉linux的基本命令操作
5. 熟练进行环境配置
6. 了解基本的用户管理,进程管理命令
7. 了解简单的权限设置
8. 熟练通过rpm,yum源进行软件安装
9. shell的介绍与运行环境和运行方式
10. 变量、read命令、数组、运算、测试
11. 条件控制、循环、方法
```



## 三 教学导读

```
在操作系统的阵营中，Linux始终占据着很大的一部分江山。Linux有着很多的发行版本，但在企业应用中，多数会采用CentOS作为服务器的操作系统。在本篇内容中，我们将讲解CentOS系统的使用。从最开始的操作系统的安装，到CentOS中常用的命令，到进程管理、权限管理、用户管理、软件安装等。通过学习本篇课程，大家可以掌握CentOS的基础使用，也为我们后续课程的学习打下扎实的基础。

本篇课程采用的CentOS的版本为7
```



## 四 教学内容

### 第一章 Linux简介【了解】

#### 1. Linux的介绍

Linux是一种自由和开放源码的操作系统，存在着许多不同的Linux发行版本，但它们都使用了[Linux内核](http://www.kernel.org)。

**Unix与Linux**

```
    20世纪60年代，美国的AT&T、MIT、G.E.合作，准备开发一款分时、多用户的操作系统，命名为MULTICS。后来，由于这个项目过于庞大，进度过于缓慢。因而，隶属于AT&T的贝尔实验室退出了这个项目。在贝尔实验室中，有一位大佬叫做Ken·Thompson(肯·汤普森)，在MULTICS中负责设计一个一款名为Space Traval的游戏。后来，随着MULTICS的搁置，这个游戏自然也就流产了。但是，肯汤普森又不想自己的游戏就这么消失，于是就想着把这个游戏移植出来。但是，没有操作系统啊，怎么办呢。。。
    肯·汤普森申请了一台当时已经报废的机器DEC PDP-7，参考着MULTICS的设计思路，反其道而行之，开发了一个操作系统，并把Space Traval成功的移植了过来。在给一个同事展示的时候，同事说: 这个操作系统的思想与MULTICS完全是相反的，干脆叫Unicas得了。于是，肯汤普森将这个操作系统命名为Unicas。
    贝尔实验室的另一位同事，Dennis·Ritchie(丹尼斯·里奇)在得知了这个操作系统后，想把它移植到其他的设备上。但是当时肯·汤普森在开发的时候是使用B语言来完成的，因此移植的难度非常高，基本上就是从头再做一份了。于是，丹尼斯·里奇与肯·汤普森，在原来的B语言的基础上，对其再封装改造，发明了C语言，并将这个Unicas重新编写，命名为UNIX。这一年是1969年。他们两个人后来也联合获得了图灵奖。
    
    1969年，UNIX诞生于贝尔实验室，同年，在芬兰诞生了一个婴儿，名为Linus Torvalds。
    
    Linus Torvalds长大后，在芬兰的赫尔辛基大学上学。上学期间，有一个教授编写了一本书叫做《操作系统》，讲述的是如何使用UNIX。但是遗憾是的当时的UNIX已经被AT&T商业化了，很不方便大学的授课。于是，这位教授仿照着UNIX，开发了一款操作系统，非常的短小精悍，命名为Minix。虽然功能与UNIX无法相比，但是足够校园使用，因而在大学里非常的流行。但是，由于Minix过于短小精悍，很多学生想写一些接口，来拓展Minix的功能。很遗憾，这位教授没有允许。于是，这些学生也很郁闷，其中就包括Linus Torvalds。
    于是，Linus Torvalds走上了许多前辈的老路:
    有操作系统吗？
    有！Minix!
    能用吗？
    不能，老师不让拓展！！
    怎么办？
    自己写！
    于是，Linus Torvalds仿照着Unix设计了一个当时还并不能称为操作系统的玩意，只有一个文件系统以及一些系统服务，并命名为Linux0.01，并将其开源。
    
    Linux就这样诞生了！
```

**Linux的应用领域**

```
- 企业服务器
  需要注意的是，企业服务器是不需要图形界面的。因为图形化占用的资源较多，且机房部署的时候也没有图形化的用武之地。
- 嵌入式系统
  Linux可安装在各种计算机硬件设备中，比如手机、平板电脑、路由器、智能家电等嵌入式系统。
- 电影娱乐行业
  电影后期特效处理。
```

**Linux的特点**

```
开源,多用户，多任务，丰富的网络功能，可靠的系统安全，良好的可移植性，具有标准兼容性，良好的用户界面，出色的速度性能
```



#### 2. Linux的两大阵营

- **RedHat系列：**RedHat，Suse，CentOS，Fedora等

    ```
    Red Hat：
    	RHEL(Redhat Enterprise Linux，也就是所谓的Redhat Advance Server收费版本. 红帽企业级Linux
    
    Fedora Core：
    	由原来的Redhat桌面版本发展而来，免费版本
    
    CentOS：
    	RHEL的社区克隆版本，免费
    
    Fedora: 
    	想尝试最先进的技术，等不及程序的稳定版出来。其实，Fedora就是红帽公司的一个测试平台；产品在成为企业级发行版之前，在该平台上进行开发和测试。
    
    SuSe：
    	最华丽的Linux发行版, 企业服务器端 X windows和程序应用方面做的确实不错。尤其与Microsoft的合作关系，应该是在所有的Linux发行版本中最亲密的。
    ```

    

- **Debian系列：**Debian，Ubuntu等

    ```
    Debian：
    	运行起来极其稳定，这使得它非常适合用于服务器
    	
    Ubuntu：
    	是Debian的一款衍生版，也是当今最受欢迎的免费操作系统, 最流行的linux桌面系
    ```

> 两大阵营，在命令上有略微的区别，同时安装的方式也略有不同。



#### 3. CentOS社区版介绍

```
主流：目前的Linux操作系统主要应用于生产环境，主流企业级Linux系统仍旧是RedHat或者CentOS

免费：RedHat 和CentOS差别不大，CentOS是一个基于Red Hat Linux 提供的可自由使用源代码的企业级Linux发行版本

更新方便：CentOS独有的yum命令支持在线升级，可以即时更新系统，不像RedHat 那样需要花钱购买支持服务！

CentOS官网：http://www.centos.org/

CentOS搜狐镜像：http://mirrors.sohu.com/centos/

CentOS网易镜像：http://mirrors.163.com/centos/

基于内核，再增加一些桌面，应用程序，就可以发行，称为开发版。
```



### 第二章 Linux的安装【重要】

#### 1. VMWare&Parallels Desktop的安装

##### 1.1. VMWare的简介

VMWare是一个虚拟PC的软件，可以在现有的操作系统上虚拟出一个新的硬件环境，相当于模拟出一台新的PC，以此来实现在一台机器上真正同时运行两个独立的操作系统。

VMware主要特点：

```
1 不需要分区或重新开机就能在同一台PC上使用两种以上的操作系统
2 本机系统可以与虚拟机系统网络通信
3 可以设定并且随时修改虚拟机操作系统的硬件环境
```

##### 1.2. VMWare安装的注意事项

```
1 安装路径下不能有中文，空格，特殊符号（尽量不要安装在C盘）,路径不能太深
2 如果想要卸载重新安装，双击其安装文件，选择卸载软件，尽量不要手动去删除其安装目录
3 如果使用手动删除VMware的安装目录进行卸载，要自行删除注册表中VMware的相关信息
```

##### 1.3. Parallels Desktop的简介

Parallels Desktop是Mac平台上的虚拟机软件，也是Mac平台最好的虚拟机软件之一。在Mac平台，可以使用Parallels Desktop模拟出一台新的PC，并实现在一台机器上真正同时运行两个独立的操作系统。安装比较简单，下载最新版本，直接傻瓜式的安装即可。注意，安装过后需要购买。



#### 2. VMWare安装Linux

##### 2.1. 准备事项

注意：在bios里面设置虚拟化 将 inter technology vatuializtion设置成 enable

虚拟机：VMWare workstation

虚拟机版本：CentOS-7-x86_64-Minimal-1908.iso

电脑: 笔记本或者台式机

系统版本: win7 win8.1 win10 等操作系统



##### 2.2. 安装

|                                                              |                                                 |
| ------------------------------------------------------------ | ----------------------------------------------- |
| ![image-20210325141111956](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141111956.png) | 01 选择“创建新的虚拟机”                         |
| ![image-20210325141142771](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141142771.png) | 02 选择“自定义”                                 |
| ![image-20210325141202135](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141202135.png) | 03 选择兼容性，默认的即可                       |
| ![image-20210325141317013](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141317013.png) | 04 选择镜像文件，可以在这里选择，也可以稍后安装 |
| ![image-20210325141345942](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141345942.png) | 05 选择操作系统                                 |
| ![image-20210325141400771](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141400771.png) | 06 选择虚拟机的名字和虚拟机存储的路径           |
| ![image-20210325141427758](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141427758.png) | 07 选择CPU核数                                  |
| ![image-20210325141452887](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141452887.png) | 08 选择内存，建议2G往上                         |
| ![image-20210325141511364](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141511364.png) | 09 选择网络连接方式，这里一定要选择“NAT”        |
| ![image-20210325141542204](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141542204.png) | 10 选择I/O控制器类型，默认即可                  |
| ![image-20210325141603874](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141603874.png) | 11 选择磁盘类型，默认即可                       |
| ![image-20210325141626731](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141626731.png) | 12 创建新虚拟磁盘                               |
| ![image-20210325141643174](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141643174.png) | 13 选择磁盘容量                                 |
| ![image-20210325141701559](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141701559.png) | 14 磁盘文件夹命名                               |
| ![image-20210325141723943](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141723943.png) | 15 最后的确认界面，在这里还可以修改硬件配置     |
| ![image-20210325141746547](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141746547.png) | 16 如果之前没有选择镜像，在这里选择镜像文件     |
| ![image-20210325141812565](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141812565.png) | 17 选择镜像文件                                 |
| ![image-20210325141826015](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325141826015.png) | 18 开启虚拟机                                   |
| ![image-20210325142426136](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142426136.png)<br />![image-20210325142521935](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142521935.png) | 19 如果出现黑屏的处理                           |



#### 3. Parallels Desktop安装Linux

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20210325142022884](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142022884.png) | 01 选择安装Windows或其他操作系统，然后继续                   |
| ![image-20210325142053518](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142053518.png) | 02 Parallels Desktop会自动扫描磁盘上的镜像文件，在这里选择指定的文件即可 |
| ![image-20210325142128028](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142128028.png) | 03 如果找不到磁盘文件，可以在这里手动选择，找到镜像文件拖拽到这里即可。或者可以点击选择文件去选择镜像文件 |
| ![image-20210325142218039](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142218039.png) | 04 取消“快速安装”                                            |
| ![image-20210325142237303](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142237303.png) | 05 选择虚拟机文件保存的位置，以及虚拟机的名字，并勾选“安装前设定”，进行硬件的自定义 |
| ![image-20210325142311680](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142311680.png) | 06 分配硬件，例如分配CPU和内存。                             |
| ![image-20210325142333003](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142333003.png) | 07 直接启动即可                                              |



#### 4. Linux的安装

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20210325142548406](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142548406.png) | 01 选择“Install CentOS7”                                     |
| ![image-20210325142612836](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142612836.png) | 02 选择安装界面的语言                                        |
| ![image-20210325142627821](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142627821.png) | 03 选择时区，这里选择上海                                    |
| ![image-20210325142647410](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142647410.png)<br />![image-20210325142728563](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142728563.png) | 04 选择安装类型，选择最小安装即可，不需要图形界面与其他的组件 |
| ![image-20210325142743880](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142743880.png) | 05 选择安装位置，自定义分区                                  |
| ![image-20210325142809385](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142809385.png) | 06 我要配置分区，进行自定义分区                              |
| ![image-20210325142830286](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142830286.png) | 07 添加分区的方式                                            |
| ![image-20210325142846787](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142846787.png) | 08 分区的说明                                                |
| ![image-20210325142921715](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142921715.png) | 09 启动分区                                                  |
| ![image-20210325142933056](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142933056.png) | 10 内存交换分区，一般设置为内存的2倍                         |
| ![image-20210325142953718](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325142953718.png) | 11 /分区                                                     |
| ![image-20210325143009533](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325143009533.png) | 12 格式化分区                                                |
| ![image-20210325143023725](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325143023725.png) | 13 手动连接网络                                              |
| ![image-20210325143157683](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325143157683.png) | 14 连接到网络，以及设置主机名字，主机的名字后续在Linux中可以修改 |
| ![image-20210325143236445](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325143236445.png) | 15 高级配置中的配置，Windows使用VMWare安装时候，使用的是ens33网卡；Mac使用Parallels Desktop安装的时候使用eth0网卡 |
| ![image-20210325143336388](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325143336388.png) | 16 开始安装                                                  |
| ![image-20210325143350140](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325143350140.png) | 17 配置用户                                                  |
| ![image-20210325143403665](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325143403665.png) | 18 设置用户的密码                                            |
| ![image-20210325143423384](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325143423384.png) | 19 重启系统                                                  |
| ![image-20210325143457082](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325143457082.png) | 20 重启之后，进入正常的操作界面                              |



#### 5. 学习的建议

```
- 转变思维习惯
    我们安装的是最小化安装，即Minimal版本，没有图形界面，只有命令行。因此我们需要使用命令来操作。
    基本没有鼠标的操作，只需要用键盘键入命令即可。

- Linux 与windows 主要区别：
	  Linux严格区分大小写
	  Linux中一切皆文件
    Linux不区分扩展名  扩展名是为了让管理员使用

- 不惧英文  勇往直前
    记住常用英文，如：command not found   Permission denied

- 动起来 练习
    多练多练多练，命令要敲的666，虚拟机环境可以随意折腾
```



#### 6. Linux目录简介

##### 6.1. Linux的目录树结构

![image-20210325143832656](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325143832656.png)



##### 6.2. 目录说明

| 目录        | 描述、说明                                                   |
| ----------- | ------------------------------------------------------------ |
| /           | Linux的根目录                                                |
| /bin        | binaries，存放系统命令的目录，所有用户都可以执行             |
| /sbin       | super user binaries，保存和系统环境设置相关的命令，只有超级用户可以使用这些命令，有些命令可以允许普通用户查看 |
| /usr/bin    | 存放系统命令的目录，所有用户都可以执行。这些命令和系统启动无关，单用户模式下不能执行 |
| /usr/sbin   | 存放根文件系统不必要的系统管理命令，超级用户可执行           |
| /root       | 存放root用户的相关文件,root用户的家目录。超级用户宿主目录    |
| /home       | 当前普通用户的文件夹,名字默认就是当前的用户，例如: /home/qianfeng |
| /cd         | 用户缺省宿主目录，例如: /home/qianfeng                       |
| /tmp        | temporary，存放临时文件                                      |
| /etc        | etcetera，存放系统配置文件                                   |
| /usr        | unix software resource，系统软件共享资源目录，存放所有命令、库、手册页等 |
| /proc       | 虚拟文件系统，数据保存在内存中，存放当前进程信息             |
| /boot       | 系统启动目录                                                 |
| /dev        | devices，存放设备文件                                        |
| /sys        | 虚拟文件系统，数据保存在内存中，主要保存与内存相关信息       |
| /lib        | 存放系统程序运行所需要的共享库                               |
| /lost+found | 存放一些系统出错的检查结果                                   |
| /var        | variavle，动态数据保存位置，包含经常发生变动的文件，如邮件、日志文件、计划任务等 |
| /mnt        | mount，挂载目录，临时文件系统的安装点，默认挂载光驱和软驱的目录 |
| /media      | 挂载目录，挂载媒体设备，如软盘和光盘                         |
| /misc       | 挂载目录，挂载NFS服务                                        |
| /opt        | 第三方安装的软件保存位置，习惯放在/usr/local目录下           |
| /src        | 服务数据目录                                                 |



#### 7. 三种网络连接方式

##### 7.1. 网络环境的相关概念

```
1 外网：IP是唯一性，不能重复。范围：0.0.0.0~255.255.255.255

2 内网：通过路由器或者交换器设备，来重新设置IP地址。
  不同的内网的ip可以相同。内网A的某一台主机通过自己的网卡与自己的路由器A通信，然后路由器A通过网络中心
  与路由器B通信，路由器B再与内网中的某一台机器的网卡通信。

3 IP: ip地址是确定一台机器的唯一标识符

4 NETMASK（子网掩码）:与ip连用，用于确定网络段位			
  192.168.1.x      范围：1-254
  255.255.255.0 	
  子网掩码有1的位置对应ip的部分就是网络段位。0对应的位置就是内网中主机的位置。
  子网掩码作用:用来指定当前机器属于哪个ip号段的
  判断方式:将当前的ip的二进制按位与子网掩码的二进制等于网段地址
  如: 192.168.1.2  &  255.255.255.0  = 192.168.1.0	
  
5 GATEWAY：网关,用于与连接外网的机器设备通信（路由器）换句话说，网关就是路由器的IP

6 DNS:域名解析服务器
  119.75.217.109  www.baidu.com
```



##### 7.2. 互联网DNS服务器的域名访问机制

![image-20210325164024982](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325164024982.png)

##### 7.3. 设备介绍

1 路由器

```
路由器是连接两个或多个网络的硬件设备，在网络间起网关的作用，是读取每一个数据包中的地址然后决定如何传送的专用智能性的网络设备。它能够理解不同的协议，例如某个局域网使用的以太网协议，因特网使用的TCP/IP协议。这样，路由器可以分析各种不同类型网络传来的数据包的目的地址，把非TCP/IP网络的地址转换成TCP/IP地址，或者反之；再根据选定的路由算法把各数据包按最佳路线传送到指定位置。所以路由器可以把非TCP/ IP网络连接到因特网上。
```

2 交换机

```
交换机（Switch）意为“开关”是一种用于电（光）信号转发的网络设备。它可以为接入交换机的任意两个网络节点提供独享的电信号通路。最常见的交换机是以太网交换机。其他常见的还有电话语音交换机、光纤交换机等。
```

3 Modem

```
调制解调器（英文名Modem），俗称“猫”，是一种计算机硬件.它能把计算机的数字信号翻译成可沿普通电话线传送的脉冲信号,而这些脉冲信号又可被线路另一端的另一个调制解调器接收，并译成计算机可懂的语言。

计算机内的信息是由“0”和“1”组成数字信号，而在电话线上传递的却只能是模拟电信号。于是，当两台计算机要通过电话线进行数据传输时，就需要一个设备负责数模的转换。
```

4 虚拟机

```
虚拟机（Virtual Machine）指通过软件模拟的具有完整硬件系统功能的、运行在一个完全隔离环境中的完整计算机系统。在实体计算机中能够完成的工作在虚拟机中都能够实现。在计算机中创建虚拟机时，需要将实体机的部分硬盘和内存容量作为虚拟机的硬盘和内存容量。每个虚拟机都有独立的CMOS、硬盘和操作系统，可以像使用实体机一样对虚拟机进行操作。
```



##### 7.4. 虚拟机网络连接方式

安装完虚拟机后，默认安装了如下图的两块虚拟网卡——VMnet1和VMnet8，其中VMnet1是host网卡，用于host方式连接网络；VMnet8是NAT网卡，用于NAT方式连接网络的，并且它们的IP地址是随机生成的。

VMware提供了三种工作模式，分别是bridged（桥接模式）、NAT（网络地址转换模式）和host-only（仅主机模式）。

VMware所在主机的设置,见下图

![image-20210325164145969](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325164145969.png)

###### 7.4.1. bridged（桥接模式）

在这种模式下，使用VMnet0虚拟交换机，虚拟操作系统就像是局域网中的一台独立的主机，与宿主计算机一样，它可以访问网内任何一台机器。在桥接模式下，可以手工配置它的TCP/IP配置信息（IP、子网掩码等，而且还要和宿主机器处于同一网段），以实现通过局域网的网关或路由器访问互联网，还可以将IP地址和DNS设置成“自动获取”。如果你想利用VMWare在局域网内新建一个服务器，为局域网用户提供Web或网络服务，就应该选择桥接模式。

在桥接模式中，使用VMnet0虚拟交换机，此时虚拟机相当于网络上的一台独立计算机与主机一样，拥有一个独立的IP地址。

![image-20210325164241392](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325164241392.png)



###### 7.4.2. NAT（网络地址转换模式）

使用NAT模式，就是让虚拟机借助NAT（网络地址转换）功能，通过宿主机器所在的网络来访问公网。也就是说，使用NAT模式可以实现在虚拟系统里访问互联网。NAT模式下的虚拟机的TCP/IP配置信息是由VMnet8虚拟网络的DHCP服务器提供的，因此IP和DNS一般设置为“自动获取”，因此虚拟系统也就无法和本局域网中的其他真实主机进行通讯。采用NAT模式最大的优势是虚拟系统接入互联网非常简单，你不需要进行任何其他的配置，只需要宿主机器能访问互联网即可。如果你想利用VMWare安装一个新的虚拟系统，在虚拟系统中不用进行任何手工配置就能直接访问互联网，建议你采用NAT模式。

NAT模式中使用Vmnet8虚拟交换机，此时虚拟机可以通过主机“单向访问”网络上的其他主机，其他主机不能访问虚拟机。

注意:主机处的192.168.62.1指的是主机的连接V8交换机的地址,与虚拟交换机处的网关必须是同一个地址

![image-20210325164310426](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325164310426.png)



###### 7.4.3. host-only（主机模式）

Host-Only模式其实就是NAT模式去除了虚拟NAT设备，然后使用VMware Network Adapter VMnet1虚拟网卡连接VMnet1虚拟交换机来与虚拟机通信的，Host-Only模式将虚拟机与外网隔开，使得虚拟机成为一个独立的系统，只与主机相互通讯。其网络结构如下图所示：

如果要使得虚拟机能联网，我们可以将主机网卡共享给VMware Network Adapter VMnet1网卡，从而达到虚拟机联网的目的。

![image-20210325164337269](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325164337269.png)



#### 8. 远程工具的使用

##### 8.1. SSH协议介绍

```
1 SSH 为 Secure Shell 的缩写，由 IETF 的网络小组（Network Working Group）所制定
2 SSH 为建立在应用层基础上的安全协议。

3 SSH 专为远程登录会话和其他网络服务提供安全性的协议,可以有效防止远程管理过程中的信息泄露问题。
4 SSH 由客户端和服务端两部分组成。服务端是一个sshd进程，提供密钥认证、密钥交换等处理。客户端包含     SSH程序以及scp(远程拷贝)、slogin(远程登陆)、sftp(安全文件传输) 
```

![image-20210325164437777](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325164437777.png)



##### 8.2. 远程工具的使用

```
常用的连接工具有putty、MobaXterm、SmarTTY、SecureCRT、XShell、finalshell等
我们选择MobaXterm，进行安装和使用
安装包：MobaXterm_Installer_v12.4.zip   下一步下一步即可
使用：启动MobaXterm后，参考下图得1，2，3步，进行填写。
```

![image-20210325164523257](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325164523257.png)



### 第三章 Linux的常用命令【重要】

#### 1. 命令格式的说明

```
命令格式：命令  [-选项][参数]  参数

eg: [root@qianfeng01 ~]# ls  -la  /usr

说明：	

大部分命令遵从该格式

多个选项时，可以一起写   

eg: [root@qianfeng01 ~]# ls –l –a  ls –la

简化选项与完整选项(注：并非所有选项都可使用完整选项) 	

eg: [root@qianfeng01 ~]# ls –all   ls –a

帮助命令：（相当于命令说明书）
```



#### 2. 帮助命令

##### 2.1. man

```
英文：manual 	命令路径：/usr/bin/man		执行权限：所有用户

作用：获取命令或配置文件的帮助信息

语法：man [命令/配置文件]

eg:[root@qianfeng01 ~]# man ls    man  services   

(查看配置文件时，不需要配置文件的绝对路径，只需要文件名即可）

调用的是more命令来浏览帮助文档，按空格翻下一页，按回车翻下一行，按q退出。
```



##### 2.2. help

```
help  查看shell内置命令的帮助信息

eg: [root@qianfeng01 ~]#help cd

命令名 --help

命令名 --help 列举该命令的常用选项

eg: [root@qianfeng01 ~]#cp --help
```



#### 3. 文件处理命令

##### 3.1. cd

```
英文：change directory	命令路径：内部命令	执行权限：所有用户

cd : 切换工作目录

cd -: 回到上一次的操作所在位置

cd ~: 回到用户家目录

cd ./ : 当前目录

cd ../: 回到上一级目录

cd   : 回到用户家目录
```



##### 3.2. ls

```
查看目录   ls + 选项 + 目录名

英文：list	命令路径：/bin/ls	执行权限：所有用户

ll 等价于ls -l

-l 列表形式显示                                     

eg: [root@qianfeng01 ~]# ls -l  /bin  查看根目录下的bin目录下的所有文件或目录以列表的形式查看

-a 所有文件或目录包括隐藏文件	        

eg: [root@qianfeng01 ~]# ll -a /bin 查看根目录下的bin目录下的所有文件或目录包含隐藏文件

-h 以可读的方式显示文件大小,配合-l使用

eg: [root@qianfeng01 ~]# ll  -h /bin 查看根目录下的bin目录下的所有文件或目录（显示文件大小单位，如KB等）
```



##### 3.3. pwd

```
英文：print working directory	命令路径：/bin/pwd		执行权限：所有用户

作用：显示当前工作目录

linux下的路径

pwd: 打印当前工作目录（全目录)

绝对路径: 以根目录为开始的目录

相对路径: 相对当前目录的路径

语法: pwd [-LP]   

eg: [root@qianfeng01 ~]# cd /etc/init.d

pwd [-P]
```



##### 3.4. mkdir

```
英文：make directories	命令路径：/bin/mkdir	执行权限：所有用户

作用：创建新目录

语法：mkdir [-p] 目录名

-p 父目录不存在情况下先生成父目录 （parents）

eg: [root@qianfeng01 ~]# mkdir linux/test  如果目录linux不存在，则报错，使用参数-p即可自动创建父目录
```



##### 3.5. touch

```
命令路径：/bin/touch		执行权限：所有用户

作用：创建空文件或更新已存在文件的时间      

语法：touch 文件名    

eg: [root@qianfeng01 ~]# touch  a.txt  b.txt   touch {a.txt,b.txt}    同时创建多个文件,注意:后面{}中,中间不能有空格

eg: [root@qianfeng01 ~]# touch "program files"  创建带空格的文件 

注意：生产环境中，文件名，一定不要加空格
```



##### 3.6. echo

```
命令路径：/bin/echo	执行权限：所有用户

作用：查看某些环境变量/给文件增加内容（将文件创建出来）

语法：echo '字符串'  > 文件名   

eg: 

[root@qianfeng01 ~]# echo  $PWD     查看当前的路径

[root@qianfeng01 ~]# echo  'haha'  >  test1.txt    将字符串haha添加到test1.txt里面,注意;会将文件原来的内容覆盖

[root@qianfeng01 ~]# echo 'haha'  >> test1.txt    将字符串haha追加到test1.txt中,不会覆盖原来的内容
```



##### 3.7. cp

```
英文：copy		命令路径：/bin/cp		执行权限：所有用户

作用：复制文件或目录

语法：cp [–rp]  源文件或目录	 目的目录

-r -R recursive 递归处理，复制目录

-p 保留文件属性 （原文件的时间不变）

eg:

1.相对路径  [root@qianfeng01 ~]# cp –r /etc/* .  将etc下的全部内容递归拷贝到当前的目录下 

2.绝对路径  [root@qianfeng01 ~]# cp –r /*ect/service  /root/test/aa/bb
```



##### 3.8. mv

```
英文：move 	命令路径：/bin/mv		执行权限：所有用户

作用：移动文件或目录、文件或目录改名

语法：mv 源文件或目录          目的目录
```



##### 3.9. rm

```
英文：remove 	命令路径：/bin/rm		执行权限：所有用户

作用：删除文件

语法： rm [-rf] 文件或目录

-r（recursive）删除目录，同时删除该目录下的所有文件

-f（force） 强制删除文件或目录 即使原档案属性设为唯读,亦直接删除,无需逐一确认

注意：工作中，谨慎使用rm –rf 命令。
```



##### 3.10. vi编辑器

```
vim/vi是Unix / Linux上最常用的文本编辑器而且功能非常强大。

只有命令，没有菜单。
```

![image-20210325165006776](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210325165006776.png)

```
命令模式：又称一般模式

编辑模式：又称底行模式，命令行模式

插入模式: 可以编辑文本
```

1. 插入命令

    | 命令          | 描述               |
    | ------------- | ------------------ |
    | a             | 在光标后附加文本   |
    | A (shift + a) | 在本行末尾附加文本 |
    | i             | 在光标前附加文本   |
    | I (shift + i) | 在本行行首附加文本 |
    | o             | 在光标下插入新行   |
    | O (shift + o) | 在光标上插入新行   |

    

2. 定位命令

    | 命令      | 描述           |
    | --------- | -------------- |
    | :set nu   | 显示行号       |
    | :set nonu | 取消行号显示   |
    | gg        | 跳转到第一行   |
    | G         | 跳转到最后一行 |
    | nG        | 跳转到第n行    |
    | :n        | 跳转到第n行    |

    

3. 保存和退出命令

    | 命令            | 描述                                                         |
    | --------------- | ------------------------------------------------------------ |
    | :w              | 保存修改的内容                                               |
    | :w file_name    | 另存为指定文件，如果文件不存在，会创建一个新的文件           |
    | :w >> file_name | 将内容追加到指定文件中，这个文件需要事先存在                 |
    | :wq             | 保存修改并退出                                               |
    | shift + zz (ZZ) | 保存修改并退出（快捷键）                                     |
    | :q!             | 不保存修改的内容并强制退出                                   |
    | :wq!            | 强制保存修改并退出（文件的所有者可以忽略只读权限，进行修改） |

    ```
    eg:
    1. 如果文本没有修改，可以使用:q退出
    2. 如果文本内容已经修改，无法使用:q进行退出，需要使用:q!强制退出
    3. 有突发的情况，导致窗口异常退出的时候，需要删除与文件同名的.swp文件
    ```

    

4. 删除命令

    | 命令    | 描述                                            |
    | ------- | ----------------------------------------------- |
    | x       | 删除光标所在处字符   nx 删除光标所在处后n个字符 |
    | dd      | 删除光标所在行，ndd删除n行                      |
    | :n1,n2d | 删除指定范围的行（eg :1,3d   删除了123这三行）  |
    | dG      | 删除光标所在行到末尾的内容                      |
    | D       | 删除从光标所在处到行尾                          |

    

5. 复制和剪切命令

    | 命令    | 作用                       |
    | ------- | -------------------------- |
    | yy、Y   | 复制当前行                 |
    | nyy、nY | 复制当前行以下n行          |
    | dd      | 剪切当前行                 |
    | ndd     | 剪切当前行以下n行          |
    | p       | 粘贴在当前光标所在行下一行 |
    | P       | 粘贴在当前光标所在行上一行 |
    
    
    
6. 替换和查找命令

    | 命令         | 描述                                |
    | ------------ | ----------------------------------- |
    | r            | 取代光标所在处字符                  |
    | R(shift + r) | 从光标所在处开始替换字符，按Esc结束 |
    | u            | undo,取消上一步操作                 |
    | ctrl+r       | redo,返回到undo之前                 |

    

7. 搜索和替换命令

    | 命令              | 作用                                      |
    | ----------------- | ----------------------------------------- |
    | /string           | 向后搜索指定字符串                        |
    | ?string           | 向前搜索指定字符串                        |
    | n                 | 搜索字符串的下一个出现位置,与搜索顺序相同 |
    | N（Shift + n）    | 搜索字符串的上一个出现位置,与搜索顺序相反 |
    | :%s/old/new/g     | 全文替换指定字符串                        |
    | :n1,n2s/old/new/g | 在一定范围内替换指定字符串                |

    ```
    注意:当遇到特殊字符时,需要起始行，终止行s/要替换的字符串/替换的新的字符串/g    将/g换成/c 询问确认
    
     eg: : %s/ftp/yang/g    全局替换，把ftp替换为yang
    
     eg: : 41,44/yang/lee/c   从41行到44行，把yang替换为lee,询问是否替换
    
     eg: : 41,44/yang/lee/g   同上，不询问，直接替换
    
     eg: : %s/\/root/\/ROOT/g  把/root替换为/ROOT,遇到特殊字符需要进行转义处理
    
     eg: : %s#/bin/bash#/bin/ksh#g        把/bin/bash全部替换为/bin/ksh进行转义处理, #之内不需要转义
    ```

    

8. 使用替换命令添删注释

    ```
    :% s/^/#/g 来在全部内容的行首添加 # 号注释 (^代表行首)
    
    :1,10 s/^/#/g 在1~10 行首添加 # 号注释
    
    vi里面查命令
    
    :!which cp
    
    vi里面导入命令的结果
    
    :r !which cp
    ```



##### 3.11. ln

- 英文： link 	命令路径：/bin/ln	执行权限：所有用户

- 作用：产生链接文件

- 语法：

    ```
    ln -s   [源文件][目标文件]    创建软链接      相当于建立快捷方式    注意:源文件 使用 绝对路径
    
    ln  [源文件][目标文件]    创建硬链接    相当于深拷贝
    
    eg: [root@qianfeng01 ~]# ln -s /etc/service  ./service.soft
    
    创建文件/etc/service的软链接service.soft
    
    eg: [root@qianfeng01 ~]# ln  /etc/service  /service.hard 
    
    创建文件/etc/service的硬链接/service.hard   
    ```

- 软连接类似于windows下的快捷方式

    ```
    软连接文件格式： lrwxrwxrwx. 1 root root     13 Jul 20 07:50 service -> /etc/services
    
    格式解析:
    
    1 1代表硬链接数量，如果该文件没有硬链接，就只有本身一个硬链接。
    
    2 13代表 链接文件的长度
    
    3 软连接的文件类型是 l（软连接），软连接文件的权限 都是  lrwxrwxrwx 
    
    4 ->  箭头指向到源文件
    
    真正的权限取决于对源文件的权限
    
    时间值为创建软连接的时间
    
    软连接可以跨文件系统生成
    ```

- 硬链接特性

    ```
    1 相当于 cp -p +同步更新  
    
    2 通过i节点识别，与源文件有相同的inode节点
    
    3 硬链接不能跨分区，[root@qianfeng01 ~]# ln /home/test/issuels /boot/test （错误）  
    
    4 不能针对目录使用  [root@qianfeng01 ~]# ln /tmp/ aa.hard   （无法将目录/tmp 生成硬链接）
    ```

- 删除软连接,源文件还在

    ```
    [root@qianfeng01 ~]# rm -rf symbolic_name 
    ```



#### 4. 查看命令

##### 4.1. cat

```
英文：concatenate 	命令路径：/bin/cat		执行权限：所有用户

作用：显示文件内容,直接显示全部内容

语法：cat [-n][文件名]

-A  显示所有内容，包括隐藏的字符   

-n	显示行号     

eg：[root@qianfeng01 ~]# cat /etc/services
```



##### 4.2. more

```
命令路径：/bin/more		执行权限：所有用户

作用：分页显示文件内容

语法：more [文件名]

空格或f   显示下一页

enter键   显示下一行

q或Q  退出

相关指令:less
```



##### 4.3. head

```
命令路径：/usr/bin/head		执行权限：所有用户

作用：查看文件前几行（默认10行）

语法：head [文件名]

-n 指定行数 

eg：[root@qianfeng01 ~]# 
head -20 /etc/services   
head –n 3 /etc/services
```



##### 4.4. tail

```
命令路径：/usr/bin/tail	执行权限：所有用户

作用：查看文件的后几行       

语法：tail [文件名] 

-n 指定行数 

获取一个大文件的部分文件，可使用head或tail命令

eg: 
[root@qianfeng01 ~]# head -n 100 /etc/services >config.log      会直接用 /etc/services里面前100行的数据导入conflg.log文件中,并覆盖其中的内容

[root@qianfeng01 ~]# head -n 100 /etc/services >>config.log      会直接用 /etc/services里面前100行的数据导入conflg.log文件中,并进行内容的追加
```



#### 5. 搜索查找命令

##### 5.1. find

```
命令路径：/bin/find		执行权限：所有用户

作用：查找文件或目录      

语法：find [搜索路径][匹配条件]

如果没有指定搜索路径，默认从当前目录查找
```

**find命令选项**

- -name : 按照名字查找

    ```
    说明: 按名字查找，精准查找
    
    eg: [root@qianfeng01 ~]# find  /etc  -name  “init” 在目录/etc中查找文件init
    ```

- -iname : 按照名字查找

    ```
    说明: 按名字查找，不区分大小写
    
    find查找字符匹配：
    
    *: 匹配所有
    
    ?: 匹配单个字符
    
    eg: [root@qianfeng01 ~]# find  /etc  -name  “init???”    在目录/etc中查找以init开头的，且后面有三位的文件
    
    模糊匹配的条件，建议使用单引号或双引号括起来。
    
    如果*被转义，可使用单双引号括住查询条件，或者使用\\*。
    
    eg: [root@qianfeng01 ~]# find   –name  "*g"
    ```

- -size : 按照大小查找

    ```
    以block为单位，一个block是512B, 1K=2block    +大于  -小于  不写是等于 
    
    eg: [root@qianfeng01 ~]# find /etc -size -204800  在etc目录下找出小于100MB的文件
    
    100MB=102400KB=204800block 
    ```

- -type : 按照类型查找

    ```
    f 二进制文件  l 软连接文件 d 目录  c 字符文件 
    
    eg:[root@qianfeng01 ~]# find /dev -type c
    
    find查找的基本原则：
    
    占用最少的系统资源，即查询范围最小，查询条件最精准
    
    注意: 如果明确知道查找的文件在哪一个目录，就直接对指定目录查找，不查找根目录/ 
    ```



##### 5.2. grep

```
命令路径：/bin/grep		执行权限：所有用户

作用：在文件中搜寻字符串匹配的行并输出      

语法：grep [-cinv] '搜寻字符串' filename

选项与参数：

-c ：输出匹配行的次数（是以行为单位，不是以出现次数为单位）

-i ：忽略大小写，所以大小写视为相同

-n ：显示匹配行及行号

-v ：反向选择，显示不包含匹配文本的所有行。

eg：[root@qianfeng01 ~]# grep ftp  /etc/services 

eg: [root@qianfeng01 ~]# grep -v ^# /etc/inittab         去掉文件行首的#号

eg: [root@qianfeng01 ~]# grep -n “init”/etc/inittab  显示在inittab文件中，init匹配行及行号

eg: [root@qianfeng01 ~]# grep -c“init”/etc/inittab   显示在inittab文件中，init匹配了多少次
```

```
管道命令: |
以前面命令的输出结果，作为第二个命令的输入
eg: [root@qianfeng01 ~]# ls | grep abc	ls表示列举当前路径下所有的文件，在结果中查找包含abc的内容
```



##### 5.3. which

```
命令路径：/usr/bin/which		执行权限：所有用户

作用：显示系统命令所在目录（绝对路径及别名）  

which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令

eg: [root@qianfeng01 ~]# which ls    出现下面的信息,是ls指令的详情,说明ls指令是存在的

alias ls='ls --color=auto'

/bin/ls

eg: [root@qianfeng01 ~]# which zs   出现下面的信息,说明zs指令是不存在的

/usr/bin/which: no zs in (/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/ch/bin)
```



##### 5.4. whereis

```
命令路径：/usr/bin/whereis		执行权限：所有用户

作用：搜索命令所在目录 配置文件所在目录  及帮助文档路径      

eg: [root@qianfeng01 ~]# which passwd    和   [root@qianfeng01 ~]#whereis  passwd   

eg: 查看/etc/passwd配置文件的帮助，就用  man 5 passwd
```



#### 6. 磁盘的操作命令

##### 6.1. df

```
作用：用于查看Linux文件系统的状态信息,显示各个分区的容量、已使用量、未使用量及挂载点等信息。看剩余空间

语法：df [-hkam][挂载点]

-h  （human-readable）根据磁盘空间和使用情况 以易读的方式显示 KB,MB,GB等

-k   以KB 为单位显示各分区的信息，默认

-m   以MB为单位显示信息

-a   显示所有分区包括大小为0 的分区
```



##### 6.2. du

```
作用：用于查看文件或目录的大小（磁盘使用空间）

语法：du [-ahs][文件名目录]

-a 显示子文件的大小

-h 以易读的方式显示 KB,MB,GB等

-s summarize 统计总占有量

eg: [root@qianfeng01 ~]# du -a(all) /home 　显示/home 目录下每个子文件的大小,默认单位为kb

eg: [root@qianfeng01 ~]# du -h /home 以K，M,G为单位显示/home 文件夹下各个子目录的大小

eg: [root@qianfeng01 ~]# du -sh /home 　以常用单位（K,M,G）为单位显示/home 目录的总大小 -s summarize

df命令和du命令的区别：

df命令是从文件系统考虑的，不仅考虑文件占用的空间，还要统计被命令或者程序占用的空间。

du命令面向文件，只计算文件或目录占用的空间。

eg: 执行下面的命令查看区别

[root@qianfeng01 ~]# df –h /

[root@qianfeng01 ~]# du –sh /
```



##### 6.3. free

```
作用：显示系统内存的使用情况，包括物理内存、交换内存(swap)和内核缓冲区内存。

语法： free [-kmg]

选项：

-k:  以KB为单位显示，默认就是以KB为单位显示

-m:  以MB为单位显示

-g:  以GB为单位显示

清理缓存命令：

eg: [root@qianfeng01 ~]# echo 1 > /proc/sys/vm/drop_caches
```



#### 7. 打包压缩命令

##### 7.1. gzip

```
英文：GNU zip 	命令路径：/bin/gzip		执行权限：所有用户

作用：压缩(解压)文件,压缩文件后缀为.gz    

gzip 只能压缩文件，不能压缩目录；不保留原文件

语法：gzip 文件
 
-d   将压缩文件解压（decompress）

解压使用gzip –d或者 gunzip
```



##### 7.2. bzip2

```
命令路径：/usr/bin/bzip2		执行权限：所有用户

作用：压缩(解压)文件,压缩文件后缀为.bz2

语法：bzip2 [-k][文件]   

-k: 产生压缩文件后保留原文件（压缩比高）

-d: 解压缩的参数（decompress）

解压使用bzip2 –d或者 bunzip2
```



##### 7.3. zip

```
命令路径：/usr/bin/zip		执行权限：所有用户

作用: 压缩(解压)文件,压缩文件后缀为.zip,保留源文件

语法: zip 选项 [压缩后文件名称] [文件或目录]   

-r 压缩目录

eg：

[root@qianfeng01 ~]# zip services.zip /etc/services  压缩文件

[root@qianfeng01 ~]# zip -r test.zip  /test  压缩目录

如果不加-r选项，压缩后的文件没有数据。

解压使用unzip ,注意如果解压时,已经存在与压缩文件同名的目录名,默认不会进行覆盖,如果选择Y,会进行覆盖
```



##### 7.4. tar

```
命令路径：/bin/tar	执行权限：所有用户

作用：文件、目录打（解）包

语法：tar [-zcf] 压缩后文件名  文件或目录

-c 建立一个压缩文件的参数指令（create），后缀是.tar

-x 解开一个压缩文件的参数指令（extract）

-z 以gzip命令压缩/解压缩  

-j  以bzip2命令压缩/解压缩 

-v 压缩的过程中显示文件（verbose）

-f file 指定文件名,必选项
```



##### 7.5. 示例

1. 简单理解

    ```
    [root@qianfeng01 ~]# tar –cf   tar –xf	单独的打包 ，解包  
    
    [root@qianfeng01 ~]# gzip   bzip2	打包之后，进行压缩  
    
    [root@qianfeng01 ~]# tar –zcvf       [root@qianfeng01 ~]#tar -zxvf	一步到位  
    
    [root@qianfeng01 ~]# tar  -z 以gzip打包目录并压缩  文件格式.tar.gz（.tgz）
    
    [root@qianfeng01 ~]# tar  -j 以bzip2打包目录并压缩  文件格式.tar.bz2
    ```

    

2. 示例

    ```
    eg: [root@qianfeng01 ~]# tar -zcvf dir1.tar.gz  dir1   使用gzip将目录dir1压缩成一个打包并压缩文件dir1.tar.gz
    
    eg: [root@qianfeng01 ~]# tar -zxvf  dir1.tar.gz -C(大写的)  /root     解压到指定的/root目录
    
    eg: [root@qianfeng01 ~]# tar -cvf bak.tar  .    将当前目录的文件打包为bak.tar
    
    eg: [root@qianfeng01 ~]# tar -xvPf bak.tar   解压到当前目录,默认是相对路径,P意思是支持绝对路径
    
    eg: [root@qianfeng01 ~]# tar  -cvPf  bak.tar  /a/b    将/a/b路径压缩成bak.tar
    
    eg: [root@qianfeng01 ~]# tar -zcvf bak.tar.gz  bak.tar   或 [root@qianfeng01 ~]#gzip  bak.tar    使用gzip将打包文件bak.tar压缩为bak.tar.gz
    
    eg: [root@qianfeng01 ~]# tar -jcvf bak.tar.bz2  bak.tar  或 [root@qianfeng01 ~]#bzip2 bak.tar   使用bzip2将打包文件bak.tar压缩为bak.tar.bz2
    
    eg: [root@qianfeng01 ~]# tar -rvf bak.tar /etc/password     将/etc/password追加文件到bak.tar中
    
    eg: [root@qianfeng01 ~]# tar -cjvf test.tar.bz2  test  生成test.tar.bz2的压缩文件
    
    eg: [root@qianfeng01 ~]# tar -xjf test.tar.bz2        解压
    ```

    

3. 最常用: tar + gzip

    ```
    [root@qianfeng01 ~]# tar –zcvf 压缩    
    
    [root@qianfeng01 ~]# tar –zxvf  解压
    
    补充：源文件是保留的，不会被删除
    ```



#### 8. date命令

```
1 date命令是显示或设置系统时间与日期。

2 查看系统时间直接使用	date

3 以自定义的方式显示系统时间:	date +'%Y-%m-%d %H:%M:%S'   

注意:+与字符串之间不能有空格,与date之间要有空格

4 设置时间

eg: [root@qianfeng01 ~]# date  -s  "2015-5-8 19:48:00"

5 同步到bios，重启之后才能继续生效

eg: [root@qianfeng01 ~]# hwclock -w
```



#### 9. 系统关机命令

##### 9.1. 关机命令：shutdown

```
语法：shutdown  [选项] 时间    
选项： 
-c: 取消前一个关机命令   
-h: 关机   
-r: 重启

eg: 
[root@qianfeng01 ~]# shutdown -h  now  立即关机     
[root@qianfeng01 ~]# shutdown -h  20:30  定时关机

其他关机命令：
halt     
poweroff     
init 0
```



##### 9.2. 重启命令：reboot

```
eg: reboot 重启系统    

[root@qianfeng01 ~]# reboot -h now    立即重启
```



##### 9.3. 重启命令：init 6

```
注意：生产环境中，关机命令和重启命令谨慎执行。
```



##### 9.4. 执行等级

```
linux有七种执行等级:
init 0：停机
init 1：单用户形式，只root进行维护
init 2：多用户，不能使用net file system
init 3：完全多用户
init 5：图形化
init 4：安全模式
init 6：重启 
```



##### 9.5. 其他命令

```
uname -r	查看系统版本信息    
[root@qianfeng01 ~]# basename     a/b/c/test.txt		显示:test.txt   查看命令中的名字     
[root@qianfeng01 ~]# dirname      a/b/c/test.txt	  显示: a/b/c/	查看命令中的路径     
```



#### 10. Linux的快捷键命令

```
善于查看man help等帮助文档 

利用好Tab键  自动补全

掌握好一些快捷键

ctrl + c（停止当前进程）

ctrl + z  挂起当前进程，放后台

ctrl + r（查看命令历史） history

ctrl + l（清屏，与clear命令作用相同）

对当前命令行的操作:

方向箭头  上 下 可以查看执行过的命令并再次使用

ctrl + a  行首 ctrl + e 行尾  ctrl+d  清除当前的字符

ctrl + k  清除光标后面的内容   ctrl+ w 清除光标前面的单词
```



#### 11. 网络相关操作

##### 11.1. 修改ip地址

说明: (以网络方式为NAT示例)

1. 使用命令行修改配置文件

    ```shell
    # 注: 这里编辑的文件为你的网卡的信息
    # 在Windows平台使用VMWare安装，默认使用的是ens33
    # 在Mac平台使用Parallels Desktop安装，默认使用的是eth0
    [root@qianfeng01 ~]# vi /etc/sysconfig/network-scripts/ifcfg-ens33
    
    BOOTPROTO=static    			# static => 静态ip  dhcp => 动态ip   none => 不设置,默认静态ip
    HWADDR=00:0C:29:3D:B0:9B  # 电脑的mac地址,建议去掉
    UUID=6f89eb5e-bff4-4f6b-a045-b7722bd188fc     # 电脑的UID，建议去掉
    ONBOOT=yes								# 是否开机自动使用
    nm_controlled=yes   			# 设置network manager的参数,实时生效,修改后无需要重启网卡立即生效。建议删掉
    IPADDR=192.168.10.101
    NETMASK=255.255.255.0   	# 或者   PREFIX=24
    GATEWAY=192.168.10.2      # 网段2任意，IP地址2固定，网段为vmnet8的设置的IP网段（Mac平台安装的需要将其设置为192.168.10.1）
    DNS1=114.114.114.114   		# 国内的DNS
    DNS2=8.8.8.8  						# 谷歌的DNS
    defroute=yes  						# 默认的路由   建议删掉
    IPV4_FAILURE_FATAL=yes 		# 如果为yes，则ipv4配置失败禁用设备   建议删掉
    ```

    

2. 重启网络生效

    ```properties
    CentOS 6: service network restart
    CentOS 7: systemctl restart network
    ```



##### 11.2. 关闭防火墙

1. 立即关闭防火墙，重启后生效

    ```shell
    [root@qianfeng01 ~]# systemctl start firewalld		[开启防火墙]
    [root@qianfeng01 ~]# systemctl stop firewalld	    [关闭防火墙]
    [root@qianfeng01 ~]# systemctl restart firewalld  [重启防火墙]
    [root@qianfeng01 ~]# systemctl status firewalld   [查看防火墙运行状态]
    ```

    

2. 关闭防火墙开机自启动

    ```shell
    [root@qianfeng01 ~]# systemctl enable firewalld	  [打开开机自启动]
    [root@qianfeng01 ~]# systemctl disable firewalld  [关闭开机自启动]
    ```



##### 11.3. 关闭NetworkManager

NetworkManager服务是管理和监控网络设置的守护进程，CentOS7更加注重使用NetworkManager服务来实现网络的配置和管理。它是一个动态的，事件驱动的网络管理服务。

```shell
常用管理命令：
[root@qianfeng01 ~]# systemctl  status  NetworkManager
[root@qianfeng01 ~]# systemctl  start   NetworkManager
[root@qianfeng01 ~]# systemctl  stop    NetworkManager
[root@qianfeng01 ~]# systemctl  disable NetworkManager
[root@qianfeng01 ~]# systemctl  enable  NetworkManager
```



##### 11.4. 网络通信命令

###### 11.4.1. ping

```
命令路径：/bin/ping		执行权限：所有用户
作用：测试网络的连通性
语法：ping 选项 IP地址 
-c 指定发送次数    
ping 命令使用的是icmp协议，不占用端口
eg: [root@qianfeng01 ~]#ping -c 3 127.0.0.1
```



###### 11.4.2. ip addr

```
英文：interface configure 命令路径：/sbin/ifconfig		执行权限：root

作用：查看和设置网卡网络配置

语法：ifconfig [-a][网卡设备标识]   

-a：  显示所有网卡信息

ifconfig  网卡名字    查看单个的网卡信息
```



###### 11.4.3. netstat

```
英文：network statistics		命令路径：/bin/netstat		执行权限：所有用户

注意：如果在系统中默认没有这个命令：直接安装net-tools包即可

作用：主要用于检测主机的网络配置和状况

-a (all)显示所有连接和监听端口

-t  (tcp)仅显示tcp相关选项

-u (udp)仅显示udp相关选项

-n 使用数字方式显示地址和端口号

-p 显示socket的PID和进程的名字

-l  (listening)显示监控中的服务器的socket

eg: [root@qianfeng01 ~]# netstat -tlnu      查看本机监听的端口

eg: [root@qianfeng01 ~]# tcp 0 0 0.0.0.0:111 0.0.0.0:* LISTEN

协议  待收数据包  待发送数据包  本地ip地址：端口 远程IP地址：端口

netstat –antpl  

eg: [root@qianfeng01 ~]# netstat -ntlp | grep httpd  查看某一个程序的端口信息

eg: [root@qianfeng01 ~]# netstat -atnp | grep httpd  

eg: [root@qianfeng01 ~]# netstat -atnp | grep 80   查看端口
```



##### 11.5. 修改主机名

- 临时生效: `hostname qianfeng01`
- 永久生效: `hostnamectl set-hostname qianfeng01`



##### 11.6. 修改映射关系

```
修改主机名和ip地址之间的映射关系

执行 [root@qianfeng01 ~]# vim /etc/hosts   进入hosts文件,加入代码  

主机ip   			主机名

192.168.10.111      qianfeng01

192.168.10.112      qianfeng02

192.168.10.113      qianfeng03

最好让主机名字,虚拟机名字,网络连接名字保持一致
```



#### 12. 进程管理命令

##### 12.1. 进程和程序的区别

```
1 程序是静态概念，本身作为一种软件资源长期保存；而进程是程序的执行过程，它是动态概念，有一定的生命期，是动态产生和消亡的。

2 程序和进程无一一对应关系。一个进程在活动中可有顺序地执行若干个程序。
```

##### 12.2. 父进程与子进程

```
1 子进程是由一个进程所产生的进程，产生这个子进程的进程称为父进程。

2 在Linux系统中，使用系统调用fork创建进程。fork复制的内容包括父进程的数据和堆栈段以及父进程的进程环境。

3 父进程终止子进程自然终止。
```

##### 12.3. 进程和线程的区别

```
进程： 就是正在执行的程序或命令，每一个进程都是一个运行的实体，都有自己的地址空间，并占用一定的系统资源。

线程： 轻量级的进程；进程有独立的地址空间，线程没有；线程不能独立存在，它由进程创建；相对讲，线程耗费的cpu和内存要小于进程。

进程管理的作用:判断服务器的健康状态;查看系统所有的进程;杀死进程.
```

##### 12.4. 前台进程和后台进程

```
前台进程：

在Shell提示处打入命令后，创建一个子进程，运行命令，Shell等待命令退出，然后返回到对用户给出提示符。这条命令与Shell异步运行，即在前台运行，用户在它完成之前不能执行另一个命令。 

后台进程：

在Shell提示处打入命令，若后随一个&，Shell创建的子进程运行此命令，但不等待命令退出，而直接返回到对

用户给出提示。这条命令与Shell同步运行，即在后台运行。后台进程必须是非交互式的。
```

##### 12.5. ps

```
作用：查看系统中的进程信息

语法：ps [-auxle]

常用选项

a：显示所有用户的进程

u：显示用户名和启动时间

x：显示没有控制终端的进程

e：显示所有进程，包括没有控制终端的进程

l：长格式显示
```



##### 12.6. 查看系统中所有进程

```
ps aux     #查看系统中所有进程，使用BSD操作系统格式，unix

ps -le     #查看系统中所有进程，使用Linux标准命令格式

eg: [root@qianfeng01 ~]# ps -u or ps -l  查看隶属于自己进程详细信息

eg: [root@qianfeng01 ~]# ps aux | grep sam    查看用户sam执行的进程 

eg: [root@qianfeng01 ~]# ps -ef | grep init        查看指定进程信息
```



##### 12.7. pstree

```
作用：查看当前进程树

语法：pstree [选项]

-p  显示进程PID

-u  显示进程的所属用户
```



##### 12.8. top

```
作用：查看系统健康状态  

显示当前系统中耗费资源最多的进程,以及系统的一些负载情况。

语法：top [选项]

-d 秒数，指定几秒刷新一次，默认3秒（动态显示）
```



##### 12.9. kill

```
作用：关闭进程

语法：kill [-选项] pId

eg: [root@qianfeng01 ~]# kill -9 进程号（强行关闭）  常用
	
eg: [root@qianfeng01 ~]# kill -1 进程号（重启进程）

eg: [root@qianfeng01 ~]# killall -l  关闭所有进程(忽略进程名的大小写)
```



##### 12.10. w

```
作用: 查看用户信息

语法: w 用户名

eg: [root@qianfeng01 ~]# w root 

显示:

USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT

root     pts/0    10.0.158.3       06:02    0.00s  0.13s  0.00s w root

解释:

JCPU：以终端代号来区分，该终端所有相关的进程执行时，所消耗的CPU时间会显示在这里

PCPU：CPU执行程序耗费的时间

WHAT：用户正在执行的操作
```



##### 12.11. nohup

```
作用: 使进程在用户退出登陆后仍旧继续执行,nohup命令将执行后的数据信息和错误信息默认储存到文件nohup.out中

语法: nohup program &      &在后台运行的意思

eg: [root@qianfeng01 ~]# nohup ping  www.baidu.com  &

使用: [root@qianfeng01 ~]# ps -ef  | grep  ping 查看当前的ping进程
```



##### 12.12. uptime

```
作用: 查看负载

例如: 使用uptime确定是服务器还是网络出了问题。如果网络应用程序运行，运行uptime来了解系统负载是否很高。如果负载不高，这个问题很有可能是由于网络引起的而非服务器。

语法: uptime

eg: 执行: [root@qianfeng01 ~]# uptime

显示: 06:32:04 up 19:57,  4 users,  load average: 0.00, 0.01, 0.00

解释: 系统时间		用户数量     服务器在过去的1分钟、5分钟、15分钟的系统平均负载值
```



#### 13. 用户管理命令

##### 13.1. su

```
语法:  su  用户名

作用:  切换用户

eg: [root@qianfeng01 ~]# su  root    切换回root用户,注意:如果是root用户,每次都会要求输入密码.普通的用户可以直接切换
```



##### 13.2. useradd

```
添加用户

语法：useradd [选项] 用户名
```



##### 13.3. passwd

```
修改密码命令

语法：passwd [选项][用户名]

用户密码：生产环境中，用户密码长度8位以上，设置大小写加数字加特殊字符，要定期更换密码。

ys^h_L9t  
```



##### 13.4. userdel

```
删除用户

-r 删除账号时同时删除宿主目录（remove）

注意：一个用户已经打开，需要删除

做法：先跳到当前用户下面，执行exit  或 exit 用户名。这时会自动跳回root界面。

如果还是无法删除，可以将系统重启
```



##### 13.5. groupadd

```
用于添加组

-g 指定gid
```



##### 13.6. groupmod

```
用于修改组

-n 更改组名（new group）

[root@qianfeng01 ~]# groupmod  -n new_gname old_gname
```



##### 13.7. groupdel

```
用于删除组

如果要删除的组归属于某一个用户的所属组，则不能删除该组

[root@qianfeng01 ~]# useradd -g hadoop hdfs

[root@qianfeng01 ~]# groupdel hadoop
```



#### 14. 文件权限命令

##### 14.1. 三种基本权限

```
r 读权限（read）

w 写权限（write）

x 执行权限 （execute）
```



##### 14.2. 权限说明

![image-20210326152735995](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210326152735995.png)

```
所有者  所属组  其他人

第1位：文件类型（d 目录，- 普通文件，l 链接文件）

第2-4位：所属用户(所有者)权限，用u（user）表示

第5-7位：所属组权限，用g（group）表示

第8-10位：其他用户（其他人）权限，用o（other）表示

第2-10位：表示所有的权限，用a（all）表示

eg: -rw-r--r--. 1 root root 3664 Nov 30 17:42 CentOS-Vault.repo.bak

第十一位的1代表硬链接数
```

| 字符 | 权限     | 对文件的含义     | 对目录的含义                         |
| ---- | -------- | ---------------- | ------------------------------------ |
| r    | 读权限   | 可以查看文件内容 | 可以列出目录的内容（ls）             |
| w    | 写权限   | 可以修改文件内容 | 可以在目录中创建删除文件( mkdir,rm ) |
| x    | 执行权限 | 可以执行文件     | 可以进入目录(cd)                     |

```
对于文件,我们有执行权限的命令：

r-cat,more,head,tail,less

w-echo,vi

x-命令，脚本

对于目录,我们有执行权限的命令：

r-ls

w-touch,mkdir,rm,rmdir

x-cd 

能删除文件的权限是必须对该文件所在的目录有wx权限。
```



##### 14.3. chmod

```
用于权限更改

英文：change mode (change the permissions mode of a file)

作用：改变文件或目录权限

语法：

chmod [{ugoa}{+-=}{rwx}][文件名或目录]  

chmod [mode=421][ 文件或目录]

参数：-R 下面的文件和子目录做相同权限操作（Recursive递归的）

可以有修改一个文件的权限:

1.root

2.文件所有者

eg: [root@qianfeng01 ~]# chmod  u+x  a.txt

eg: [root@qianfeng01 ~]# chmod u+x,o-x a.txt

用数字来表示权限（r=4，w=2，x=1，-=0）

eg: [root@qianfeng01 ~]# chmod  750  b.txt

rwx和数字表示方式能随意切换

注意：root用户是超级用户，不管有没有权限，root都能进行更改。用普通用户测试权限。

不能用一个普通用户去修改另一个普通用户的权限。
```



##### 14.4. chown

```
用于更改所有者

英文：change file ownership

作用：更改文件或者目录的所有者 

语法 : chown user[:group] file... 

-R : 递归修改

参数格式 : user : 新的档案拥有者的使用者 ID

group : 新的档案拥有者的使用者群体(group) 

eg：[root@qianfeng01 ~]# chown lee file1   把file1文件的所有者改为用户lee

eg：[root@qianfeng01 ~]# chown lee:test file1   把file1文件的所有者改为用户lee,所属组改为test

eg：[root@qianfeng01 ~]# chown –R  lee:test dir   修改dir及其子目录的所有者和所属组
```



##### 14.5. chgrp

```
用于改变所属组

英文：change file group ownership

作用：改变文件或目录的所属组

语法 : chgrp [group] file... 

eg: [root@qianfeng01 ~]# chgrp root test.log    把test.log的所属组修改为root
```



#### 15. sudo权限的配置

##### 15.1. 作用

```
root把本来只能超级用户执行的命令赋予普通用户执行。

sudo的操作对象是系统命令
```



##### 15.2. 修改sudoers文件

```
执行 [root@qianfeng01 ~]# visudo       这里实际修改的是/etc/sudoers文件

sudoers文件内部的信息解释

root        ALL=（ALL）    ALL         
#用户名   被管理主机的地址（不是访问地址）=（可使用的身份）    授权命令（绝对路径）

%wheel   ALL=（ALL）    ALL	 
#%组名    被管理主机的地址=（可使用的身份）    授权命令（绝对路径）

[root@qianfeng01 ~]# sudo -l    查看可用的sudo命令

注意:
sudo命令用来以其他身份来执行命令，预设的身份为root。在/etc/sudoers中设置了可执行sudo指令的用户。
若其未经授权的用户企图使用sudo，则会发出警告的邮件给管理员。
用户使用sudo时，必须先输入密码，之后有5分钟的有效期限，超过期限则必须重新输入密码。
```



##### 15.3. 语法

```
sudo(选项)(参数)
选项 

-b：在后台执行指令；

-h：显示帮助； 

-H：将HOME环境变量设为新身份的HOME环境变量； 

-k：结束密码的有效期限，也就是下次再执行sudo时便需要输入密码；

-l：列出目前用户可执行与无法执行的指令； 

-p：改变询问密码的提示符号； 

-s：执行指定的shell； 

-u<用户>：以指定的用户作为新的身份。若不加上此参数，则预设以root作为新的身份； 

-v：延长密码有效期限5分钟； 

-V ：显示版本信息。
```



##### 15.4. 实例

###### 15.4.1. 说明信息

```
1 配置sudo必须通过编辑/etc/sudoers文件，而且只有超级用户才可以修改它，还必须使用visudo编辑。

2 之所以使用visudo有两个原因

一是它能够防止两个用户同时修改它

二是它也能进行有限的语法检查

3 所以，即使只有你一个超级用户，你也最好用visudo来检查一下语法。 visudo默认的是在vi里打开配置文件，用vi来修改文件。我们可以在编译时修改这个默认项。

4 visudo不会擅自保存带有语法错误的配置文件，它会提示你出现的问题，并询问该如何处理，错误提示：sudoers file:syntax error,line 22<< 

此时我们有三种选择：

键入“e”是重新编辑

键入“x”是不保存退出

键入“Q”是退出并保存。如果真选择Q，那么sudo将不会再运行，直到错误被纠正。 
```



###### 15.4.2. 实例1

```
让用户hd1可以通过sudo执行所有root可执行的命令。以root身份用visudo打开配置文件，可以看到类似下面几行：
ALL=(ALL)ALL
这里root有所有权限，只要仿照现有root的例子就行，我们在下面加一行（最好用tab作为空白）
hd1 ALL=(ALL)	ALL  
```

```
第一个ALL是指网络中的主机,它指明hd1可以在此主机上执行后面的命令。
第二个括号里的ALL是指目标用户，也就是以谁的身份去执行命令,通常我们指定为all即可。
最后一个ALL当然就是指命令名了。 
```

```
保存退出后，切换到hd1用户，我们用它的身份执行命令：
[root@qianfeng01 ~]# sudo ls /root   在ls前面添加sudo可以让hd1拥有root的权限,查看root目录
```



###### 15.4.3. 实例2

```
我们限制一下hd1的权利。比如我们只想让他像root那样使用ls和ifconfig，继续修改sudoers文件
db1 ALL=(ALL)	/sbin/ifconfig,	/bin/ls,/usr/bin/sudo

再次切换到hd1用户,执行命令：
[root@qianfeng01 ~]# sudo head -5 /etc/shadow  这是提示hd1没有权限执行这个命令.
```



###### 15.4.4. 免密设置

```
我们这样再添加一行配置文件
hdp1 ALL=NOPASSWD: /bin/cat/ls, /usr/bin/sudo
再来sudo一下：不再需要密码
```



### 第四章 软件管理【重要】

#### 1. 软件安装介绍

学软件开发，各种台的软件熟练安装是必须要熟练掌握。大家都知道，Windows下安装软件时，只需用鼠标双击软件的安装程序，或者用Zip等解压缩软件解压缩即可安装；在android或者apple中安装软件时，只需要在手机应用商店点击安装即可。而在Linux下安装软件难度高于Windows、Android、ios和windows phone下软件安装。下面我就详细讲解Linux下如何安装软件。

Linux下软件的安装大概可以分为如下几种：

```
1 二进制程序的安装(最简单安装)

2 rpm程序安装(后缀名为*.rpm) 

3 yum源安装(本质也是rpm安装，它只是rpm的一种安装方式而已)

4 源码安装
```



#### 2. 二进制安装

Linux下二进制格式的软件是指事先已经在各种平台编译安装好相关软件，然后压缩打包，在安装时只需解压或者执行安装可执行文件即可。这种软件发行格式类似与windows系统，这样做的原因是保护源代码程序不对外泄露，保护知识产权。二进制软件包的优点是安装简单、容易,缺点是缺乏灵活性，相应的软件包执行在对应平台下安装，离开这个环境软件就无法运行。

二进制软件包提供了很多类型的打包方式，最常见的就是我们RPM格式的包，还有以“\*.tar.gz、\*.tgz、\*.bz2“等形式的二进制软件包，最后还有一个就是提供安装程序进行安装的二进制软件包。下面分别介绍：



##### 2.1. 安装\*.tar.gz、*.bz2二进制软件包

这种格式的软件包，安装其实就是简单的解压过程，根据不同的软件打包格式，我们用相应的解压命令解压即可。

对于\*.tar.gz软件格式解压：tar –zxvf  xxxxxx.tar.gz 对于*.bz2软件格式解压：  tar  -jxvf xxxxxx.tar.gz这类软件的卸载，对于解压后只有单一目录的软件，可以直接删除对应软件目录即可，如果解压后文件分散在几个目录中，需要一一手动删除目录。

例如：我们常用的应用服务器容器tomcat软件就是基于这种打包压缩格式发行的。我们只需下载后解压即可完成安装，如果需要卸载，直接删除对应的tomcat目录即可。



##### 2.2. 提供安装程序的软件包

这种软件包都提供了安装脚本或者安装向导程序，只需在下载此类软件包后解压，然后进入安装目录，找到类似的setup、install、install.sh之类的可执行文件运行即可，然后根据提示（比如安装路径，参数设置等等）进行相应的设置，接着安装就自动完成。

这类软件的卸载也提供了相应的卸载脚本或者卸载向导，根据提示即可完成软件卸载。

例如：经常使用的sun jdk就是这种安装方式，从网络下载下来的jdk是个二进制可执行文件，首先设置文件的可执行权限（例如chmod 755  jdk1.6.0_07.bin，权限操作本书后面会有详细讲述） ，然后运行软件（也即是./ jdk1.6.0_07.bin）即可完成安装。安装完成会在当前目录产生一个对应的jdk程序目录。



#### 3. rpm安装

RPM软件包管理

RPM是RedHat Package Manager（RedHat软件包管理工具）的缩写，这一文件格式名称虽然打上了RedHat的标志，但是其原始设计理念是开放式的，现在包括RedHat、CentOS、SUSE等Linux的发行版本都有采用，可以算是公认的行业标准了。RPM文件在Linux系统中的安装最为简便

RPM命令使用

rpm的常用参数

```
i：安装应用程序（install）

vh：显示安装进度（verbose hash） 

U：升级软件包（update） 

qa: 显示所有已安装软件包（query all）

e：卸载应用程序（erase）

注意：如果其它软件包有依赖关系，卸载时会产生提示信息，可使用--nodeps强行卸载。

查询所有安装的rpm包: [root@qianfeng01 ~]# rpm –qa

查询mysql相关的包： [root@qianfeng01 ~]# rpm –qa | grep mysql

安装：[root@qianfeng01 ~]# rpm  -ivh  jdk.rpm

卸载： [root@qianfeng01 ~]# rpm –e mysql*

强行卸载：[root@qianfeng01 ~]# rpm –e mysql*  --nodeps
```



#### 4. 软件安装之yum【重点】

##### 4.1. yum的简介

```
yum，是Yellowdog Updater Modified的缩写，是Linux一种软件包管理工具。

    在之前的课程中，我们学习过了RPM的安装。RPM的安装，其实是Linux中的标准的安装方式。但是RPM有一个非常麻烦的地方，就是需要手动管理安装包；另外，RPM是不能自动的解决依赖缺失的。在安装的软件的时候，如果这个程序需要依赖其他的程序，则需要我们手动安装，参考上一篇的练习: MySQL的安装。每一个依赖的程序都需要我们手动安装。
yum则完美的解决了这样的问题。使用yum安装，不需要关心这个软件包依赖什么程序，yum会自动的帮你安装依赖的组件。

    yum的本质其实还是RPM的安装，yum只是对其进行了自动的管理，不需要我们再手动的管理依赖。
```



##### 4.2. 阿里云镜像替换

```
yum是一个在线安装软件的管理工具，因此需要通过网络下载软件。
yum自带的镜像是一个国外的地址，速度会比较慢。因此在使用的过程中，很多时候我们是需要将其替换为国内的源的。

常见的国内的源:
http://mirrors.163.com							# 网易的源
http://mirrors.sohu.com							# 搜狐的源
http://mirrors.tuan.tsinghua.edu.cn	  			# 清华大学源
http://mirrors.ustc.edu.cn						# 中科大源
https://mirrors.aliyun.com						# 阿里巴巴的源
```

**替换阿里源**

```shell
# 1. 切换到源文件的路径下
#    在这个路径下，有若干个repo文件，里面记录的就是源信息
#    网络镜像源，在CentOS-Base.repo中
[root@qianfeng01 ~]# cd /etc/yum.repos.d

# 2. 将原来的源备份
[root@qianfeng01 yum.repos.d]# mv CentOS-Base.repo CentOS-Base.repo.bak

# 3. 下载阿里云的源配置信息到本地
[root@qianfeng01 yum.repos.d]# curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo

# 4. 重新构建YUM的缓存
[root@qianfeng01 yum.repos.d]# yum clean all
[root@qianfeng01 yum.repos.d]# yum makecache
```



##### 4.3. yum的基本操作

**查询**

```shell
yum list     				  # 查询所有可用软件包列表

yum search 关键字     			# 搜索服务器上所有和关键字相关的包

yum info 关键字 				# 来查找包名
```

**安装**

```shell
yum -y install 包名     # -y  自动回答yes  
```

**升级**

```shell
yum -y update 包名 

# 注意：如果不加包名，就升级所有的，包括内核。必须加包名升级单个软件包，慎用升级所有的
# 检测升级 yum check-update
```

**卸载**

```
yum -y remove 包名
```

**其他操作**

```shell
yum --help、man yum  				 # 帮助

yum clean all          	   		# 清除缓存和旧的包

yum repolist                	# 查看当前可用的yum源

yum deplist httpd            	# 列出一个包所有依赖的包
```



##### 4.4. 搭建本地yum源

1. 挂载光盘

    1. 自己创建`/mnt/cdrom`文件夹，为了挂载光盘的内容

        ```shell
        [root@qianfeng01 ~]# mkdir /mnt/cdrom
        ```

        

    2. 挂载相关命令

        <table>
          <tr>
            <th colspan="5">挂/卸载</th>
          </tr>
          <tr>
        		<td colspan="5">
              命令: mount / unmount<br />
              格式: mount [参数] 存储设备 挂载点<br />
              说明: 存储设备对应分区的设备文件(如"/dev/sdb1")或网络资源路径。<br />
                   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;挂载点为用户指定用于挂载的目录。挂载点必须是一个已经存在的目录。
            </td>
          </tr>
          <tr>
          	<th>参数</th>
            <th>说明</th>
            <th> </th>
          </tr>
          <tr>
          	<td rowspan="4">-t 文件系统类型</td>
            <td rowspan="4">指定文件系统的类型。<br />通常不必指定，mount会自动选择正确的类型。</td>
            <td rowspan="4">常<br />用<br />类<br />型</td>
            <td>光盘或光盘镜像</td>
            <td>iso9660</td>
          </tr>
          <tr>
            <td>fat16文件系统</td>
            <td>msdos</td>
          </tr>
          <tr>
            <td>fat32文件系统</td>
            <td>vfat</td>
          </tr>
          <tr>
            <td>ntfs文件系统</td>
            <td>ntfs</td>
          </tr>
          <tr>
          	<td rowspan="4">-o options</td>
            <td rowspan="4">主要用来描述设备或档案的挂载方式。</td>
            <td rowspan="4">常<br />用<br />参<br />数</td>
            <td>采用只读方式挂载设备</td>
            <td>ro</td>
          </tr>
          <tr>
            <td>用来把一个文件当成硬盘分区挂载上系统</td>
            <td>loop</td>
          </tr>
          <tr>
            <td>采用读写方式挂载设备</td>
            <td>rw</td>
          </tr>
          <tr>
            <td>指定访问文件系统所用字符集</td>
            <td>locharset</td>
          </tr>
          <tr>
          	<td>-f</td>
            <td colspan="4">用于除错。它会使mount并不执行实际挂载的动作，而是模拟整个挂载的过程。</td>
          </tr>
          <tr>
          	<td>-v</td>
            <td colspan="4">显示较详细信息，通常会和-f一起使用。</td>
          </tr>
          <tr>
          	<td>-f</td>
            <td colspan="4">使每一个mount的动作产生一个线程负责执行。挂载大量NFS档案系统时可以加快挂载的动作。</td>
          </tr>
          <tr>
          	<td>-a</td>
            <td colspan="4">将/etc/fstab中定义的所有档案系统挂载。通常和-f一起使用</td>
          </tr>
        </table>
        
        
    3. 命令解析
    
        ```
        ##命令格式：mount [-t vfstype] [-o options] device dir 
        其中： 
        #1.-t vfstype 指定文件系统的类型，通常不必指定。mount 会自动选择正确的类型。常用类型有： 
        光盘或光盘镜像：iso9660 
        DOS fat16文件系统：msdos 
        Windows 9x fat32文件系统：vfat 
        Windows NT ntfs文件系统：ntfs 
        Mount Windows文件网络共享：smbfs 
        UNIX(LINUX) 文件网络共享：nfs 
        #2.-o options 主要用来描述设备或档案的挂接方式。常用的参数有： 
        loop：用来把一个文件当成硬盘分区挂接上系统 
        ro：采用只读方式挂接设备 
        rw：采用读写方式挂接设备 
        iocharset：指定访问文件系统所用字符集 
        #3.device 要挂接(mount)的设备。 
        #4.dir设备在系统上的挂接点(mount point)。
        ##将镜像文件挂载在/mnt/cdrom下
        [root@qianfeng01 ~]# mkdir /mnt/cdrom  #在根目录下的mnt目录下创建目录cdrom
        [root@qianfeng01 ~]# mount -t iso9660 -o loop /dev/cdrom /mnt/cdrom	##挂载
        [root@qianfeng01 ~]# umount /mnt/dvd   							##卸载
        ```
    
        
    
2. 让网络源失效

    ```shell
    [root@qianfeng01 yum.repos.d]# cd /etc/yum.repos.d/
    
    [root@qianfeng01 yum.repos.d]# rename  .repo  .repo.bak  *        #重命名所有的.repo文件
    
    [root@qianfeng01 yum.repos.d]# cp  CentOS-Media.repo.bak  CentOS-Media.repo     #配置一个.repo文件
    ```

    

3. 对CentOS-Base.repo的解释

    ```shell
    [root@qianfeng01 yum.repos.d]# cat /etc/yum.repos.d/CentOS-Base.repo
    
    [base]
    
    name=CentOS-$releasever - Base
    
    mirrorlist=http://mirrorlist.centos.org/?release=releasever&arch=basearch&repo=os
    
    baseurl=http://mirror.centos.org/centos/releasever/os/basearch/
    
    gpgcheck=1
    
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    ```

    **解释:**

    ```
    [base]	     容器名称，一定要放在[]中
    
    name         容器说明，这个可以自己随便写
    
    mirrorlist   镜像站点，这个可以注释掉
    
    baseurl      yum源服务器的地址。默认是CentOS官方的yum源服务器
    
    enabled      此容器是否生效，如果不写或写enable=1都是生效，写成enable=0就是不生效
    
    gpgcheck      如果是1是指RPM的数字证书生效，如果是0则不生效
    
    pggkey        数字证书的公匙文件保存位置，不用修改
    ```

    

4. 修改光盘yum源文件

    ```shell
    [root@qianfeng01 yum.repos.d]# vi CentOS-Media.repo 
    
    [c6-media]
    name=CentOS-$releasever - Media
    baseurl=file:///mnt/cdrom        
    #这里的地址为自己光盘挂载地址，并把不存在的地址注释掉，在行首注释
    # file:///media/cdrom/
    # file:///media/cdrecorder/
    gpgcheck=1
    enabled=1        #把原来的0改为1，让这个yum源配资文件生效
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    ```

    可使用 yum repolist 查看当前可用yum源

    ```
    yum clean all
    
    yum repolist
    ```

    注意点:

    ```
    1 如果一旦重启服务器,需要重新挂载
    
    2 需要检查当前的虚拟机的镜像文件挂载处,将已经连接前面打勾
    
    3 如果镜像文件有两个DVD,我们可以这样做
    
    1)再添加一个光驱,将DVD2连接进来
    
    2)将服务器重启,发现在/dev下有了两个镜像文件(cdrom和cdrom1)
    
    3)在/mnt下再去创建一个文件夹(cdrom1)
    
    4)分别进行挂载操作
    [root@qianfeng01 ~]# mount -t iso9660 -o loop /dev/cdrom /mnt/cdrom
    
    [root@qianfeng01 ~]# mount -t iso9660 -o loop /dev/cdrom1 /mnt/cdrom1
    
    5)到/etc/yum.repos.d里面,同时添加路径
    
    file:///mnt/cdrom
    
    file:///mnt/cdrom1
    ```



##### 4.5. 搭建局域网yum源

1. 选择局域网内的一台机器作为YUM源服务器，安装HTTP服务进程

    ```shell
    [root@qianfeng01 ~]# yum list | grep http
    [root@qianfeng01 ~]# yum -y install httpd.x86_64
    ```

    

2. 开启HTTP服务进程并设置开机自启动

    ```shell
    [root@qianfeng01 ~]# service httpd start
    [root@qianfeng01 ~]# chkconfig httpd on
    ```

    

3. 配置HTTP服务管理YUM源目录

    ```shell
    # HTTP服务进程管理的目录是/var/www/html
    
    # 在管理目录下，创建软连接，指向YUM源目录
    [root@qianfeng01 ~]# ln -s /mnt/dvd /var/www/html/repo        <===都是用绝对路径，保没错
    ```

    

4. 其他机器只需要把本地YUM源的路径指向局域网YUM源地址即可

    ```shell
    [root@qianfeng01 yum.repos.d]# vi local.repo        		# 编辑文件，后缀必须是.repo
    
    [local]																									<=== YUM源仓库名
    name=localrepo																					<=== 昵称
    baseurl=http://局域网IP/repo															<=== 局域网YUM仓库位置
    enabled=1																								<=== 1 表示启用，0表示禁用
    gpgcheck=1																							<=== 1 表示 校验
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6			<=== 校验钥匙
    ```





#### 5. 软件安装之源码安装【了解】

##### 5.1. 源码编译安装

```
由于linux操作系统开放源代码，因而在其上安装的软件大部分也都是开源软件，例如apache、tomcat、php等软件。开源软件基本都提供源码下载，源码安装的方式；
```

**源码编译安装的优点**

```
1 用户可以自己定制软件功能，安装需要的模块，不需要的功能可以不用安装。

2 用户还可以自己选择安装路径，方便管理。

3 卸载软件也很方便，只需删除对应的安装目录即可。

4 能最大程度和服务器平台融合，效率稍微比其他方式高。

5 没有windows所谓的注册表之说。
```

**源码编译安装的缺点**

```
1 安装较为繁琐，需要自己配置

2 安装较为耗时，需要自己编译源码

3 安装较为容易出错，出错也难以解决
```



##### 5.2. 安装步骤

```
源码安装软件一般有以下几个步骤组成：下载解压源码、分析安装平台环境（ifconfigure）、编译安装软件（make,make install）。下面我们分别介绍。
```

1. 下载解压源码

    ```
    linux下软件的源码一般都是C或者C++语言编写的，并且都会在软件的官方网站上提供源码包下载，例如apache开源项目的官方网站为http://www.apache.org，此外在网上的很多开源社区一般也提供一些常用软件的源码下载，源码都打包成压缩文件，常见的源码打包压缩格式有“.tar.gz”、“.tar.baz2”等。我们可以从网站上首先下载源码文件，然后在传到所在的linux系统下，如果你的linux系统处于联网状态下，也可以直接在系统内通过wget之类的下载命令将源码包直接下载到linux系统下。
    
    下载完成，把相应的软件解包即可，针对下载软件包的不同，使用不同的方法进行解压，解压完毕后进入解压生成的目录中，在这个目录下，一般都存在一个REDAME文件，这个文本文件非常重要，它详细的介绍了这个软件所能完成的功能、授权许可、安装需求、安装注意事项、安装方式等等，由于linux各个版本的差异，以及安装环境的不同，软件的安装方式也不尽相同，所以在安装软件前，一定要事先阅读这个README文件，确保安装的正确性。
    ```

    

2. 分析安装平台环境

    ```
    在软件包解压完毕之后，我们进入源码目录，一般都会存在configure和README这么两个文件，README就是上面我们讲到的，是对软件的介绍和安装说明；linux下软件的安装受到操作系统安装环境的影响，比如某些软件在安装或者运行过程中需要调用操作系统本身的库文件，或者需要运行系统的某个工具等等，但是系统中不存在这个库文件或者这个工具没有安装，那么安装或者运行软件就会失败。
    
    为了避免这个问题的出现，在安装软件的时候就需要分析操作系统环境，检测当前系统是否拥有安装软件所需的所有文件和工具，如果系统缺少某个文件，就给出提示，直到满足软件的所有需求为止。这个就是configure文件的功能，configure文件一般是个可执行文件，可以在当前目录下直接输入“./configure”进行软件安装的环境测试，如果提示缺少某些安装包，就需要进行安装，直到测试通过。通常的，源码安装都需要GCC或者CC编译器，这些编译器一般在安装系统时定制安装包中的开发工具选项下，其实这也是我们在第二章安装linux系统中要求选择开发工具包的原因所在。
    
    此外，在执行configure分析软件需求时，还可以在“./configure”后面加上软件的安装路径以及安装所需的模块等等一些选项，来定制用户需要的软件功能。
    ```

    

3. 编译安装软件

    ```
    在验证软件安装环境后，正式进入软件的编译步骤，在进行编译前，首先了解下关于编译的一些基础知识。
    
    在linux系统下，make是我们经常用到的编译命令，无论是安装软件还是项目开发，都会经常用到编译安装命令，也就是make和make install，对于一个包含很多源文件的应用程序，使用make和makefile工具可以简单快速的解决各个源文件之间复杂的依赖关系，同时，make工具还可以自动完成所有源码文件的编译工作，并且可以只对上次编译后修改过的文件进行增量编译，因此，熟练掌握了make和makefile工具之后，源码安装软件就变的像windows下安装软件一样简单。
    ```

    

4. Makefile文件介绍

    ```
    make工具最主要的功能就是通过makefile文件来实现的，makefile文件是按照某种语法来进行编写的，文件中定义了各个源文件之间的依赖关系，并说明了如何编译源文件并生成可执行文件，它通过描述各个源程序之间的关系让make工具自动完成编译工作。
    
    在linux系统下，习惯用Makefile替代makefile文件，我们在执行configure通过后，就会在当前目录下生成这个文件，一般情况下在命令行输入make时，make命令会默认去寻找当前目录下的Makefile文件，如果我们用其它文件作为makefile，就要在make命令选项后指定makefile文件，例如我们设定编译规则文件为make_file，就需要使用以下命令指定：[root@WEBServer ~]#make -f make_file
    ```

    

5. Makefile举例

    ```
    下面我们通过举例了解下Makefile的编写原则和内容。
    
    例如有一个test程序，有三个源文件file1.c、file2.c、file3.c以及库文件LS编译生成，这三个源文件还分别包含了自己的头文件1.h、2.h、3.h。通常，C编译器将会输出三个目标文件file1.o、file2.o、file3.o。假定file1.c和file3.c都要声明用到一个apcs的文件，但是file2.c不用，也就是在file1.c和file3.c文件中都有类似“#include “apcs””的声明。
    
    下面这段代码就是对test程序的描述：
    
    1 #It is a example for describing makefile
    2 test: file1.o file2.o file3.o
    3 cc file1.o file2.o file3.o -LS -o test
    4 file1.o: file1.c 1.h apcs
    5 gcc -c file1.c
    6 file3.o: file3.c 3.h apcs
    7 gcc -c file3.c
    8 file2.o: file2.c c.h
    9 gcc -c file2.c
    
    这个描述文档其实就是一个makefile文件，第一行为注释行，第二行指定test程序有file1.o、file2.o、file3.o链接生成，第三行描述了如何从test所依赖的文件建立可执行文件，接下来的第四、六、八行定义了三个目标文件和它们依赖的“.c”、“.h”和“apcs”文件。剩下的第五、七、九行定义了如何从目标依赖的文件生成可执行文件。
    
    在了解Makefile文件结构后，我们就可以控制编译的选项，定制自己所需的软件功能。
    
    接下来，我们只需在命令行输入make命令，即可进入编译阶段，根据软件源程序的大小和系统的硬件配置，编译时间不定，编译完毕，会在当前目录以及子目录下生成相应的可执行文件，接下来就进入最后的安装软件阶段，在命令行输入make install命令，开始安装软件，安装进程会首先创建安装目录，如果我们没有指定安装的目录，安装程序默认会安装在系统的/usr/local目录下创建安装目录，然后将相应的文件和可执行程序从源码目录拷贝到安装目录下，这样安装就完毕了。
    ```



### 第五章 多虚拟机的操作

#### 1. 虚拟机克隆【掌握】

##### 1.1. 克隆前的准备工作

安装Minimal的CentOS虚拟机

![image-20210329164403065](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210329164403065-2694269.png)

```
我们后续的课程中，需要使用到3台虚拟机。因此，在这里我们再克隆两台出来即可。
虚拟机规划:
+------------+----------------+----------+
|   主机名    |       IP       |   防火墙  |
+------------+----------------+----------+
| qianfeng01 | 192.168.10.101 |   已关闭  |
+------------+----------------+----------+
| qianfeng02 | 192.168.10.102 |   已关闭  |
+------------+----------------+----------+
| qianfeng03 | 192.168.10.103 |   已关闭  |
+------------+----------------+----------+
```



##### 1.2. 修改IP地址

```
# 与qianfeng01的配置一样，修改ifcfg-ens33文件
# 如果是Mac平台使用Parallels Desktop安装的虚拟机，修改ifcfg-eth0文件
# 将qianfeng02的IP地址修改为192.168.10.102
# 将qianfeng03的IP地址修改为192.168.10.103
[root@qianfeng01 ~]# vi /etc/sysconfig/network-scripts/ifcfg-ens33

# 修改完IP地址后，需要重启网络服务
# 最好将3台虚拟机都重启一下
[root@qianfeng01 ~]# systemctl restart network
```



##### 1.3. 修改主机名

```
[root@qianfeng01 ~]# hostname qianfeng02
[root@qianfeng01 ~]# hostnamectl set-hostname qianfeng02
```



##### 1.4. 修改域名映射文件

```
[root@qianfeng02 ~]# vi /etc/hosts
...上方内容省略...
...下方添加域名的映射文件...
192.168.10.101 qianfeng01
192.168.10.102 qianfeng02
192.168.10.103 qianfeng03
```



##### 1.5. 虚拟机之间通信

```
# 使用ping命令，检测三台虚拟机之前是否可以互相通信。这一点非常重要，如果虚拟机之间无法通信，在后续的集群使用中会出现问题。
# 如果出现无法连接，按照以下方面进行问题检查
# 1. 检查IP地址
# 2. 检查host文件映射
# 3. 检查防火墙是否关闭

# qianfeng01
[root@qianfeng01 ~]# ping qianfeng02
[root@qianfeng01 ~]# ping qianfeng03

# qianfeng02
[root@qianfeng02 ~]# ping qianfeng01
[root@qianfeng02 ~]# ping qianfeng03

# qianfeng03
[root@qianfeng03 ~]# ping qianfeng01
[root@qianfeng03 ~]# ping qianfeng02
```



##### 1.6. 流程总结

```
1. 检查主机名（临时+永久）
2. 检查网卡是否启动
3. 关闭NetworkManager网络管理服务
4. 关闭防火墙
5. 更改IP地址
6. 更改主机名和主机映射
7. 使用远程连接工具（MobaXterm、FinalShell）进行连接
8. 如果出现问题，按照以上流程检查
```



#### 2. scp命令【重点】

```
cp命令，是拷贝的作用，可以实现将文件或者目录拷贝到另外一个位置。
scp命令，也是拷贝的作用，但是是远程拷贝，可以实现将文件或者目录拷贝到另外的一台机器上。
```

##### 2.1. 命令格式

```
# scp file 远程用户名@远程服务器:目标路径
# 示例: 
scp ~/data/a.log xiaoxiaoqian@qianfeng02:/home/xiaoxiaoqian/data/           # 将本机的~/data/a.log文件，拷贝到qianfeng02机器上的/home/xiaoxiaoqian/data下
scp ~/data/a.log root@qianfeng02:/root/data/    # 将本机的~/data/a.log文件，拷贝到qianfeng02机器上的/root/data下
# 上述两个拷贝的区别:
# 其实都是拷贝，区别就是使用的用户。在进行拷贝的时候，是需要你输入目标机器的指定用户的密码的。
# 例如: 
# 第一个拷贝，xiaoxiaoqian@qianfeng02，需要验证qianfeng02节点上的xiaoxiaoqian的密码。
# 第二个拷贝，root@qianfeng02，需要验证qianfeng02节点上的root的密码。

# 如果需要拷贝文件夹，添加-r
scp -r /usr/local/jdk root@qianfeng02:/usr/local/     # 将本机的/usr/local/jdk目录，拷贝到qianfeng02的/usr/local下
```

##### 2.2. 小技巧

```
# 1. 如果远程的用户，与本机的用户名相同，可以省略目标的用户名
#    示例: 本机登录的用户是root，远程的qianfeng02的用户也是root，因此可以省略
scp ~/a.log root@qianfeng02:/root/data/     # 拷贝给qianfeng02的root用户
scp ~/a.log qianfeng02:/root/data                   # root用户省略，直接写qianfeng02即可

# 2. 如果需要拷贝到远程服务器与本机相同的路径，可以使用$PWD
#    示例:
[root@qianfeng01 local]# pwd
/usr/local/
[root@qianfeng01 local]# scp -r jdk qianfeng02:/usr/local/      # 需要远程拷贝给qianfeng02的/usr/local目录，而本机当前也在这个路径下，因此可以简写
[root@qianfeng01 local]# scp -r jdk qianfeng02:$PWD                     # 用$PWD表示当前的工作路径
```



#### 3. ssh免密登录【重点】

##### 3.1. ssh的简介

```
ssh: 是一个远程登录的命令，可以远程登录到指定的虚拟机。
     其实，我们使用的远程连接工具，就是使用ssh实现的。
     
ssh在进行远程登录的时候，需要进行身份验证，输入远程登录用户的密码。
例如:
ssh root@qianfeng02         # 需要输入qianfeng02节点的root密码
ssh qianfeng02              # 当远程登录的用户名，与当前的用户名相同的时候，远程用户名可以省略
在我们后续的使用中，集群的节点之间需要频繁的进行通信，因此免密登录是必须要配置的。
如果不配置免密登录，则在很多时候集群通信的时候，需要输入密码。这就需要人工维护，成本很高，而且很麻烦。
```



##### 3.2. 免密登录的原理

```
1. qianfeng01使用ssh-keygen -t rsa生成公钥和私钥。
2. ssh-copy-id qianfeng02，将公钥拷贝到qianfeng02上，实际就是把公钥的内容追加到authorized_keys文件中。
3. 请求时: qianfeng01向qianfeng02发送连接请求时，附带主机、IP地址等信息。
4. qianfeng02收到请求后，去授权文件(authorized_keys)中查找qianfeng01的公钥，找到之后，随机生成一个字符串，并使用公钥加密，发送给qianfeng01。
5. qianfeng01收到密文后，用私钥解密，并把解密的结果返回给qianfeng02。
6. qianfeng02拿到解密结果之后，与之前的字符串进行比较，如果相同，则可以登录成功。
```

![ssh-login](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/ssh-login-2694269.png)

##### 3.3. 免密登录的实现

```
我们最终的目标是实现qianfeng01免密登录到qianfeng02，即在qianfeng01上远程登录qianfeng02不需要密码。
```

1.  在qianfeng01上生成一对钥匙，分为公钥和私钥

    ![image-20210329171418360](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210329171418360-2694269.png)

2.  当前用户的家目录下的.ssh目录中，会生成两个文件

    ![image-20210329171448603](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210329171448603-2694269.png)

3.  将公钥拷贝给要免密登录的机器

    ```
    最终要实现的是将id_rsa.pub，即公钥中的数据，添加到目标机器的authornized_keys文件中。你可以使用scp将公钥远程拷贝到目标机器，然后添加到目标文件末尾。但是麻烦。
    可以使用更简单的方式: ssh-copy-id 
    
    1. 如果没有这个命令，可以自己安装
       [root@qianfeng01 ~]# yum install openssh-clients -y
       
    2. 将公钥拷贝到目标机器
       [root@qianfeng01 ~]# ssh-copy-id qianfeng02
       
    3. 输入密码，完成拷贝
    ```

    ![image-20210329171837588](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210329171837588-2694269.png)

    

4.  拷贝完成后，会在要免密登录的机器上生成授权密码文件

    ![image-20210329171911630](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210329171911630-2694269.png)

    

5.  免密登录是单向的

    ```
    免密登录是单向的，即qianfeng01可以免密登录qianfeng02，但是qianfeng02登录qianfeng01还需要密码。
    因此，按照上述的步骤，自己实现qianfeng01、qianfeng02、qianfeng03之间彼此免密。
    ```





#### 4. 定时器crontab【掌握】

##### 4.1. crontab的简介

```
- 在Linux中，周期性的执行任务一般由cron这个守护进程来处理,它是一个linux下的定时任务执行工具，可以在无需人工干预的情况下运行作业。
   [ps -ef|grep cron]
- cron读取一个或多个配置文件，这些配置文件中包含了命令行及其调用时间。
- cron的配置文件称为“crontab”，是“cron table”的简写。
```



##### 4.2. cron服务

```
[root@qianfeng01 ~]# service crond start    //启动服务
[root@qianfeng01 ~]# service crond stop     //关闭服务
[root@qianfeng01 ~]# service crond restart  //重启服务
[root@qianfeng01 ~]# service crond reload   //重新载入配置
[root@qianfeng01 ~]# service crond status   //查看服务状态 
```



##### 4.3. cron的配置文件位置

```
1. /var/spool/cron/ 
2. 说明这个目录下存放的是每个用户包括root的crontab任务，每个任务以创建者的名字命名，比如tom建的crontab任务对应的文件就是/var/spool/cron/tom。
   一般一个用户最多只有一个crontab文件。
```



##### 4.4. crontab命令格式

```
作用：用于生成cron进程所需要的crontab文件 
格式：crontab  [-u username] -e
```



##### 4.5. 文件内容格式

```
格式如下:
* * * * * user-name command to be executed
共有六部分组成，分别表示:
分   时   日   月   星期  要运行的命令
解析：
    minute：     一小时中的哪一分钟 [0～59] 
    hour：       一天中的哪个小时 [0～23] 
    day：        一月中的哪一天 [1～31] 
    month：      一年中的哪一月 [1～12] 
    week：       一周中的哪一天 [0～6] 0表示星期天 
    commands：   执行的命令
书写注意事项 
    1 全都不能为空，必须填入，不知道的值使用通配符*表示任何时间 
    2 每个时间字段都可以指定多个值，不连续的值用,间隔，连续的值用-间隔。 
    3 命令应该给出绝对路径 
    4 用户必须具有运行所对应的命令或程序的权限
    5 */num   表示频率
```



##### 4.6. 应用案例

```
1 每天早上6点 
    0 6 * * * echo "Good morning." >> /tmp/test.txt 
    //注意 如果不进行追加 ，从屏幕上看不到任何输出，因为cron把任何输出都email到root的信箱了。
2 每两个小时 
    0 */2 * * * echo "Have a break now." >> /tmp/test.txt  
3 晚上11点到早上8点之间每两个小时和早上八点 
    0 23-8/2，8 * * * echo "Have a good dream" >> /tmp/test.txt
4 周一到周五下午，5点半提醒学生15分钟后关机
    30 17 * * 1-5 /usr/bin/wall < /etc/issue
    45 17 * * 1-5 /sbin/shutdown -h now
5 学校的计划任务， 12点14点，检查apache服务是否启动
    */2 12-14 * 3-6,9-12 1-5
6 每月 1、1 0、2 2日的4:45运行/apps/bin目录下的backup.sh
    45 4 1,10,22 * * /apps/bin/backup.sh
7 每周六、周日的 1 : 10运行一个find命令
    10 1 * * 6,0 /bin/find -name "core" -exec rm {} \;
8 在每天 18:00至23 :00之间每隔30分钟运行/apps/bin目录下的dbcheck.sh
    0,30 18-23 * * * /apps/bin/dbcheck.sh
9 每星期六的 11:00 pm运行/apps/bin目录下的qtrend.sh
    0 23 * * 6 /apps/bin/qtrend.sh
```



#### 5. 时间同步【掌握】

```
在实际生产环境中，很多软件或者很多任务对集群上的时间是否一致要求是很严格的。有的要求集群中的所有机器上的时间差不能在10分钟以外，有的要求所有机器上的时间差不能在30秒以外。这样，在技术上，就要涉及到怎么将时间同步的问题了。
```

##### 5.1. 同步网络的时间

**同步时间**

```
# 同步时间，需要使用ntpdate命令，如果没有可以使用yum安装。
# 使用ntpdate同步网络上的时间服务器的时间，例如: time.windows.com、ntp.aliyun.com
[root@qianfeng01 ~]# ntpdate -u ntp.aliyun.com
```

**定时同步**

```
# 可以将时间同步做成一个定时任务，每隔一段时间就同步一下网络的时间
[root@qianfeng01 ~]# crontab -e
* * * * * /usr/sbin/ntpdate -u ntp.aliyun.com
```



##### 5.2. 自定义时间服务器

```
如果集群没有联网，那么可以自己搭建一个时间服务器，让集群中的所有机器都同步局域网内的时间服务器。
# 1 选择集群中的某一台机器作为时间服务器，例如qianfeng01
# 2 保证这台服务器安装了ntp.x86_64。
# 3 保证ntpd 服务运行......
[root@qianfeng01 ~]# sudo service ntpd start
#   开机自启动:
[root@qianfeng01 ~]# chkconfig ntpd on
    
# 4 配置相应文件：
[root@qianfeng01 ~]# vi /etc/ntp.conf
    
    # Hosts on local network are less restricted.
    # restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap
  # 添加集群中的网络段位
    restrict 192.168.10.0 mask 255.255.255.0 nomodify notrap

    # Use public servers from the pool.ntp.org project.
    # Please consider joining the pool (http://www.pool.ntp.org/join.html).
    # server 0.centos.pool.ntp.org iburst    注释掉
    # server 1.centos.pool.ntp.org iburst      注释掉
    # server 2.centos.pool.ntp.org iburst    注释掉
    # server 3.centos.pool.ntp.org iburst    注释掉
    server 127.127.1.0     -master作为服务器
# 5 其他机器要保证安装ntpdate.x86_64

# 6 其他机器要使用root定义定时器
*/1 * * * * /usr/sbin/ntpdate -u qianfeng01 
```



### 第六章 Shell编程【重点】

#### 1. Shell的概念介绍

##### 1.1. 命令解释器

Shell是命令解释器(command interpreter)，是Unix操作系统的用户接口，程序从用户接口得到输入信息，shell将用户程序及其输入翻译成操作系统内核（kernel）能够识别的指令，并且操作系统内核执行完将返回的输出通过shell再呈现给用户，下图所示用户、shell和操作系统的关系：

![image-20210411153245838](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210411153245838-2694269.png)

一个系统可以存在多个shell，可以通过cat /etc/shells命令查看系统中安装的shell。

```
[root@qianfeng01 ~]# cat /etc/shells

/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
```

操作系统内核（kernel）与shell是独立的套件，而且都可被替换；不同的操作系统使用不同的shell； 同一个kernel之上可以使用不同的shell。 也可以查看当前shell环境是哪种：

```
[root@qianfeng01 ~]# echo $SHELL
```



##### 1.2. Shell脚本

Shell也是一门编程语言，即shell脚本。在此脚本中，我们可以使用一些编程语法来进行一些任务操作。  如：变量、类型、分支结构、循环结构、数组、函数等语法。 在shell脚本里，必须指定一种shell命令解释器。



#### 2. Shell编程规范

##### 2.1. 脚本文件的结构

```
1 文件的扩展名必须是.sh
2 文件的首行必须使用#!       指定script的运行shell环境(即脚本解释器)
   如：#!/bin/bash
3 脚本里的行注释符号为#
4 指令、选项、参数之间即使有多个空格仍会被视为一个空格。
5 tab键形成的空白也被视为一个空格键
6 空白行会被忽略
```



##### 2.2. 脚本文件的执行

-   使用bash程序调用执行，只需要有读权限即可

    ```
    [root@qianfeng01 ~]# sh *.sh      
    或者
    [root@qianfeng01 ~]# bash *.sh
    ```

    

-   直接写script，必须要有rx权限才行

    ```
    [root@qianfeng01 ~]# ./*.sh         
    绝对路径写法： /hadoop/*.sh
    相对路径写法： ./*.sh
    ```

    

-   借助变量PATH功能

    ```
    将*.sh放入~/bin目录下，因为PATH里拼接了~/bin目录。
    注意：~/bin目录必须自行创建
    ```



#### 3. Shell的变量

##### 3.1. 变量的用法

```
1. 变量的命名规则   
   - 命名只能使用英文字母，数字和下划线。首个字符不能以数字开头。
   - 字母习惯使用大写。
   - 中间不能有空格。
   - 不能使用标点符号。
   - 不能使用bash里的关键字（可用help命令查看保留关键字）

2. 变量的使用规则
   - 直接定义变量名称，没有类型需要强调（类似于数学中:x=1,y=2,z=x+y）
   - 赋值时，"="前后不能有空格
   - 命令的执行结果赋值给变量时，使用反单引号      如：TIME=`date`
   - 调用变量时，必须使用$       格式： $变量名    或    ${变量名}
```



##### 3.2. 变量的分类

Linux Shell中的变量可以分为三种变量: **局部变量**、**环境变量**、**特殊变量**。可以通过`set`命令查看系统中存在的所有变量

```
局部变量：也就是用户自定义的变量，在脚本中或命令中定义
环境变量：保存和系统操作环境相关的数据。$HOME、$PWD、$SHELL、$USER等等
特殊变量：
    一种是位置参数变量：主要用来向脚本中传递参数或数据，变量名不能自定义，变量作用固定。
    一种是预定义变量：是Bash中已经定义好的变量，变量名不能自定义，变量作用也是固定的。
```



##### 3.3. 局部变量

用户自定义的变量由字母或下划线开头，由字母，数字或下划线序列组成，并且大小写字母意义不同，变量名长度没有限制。

-   **定义变量**

    ```
    习惯上用大写字母来命名变量。变量名以字母表示的字符开头，不能用数字。
    ```

    

-   **调用变量**

    ```
    在使用变量时，要在变量名前加上前缀“$”.
    使用echo 命令查看变量值
    eg: [root@qianfeng01 ~]# echo $A
    ```

    

-   **变量赋值**

    -   第一种: 定义时赋值

        ```
        # 变量=值
        # 注意: 在上述的定义方式中，等号的左右两侧不能有空格
        # eg:
        [root@qianfeng01 ~]# STR="hello qianfeng"
        [root@qianfeng01 ~]# A=9
        ```

        

    -   第二种: 将一个命令的执行结果给变量赋值

        ```
        [root@qianfeng01 ~]# A=`ls -la` 反引号，运行里面的命令，并把结果返回给变量A
        [root@qianfeng01 ~]# A=$(ls -la) 等价于反引号
        [root@qianfeng01 ~]# AA=$((4+5))
        [root@qianfeng01 ~]# BB=`expr 4 + 5 `
        ```

        

    -   第三种: 将一个变量的值赋给另一个变量

        ```
        [root@qianfeng01 ~]# A=$STR
        ```

        

-   **变量叠加**

    ```
    # 变量叠加，就是将两个字符串变量的值叠加在一起，类似于Java中的字符串拼接操作。
    
    [root@qianfeng01 ~]# A=123
    [root@qianfeng01 ~]# C="$A"456
    [root@qianfeng01 ~]# D=${A}789
    
    # 单引号和双引号的区别
    # 现象：单引号里的内容会全部输出，而双引号里的内容会有变化
    # 原因：单引号会将所有特殊字符脱意
    
    [root@qianfeng01 ~]# NUM=10    
    
    [root@qianfeng01 ~]# SUM="$NUM hehe"     
    [root@qianfeng01 ~]# echo $SUM     # 输出10 hehe
    
    [root@qianfeng01 ~]# SUM2='$NUM hehe'     
    [root@qianfeng01 ~]# echo $SUM2    # 输出$NUM hehe
    ```

    

-   **删除变量**

    ```
    # 删除之前已经定义过的变量，之后就无法再使用这个变量了。
    
    # 撤销变量 A
    [root@qianfeng01 ~]# unset A 
    # 声明静态的变量 B=2 ，不能 unset
    [root@qianfeng01 ~]# readonly B=2 
    # 报错: -bash: unset: B: cannot unset: readonly variable
    [root@qianfeng01 ~]# unset B
    ```

    

-   **注意事项**

    关于局部变量的作用域的问题: 用户自定义的局部变量，作用于仅为当前的Shell环境。仅在当前的Shell示例中有效，其他的Shell启动的程序不能访问局部变量。

    ```
    [root@qianfeng01 ~]# echo '#!/bin/bash' > test.sh
    [root@qianfeng01 ~]# A=22
    [root@qianfeng01 ~]# echo 'echo $A' >> test.sh
    [root@qianfeng01 ~]# echo $A   结果:22
    [root@qianfeng01 ~]# bash test.sh   结果:打印为空,因为bash会开启新的shell进程
    ```



##### 3.4. 环境变量

用户自定义的局部变量，只能在当前的Shell中生效，而环境变量会在当前Shell和其所有的子Shell中生效。如果把环境变量写入相应的配置文件，那么这个环境变量就会在所有的Shell中生效。

**作用域:** 当前的Shell以及所有的子Shell

**声明变量:** export 变量名=变量值

**定义环境变量的常见配置文件:** 

-   **/etc/profile** -> 针对系统所有的用户生效，此文件应用于所有用户每次登录系统时的环境变量定义。每次用户登录的时候，都会加载这个文件。
-   **$HOME/.bash_profile** -> 针对特定用户生效，HOME/.bash_profile文件中的定义。



##### 3.5. 位置参数变量

| 变量                                                         | 描述       |
| ------------------------------------------------------------ | ---------- |
| 0表示命令本身，9表示第1到第9个参数，10以上的参数需要使用大括号包含，例如${10} |            |
| 1 n"的形式输出所有参数                                       |            |
| 1" "n" 的形式输出所有参数                                    |            |
| $#                                                           | 参数的个数 |

-   **@ 的区别**
    -   他们都表示传递给函数或脚本的所有参数，不被双引号包含时，都以 `"$1""$2""$3"..."$n"`的形式输出所有参数。
    -   当他们被双引号包含时
        -   "$*"会将所有的参数作为一个整体，以 `$1 $2 $3 ... $n` 的形式输出所有的参数。
        -   "$@"会将各个参数分开，以 `"$1""$2""$3"..."$n"` 的形式输出所有参数。



-   Shell脚本执行测试

    ```
    # 执行脚本 test1.sh 
    #!/bin/bash
    echo "test \$*"
    for i in $*
    do
        echo $i
    done
    
    echo "test \$@"
    for i in $@
    do
        echo $i
    done
    
    echo "test \"\$*\""
    for i in "$*"
    do
        echo $i
    done
    
    echo "test \"\$@\""
    for i in "$@"
    do
        echo $i
    done
    
    
    输出结果:
    [root@qianfeng01 ~]# sh test1.sh a b
    test $*
    a
    b
    test $@
    a
    b
    test "$*"
    a b
    test "$@"
    a
    b
    ```



##### 3.6. 预定义变量

| 变量 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| $?   | 执行上一个命令的返回值。执行成功，返回0；执行失败，返回非0   |
| $$   | 当前进程的进程号(PID)，即当前脚本执行时生成的进程号          |
| $!   | 后台运行的最后一个进程的进程号(PID)，最近一个被放入后台执行的进程 |

-   测试 $?

    ```
    [root@qianfeng01 ~]# ls ; echo $?
    # 分析: 这里的意思是依次顺序执行两个命令
    # 如果分号前的命令可以执行，$?会返回0; 否则会返回非0的一个数字
    ```

    

-   测试 $$

    ```
    [root@qianfeng01 ~]# pwd > /dev/null
    [root@qianfeng01 ~]# echo $$
    ```

    

-   测试 $!

    ```
    [root@qianfeng01 ~]# ls /etc > /dev/null &
    [root@qianfeng01 ~]# echo $!
    ```



#### 4. read命令

##### 4.1. 命令说明

```
read命令，可以从控制台读取用户输入的内容，并且给指定的变量进行赋值。
命令的基本格式为: read [option] variable
常见的option:
-p : 提示语句，在输入之前，给用户提示的信息
-n : 限制输入的字符数量，到达这个数量的字符的时候，会自动的停止输入
-t : 等待时间，单位为秒。当到达这个等待时间的时候，会自动的结束输入，并且不会读取任何输入的内容!
     即，输入操作且回车，必须在指定的时间内完成。
-s : 隐藏输入的内容。在控制台上不显示任何输入的内容，常见于密码的输入
```



##### 4.2. read的实例

```
[root@qianfeng01 ~]# read -t 30 -p "please input your name:"  NAME

[root@qianfeng01 ~]# echo $NAME

[root@qianfeng01 ~]# read -s -p "please input your age :"  AGE

[root@qianfeng01 ~]# echo $AGE  注意：如果隐藏输入，这里的结果是看不到的

[root@qianfeng01 ~]# read -n 1 -p "please input your sex  [M/F]:"  GENDER

[root@qianfeng01 ~]# echo $GENDER
```



##### 4.3. 注意事项

-   在输入的时候，如果输错了需要删除的时候，执行`ctrl+delete`
-   符号不要输入中文
-   变量与之前的内容需要保持有空格



#### 5. 运算

##### 5.1. expr

```
格式 :expr m + n 或$((m+n)) 注意expr与运算符和变量间要有空格  

sum=$((m+n))   中=与$之间没有空格

expr命令：对整数型变量进行算术运算

(注意：运算符前后必须要有空格)

[root@qianfeng01 ~]# expr 3 + 5 

[root@qianfeng01 ~]# expr 3 – 5

[root@qianfeng01 ~]# echo `expr 10 / 3`     

10/3的结果为3，因为是取整

[root@qianfeng01 ~]# expr  3  \* 10     

\ 是转义符
```



##### 5.2. 示例

计算（2 ＋3 ）×4 的值

1 .分步计算

```
[root@qianfeng01 ~]# S=`expr 2 + 3`

[root@qianfeng01 ~]# expr $S  \* 4
```

2.一步完成计算

```
[root@qianfeng01 ~]# expr  `expr 2 + 3` \* 4
[root@qianfeng01 ~]# S=`expr  \` expr 2 + 3  \`  \* 4`
[root@qianfeng01 ~]# echo $S
       或
[root@qianfeng01 ~]# echo $(((2 + 3) * 4))
```



##### 5.3. {}

```
$()与${}的区别

$( )的用途和反引号``一样，用来表示优先执行的命令

eg: [root@qianfeng01 ~]# echo $(ls /root)

${ } 就是取变量了  

eg：[root@qianfeng01 ~]# echo ${PATH}

$((运算内容)) 适用于数值运算

eg: [root@qianfeng01 ~]# echo $((3+1*4))
```



#### 6. 字符串

##### 6.1. 字符串的脚本用法

```
- 字符串不能单独出现，必须要配合变量。
- 字符串可以使用单引号['  ']，也可以使用双引号[" "]，也可以不用引号
- 单引号内的任何字符没有任何意义，都会原样输出
- 单引号内使用变量是无效的，单引号内不能出现单引号
- 双引号内可以使用变量
- 双引号内可以使用转义字符
- 在字符串拼接操作时，我们可以进行无缝拼接，或者是在双引号里使用变量
```



##### 6.2. 字符串的长度

```
可以使用${#variable} 或者 expr length "${variable}"。因为expr是指令，所以别忘记使用反单引号``或者是$()

直接看案例：

[root@qianfeng01 ~]# vim test3.sh

#!/bin/bash
var='welcome to china'
length1=${#var}
length2=$(expr length "${var}")  <==$()写法
length3=`expr length "$var"`     <==反单引号写法
```



#### 7. Shell数组

##### 7.1. 数组的使用规则

```
- 在/bin/bash这个shell中，只有一维数组的概念，并且不限定数组的长度。
- 数组的元素下标是从0开始的，
- 获取数组的元素要使用下标
- 下标使用不当，会报错
```



##### 7.2. 数组的定义

```
定义格式： variable=(值1  值2    …  值n)   
注意：元素之间除了使用空格作为分隔符，还可以使用换行符。

或者 

name[0]=值1     
name[1]=值2     
…     
name[n]=值n
```



##### 7.3. 读取数组

```
${variable[index]}： 读取index索引上的元素
${variable[*]}或者${variable[@]}：读取所有元素  
${#variable[*]}或者${#variable[@]} :  读取数组的长度  
[root@qianfeng01 ~]# vim test3.sh

#!/bin/bash
citys=(cc sh bj sd hlj)
hobby[0]=book
hobby[1]=film
hobby[2]=music

echo ${citys[0]}      <==cc
echo ${hobby[*]}      <==book film music
echo ${#hobby[@]}     <==3
[root@qianfeng01 ~]# bash test3.sh
```



#### 8. test测试命令

![image-20210411163134487](https://shawn-qianfeng.oss-cn-beijing.aliyuncs.com/img/image-20210411163134487-2694269.png)

-   通常test命令不单独使用，而是与if语句连用，或是放在循环结构中

    ```shell
    # && 表示测试通过的处理逻辑
    # || 表示测试不通过的处理逻辑
    [root@qianfeng01 ~]# test -e file && echo "exists" || echo "not exists"
    ```

    

-   判断符号[]

    除了好用的test之外，我们还可以使用中括号来进行检测条件是否成立

    ```
    [root@qianfeng01 ~]# [  -r  filename ]      检测filename是否有可读权限
    [root@qianfeng01 ~]# [  -f  filename -a  -r  filename ]     检测filename是不是普通文件并且有可读权限
    ```

    

#### 9. 条件控制

##### 9.1. if条件语句-单分支

```
if-else语句，是非常简单，也是非常基础的条件语句。在Java中我们已经非常熟悉它的逻辑了，在这里，我们重点关注在Shell中的语法
if [ 条件判断 ]
then
    条件成立的执行逻辑
fi
```

或者

```
if [ 条件判断 ] ; then
    条件成立的执行逻辑
fi
```



案例:

```
#!/bin/bash

A=$1
if [ $A == "stop" ]
then
    systemctl stop firewalld
fi
```



**注意事项:**

1.  if语句需要使用fi结尾，和一般语言使用大括号结尾不同。
2.  [ 条件判断 ] 就是使用test命令判断，所以中括号和条件判断之间必须有空格
3.  then后面跟符合条件之后执行的程序，可以放在[]之后，用;分隔，也可以换行写入，就不需要;了
4.  if与中括号之间必须要有空格



##### 9.2. if条件语句-多分支

```
if [ 条件判断式1]
then
      当条件判断式1成立时，执行程序1
elif  [ 条件判断式2 ]  
then      
    当条件判断式2成立时，执行程序2
      ...省略更多条件
else
      当所有条件都不成立时，最后执行此程序
fi
```

案例1:

```
#!/bin/bash
read -p "please input your name:"  NAME
echo  $NAME
if [ $NAME == root ]
then
      echo "hello ${NAME},  welcome !"
elif [ $NAME == tom ]
then
    echo "hello ${NAME},  welcome !"
else
    echo "oh, get out here !"
fi
```

案例2:

```
编写一个坐车脚本，要求:
脚本名字: home.sh
逻辑: 从外面传入一个参数，根据参数判断: 1. 坐飞机，2. 坐火车，3. 坐火箭，4. 不回了
#!/bin/bash

CHOICE=$1
if [ $CHOICE == 1 ]
then
    echo "坐飞机"
elif [ $CHOICE == 2 ]
then
        echo "坐火车"
elif [ $CHOICE == 3 ]
then
    echo "坐火箭"
elif [ $CHOICE == 4 ]
then
    echo "不回了"
else
    echo "输入的参数有问题啊"
fi
```



##### 9.3. case

```shell
# 在Java中，我们学习过switch-case结构。这种结构，我们可以捕获一个变量的值，对这个变量取到的某些值进行不同的处理。在Shell中，也有类似的结构，就是case。
# case命令是一个多分支的if/else命令，case变量的值用来匹配value1、value2、value3等不同的值。
# 匹配到后，则执行跟在后面的命令，直到遇到双分号为止。
# 类似于if命令，case命令使用esac作为终止符。
# case行尾必须为单词in。
# 每个分支必须以右括号结束。
# 匹配模式中可使用方括号表示一个连续的范围，如[0-9]；使用竖杠符号“|”表示或。
# 最后的“\*）”表示默认模式，当使用前面的各种模式均无法匹配该变量时，将执行“*）”后的命令序列。

# case的格式

CMD=$1
case $CMD in							# case行尾必须为 变量 in，表示捕获这个变量的值
start)									# 需要匹配到的值，需要以右括号作为结尾
    echo "starting"						# 匹配到之后，执行的逻辑语句，即一个分支
    ;;									# 一个分支的逻辑，需要以;;作为结束，不会向下穿透
stop)
    echo "stopping"
    ;;
*)										# *表示以上分支都不满足的情况，类似于switch-case中的default
    echo "please using start|stop"
    ;;
esac									# case语句，需要以esac结束
```



#### 10. 循环

##### 10.1. for循环

```
for循环命令用来在一个列表条目中执行有限次数的命令。

比如，你可能会在一个姓名列表或文件列表中循环执行同个命令。

for命令后紧跟一个自定义变量、一个关键字in和一个字符串列表（可以是变量）。

第一次执行for循环时，字符串列表中的第一个字符串会赋值给自定义变量，然后执行循环命令，直到遇到done语句；

第二次执行for循环时，会右推字符串列表中的第二个字符串给自定义变量

依次类推，直到字符串列表遍历完。
```

方式一:

```
for N in 1 2 3 
do
      echo $N
done

或

for N in 1 2 3; do echo $N; done

或

for N in {1..3}; do echo $N; done

或

for N in {1,2,3}; do echo $N; done  

注意事项: {}中的数字之间不能有空格
```

方式二:

```
for ((i = 0; i <= 5; i++))
do
      echo "welcome $i times"
done
    
或

for ((i = 0; i <= 5; i++)); do echo "welcome $i times"; done
```



练习: 计算从1到100的和

```
#!/bin/bash

SUM=0
for((i=1;i<=100;i++))
do
    SUM=$(($SUM + $i))
done
echo $SUM
```



##### 10.2. while循环

```
while命令根据紧跟其后的命令(command)来判断是否执行while循环，当command执行后的返回值(exit status)为0时，则执行while循环语句块，直到遇到done语句，然后再返回到while命令，判断command的返回值，当得到返回值为非0时，则终止while循环。
```

-   第一种:

    ```
    while [ expression ]
    do
        command
    done
    ```

    ```
    # 练习: 求1-10各个数字的平方和
    
    #!/bin/bash
    num=1
    while [ $num -le 10 ]
    do
          sum=`expr $num \* $num`
          echo $sum
          num=`expr $num + 1`
    done
    
    num=1
    while [ $num -le 10 ]
    do
          sum=$(( $num * $num ))
          echo $sum
          num=$(( $num + 1 ))
    done
    ```

    

-   第二种:

    ```
    while  ((  expression  ))
    do
    command
    …
    done
    ```

    ```
    # 练习: 求1-10各个数字的平方和
    
    #!/bin/bash
    i=1
    while (( i<= 10 ))
    do
        sum=$(( $i * $i ))
        echo $sum
        i=$(( $i + 1 ))
    done
    ```



#### 11. 函数

```
函数代表着一个或一组命令的集合，表示一个功能模块，常用于模块化编程。

以下是关于函数的一些重要说明： 

在shell中，函数必须先定义，再调用

使用return value来获取函数的返回值

函数在当前shell中执行，可以使用脚本中的变量。
```

函数的格式如下:

```
函数名()
{
    命令1…
    命令2…

    return 返回值变量
}

结构:

[ function ] funname [()]
{

    action;

    [return int;]
}

function start()  / function start  / start()
编写脚本
[root@qianfeng01 ~]# bash function1.sh  

#!/bin/bash
function start() {
    echo "starting"
}
function stop {
    echo "stopping"
}
restart() {
    echo "restarting"
}
$1
```

**注意事项:**

```
- 如果函数名后没有（），在函数名和{ 之间，必须要有空格以示区分。
- 函数返回值，只能通过$? 系统变量获得，可以显示加：return 返回值
- 如果不加return，将以最后一条命令的运行结果，作为返回值。 
- return后的内容以字符串的形式写入，但是执行时会自动转成数值型，范围：数值n(0-255)
```



#### 12. 脚本调试

-   **-x:** 执行脚本，并显示所有变量的值

    -   ```
        [root@qianfeng01 ~]# vim test.sh
        
        #!/bin/bash
        a=$1
        b=3
        echo "b:"+$b
        c=$a
        echo $a
        
        [root@qianfeng01 ~]# bash -x test.sh
        ```

    -   ```
        [root@qianfeng01 ~]# vim test.sh
        
        #!/bin/bash
        a=$1
        set -x      # 这里是添加的set -x
        b=3
        echo "b:"+$b
        c=$a
        echo $a
        
        [root@qianfeng01 ~]# bash test.sh 1
        + b=3
        + echo b:+3
        b:+3
        + c=1
        + echo 1
        1
        # 这里是手动添加的set -x，只对set -x以下的部分进行调试
        ```

    

-   **-n:** 不执行脚本，只是检查语法。将返回所有语法错误，例如函数没有正确闭合等

    -   ```
        [root@qianfeng01 ~]# vim test1.sh
        
        #!/bin/bash
        for N in 1 2 3
        do
            echo $N
        # 这里忘记写done，因此for循环没有正确的闭合
        
        function start() {
            echo "hahaha"
        }
        start
        
        [root@qianfeng01 ~]# bash -n test1.sh
        test1.sh: line 20: syntax error: unexpected end of file
        ```

        

-   **-v:** 执行并显示脚本内容



### 第七章 awk【了解】

#### 1. cut命令

```
cut [选项] 文件名        默认分割符是制表符，一个制表符代表一列

选项：

-f  列号：    提取第几列

-d 分隔符：    按照指定分隔符分割列

eg:[root@qianfeng01 ~]#   cut -f  2  aa.txt   提取第二列

eg:[root@qianfeng01 ~]#   cut -d ":" -f 1,3 /etc/passwd  以:分割，提取第1和第3列

eg:[root@qianfeng01 ~]#   cat /etc/passwd | grep /bin/bash | grep -v root | cut -d ":" -f 1    获取所有可登陆的普通用户用户名

cut的局限性    不能分割不定长度的空格   

比如:df -h  不能使用cut分割

[root@qianfeng01 ~]#  df -h | grep sda1 | cut -f 5
```



#### 2. awk介绍

```
一个强大的文本分析工具

把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分再进行各种分析处理。

语法：awk ‘条件1{动作1}条件2{动作2}...’文件名

条件（Pattern）:

一般使用关系表达式作为条件： >   >=  <=等

动作（Action）：

格式化输出

流程控制语句

eg:[root@qianfeng01 ~]#   df -h | awk '{print $1 "\t" $3}'      显示第一列和第三列 
```



#### 3. FS内置变量

```
eg: 输出可登陆用户的用户名和UID

[root@qianfeng01 ~]#  cat /etc/passwd | grep "/bin/bash" | awk 'BEGIN {FS=":"} {print \$1 "\t"\$3 }'    

这里使用FS内置变量指定分隔符为：,而且使用BEGIN保证第一行也操作，因为awk命令会在读取第一行后再执行条件

注意:指定分隔符用-F更简单

eg: [root@qianfeng01 ~]#  cat /etc/passwd | grep "/bin/bash" | awk -F:  '{print \$1 "\t"\$3 }'   效果同上

eg: 判断一下根目录的使用情况

[root@qianfeng01 ~]#  df -h |grep sda1 | awk '{print \$5}' | awk -F% '{print \$1} \$1<80{print "info"}$1>80{print "warning"}'

BEGIN  在所有数据读取之前执行

eg: [root@qianfeng01 ~]#  awk 'BEGIN {printf "first Line \n"} {printf $2 }' aa.txt     在输出之前使用BEGIN输出内容

END 在所有数据执行之后执行

eg: [root@qianfeng01 ~]#  awk 'END {printf "The End \n"} {print $2}' aa.txt   所有命令执行完后，输出一句"The End"

[root@qianfeng01 ~]#  df -h | grep sda2 | awk '{print \$5}' | awk -F% '{print \$1}'

[root@qianfeng01 ~]#  df -h | grep sda2 | awk '{print $5}' | cut -d"%" -f 1

eg: 获取所有用户信息里的用户名：

[root@qianfeng01 ~]#  cat /etc/passwd | awk -F: '{print $1}'

[root@qianfeng01 ~]#  awk -F: '{print $1}' /etc/passwd

eg: 获取当前机器的ip地址： ifconfig  eth0

[root@qianfeng01 ~]#  ifconfig eth0 | grep 'inet addr' | awk -F: '{print \$2}' | awk '{print $1}'
```



### 第八章 sed【了解】

#### 1. sed命令介绍

sed: stream editor 

sed是一个非交互性文本流编辑器。它编辑文件或标准输入导出的文本拷贝。标准输入可能是来自键盘、文件重定向、字符串或变量，或者是一个管道的文本。

注意： sed并不与初始化文件打交道， 它操作的只是一个拷贝，然后所有的改动如果没有重定向到一个文件，将输出到屏幕。

语法：sed [选项]’[动作]’ 文件名 

常用选项:

-n  使用安静（silent）模式。显示经过sed特殊处理的数据。

-e  允许多点编辑。

-i   直接修改读取的档案内容，而不是由屏幕输出。

| 命令 | 功能描述                                                     |
| ---- | ------------------------------------------------------------ |
| a    | 新增， a 的后面可以接字符串，在下一行出现  注意：最好动作使用单引号，可以自动换行 |
| c    | 替换                                                         |
| d    | 删除                                                         |
| i    | 插入， i 的后面可以接字符串                                  |
| p    | 打印                                                         |
| s    | 查找并替换，例如 1,20s/old/new/g                             |

#### 2. sed命令的使用

```
eg: [root@qianfeng01 ~]#  sed  '2p' sed.txt    显示第二行和所有数据

eg: [root@qianfeng01 ~]#  sed -n '2,3p' sed.txt   只显示第二和第三行

eg: [root@qianfeng01 ~]#  df -h | sed -n '1p'   接收命令结果数据

eg: [root@qianfeng01 ~]#  sed  ‘2a bing’ sed.txt 在第二行后面添加数据

eg: [root@qianfeng01 ~]#  sed  ‘4i fengjie \  chenchen’  sed.txt 在第4行之前添加两行数据

eg: [root@qianfeng01 ~]#  sed  ‘2c this is replace’  sed.txt 替换第二行数据

eg: [root@qianfeng01 ~]#  sed  ‘s/it/edu360/g’ sed.txt  把sed.txt文件中的it替换为edu360,并输出

eg: [root@qianfeng01 ~]#  sed  -e  '1s/1/34/g;3s/yangmi//g'  sed.txt    同时进行多个替换

eg: [root@qianfeng01 ~]#  sed  –i  ‘s/it/edu360/g’  sed.txt     要想真正替换，需要使用-i参数

案例代码演示:
[root@qianfeng01 ~]# more sed.txt
sldx
it spark
hadoop edu
it hadoop
it scala

[root@qianfeng01 ~]# sed -e 's/it/edu360/g' sed.txt
sldx
edu360 spark
hadoop edu
edu360 hadoop
edu360 scala
```

eg: 使用sed获取机器的ip地址

注意:在对文件进行匹配的时候,^是一个文件的开始   $是一个文件的结束          

^.*addr:意思是说从开始到addr全部的内容

```
centos6.9系统命令
[root@qianfeng01 ~]#  ifconfig eth0 | grep 'inet addr'| sed 's/^.*addr://g' | sed 's/  Bcast.*$//g'
centos7.7系统命令
[root@qianfeng01 ~]#  ip addr | grep "inet " | grep "eth0" | sed 's/^.*inet //g' | sed 's/\/24.*$//g'
```
















## 五 实战应用

### 案例1: 二进制安装JDK

#### 1. 准备安装包

```shell
# 准备JDK的安装包，使用SSH工具，上传到Linux，放到 ~/softwares 路径下
[root@qianfeng01 ~]# cd ~/softwares && ls
```



#### 2. 解压安装

```shell
# 将刚才上传的JDK的安装包直接解压即可完成安装
# 安装路径规划: /usr/local
[root@qianfeng01 ~]# tar -zxvf jdk-8u221-linux-x64.tar.gz -C /usr/local
```



#### 3. 更名操作【非必需】

```shell
# 将JDK更名，为了方便后续的使用
[root@qianfeng01 ~]# mv jdk1.8.0_221/ jdk
```



#### 4. 配置环境变量

```shell
# 到第三步，JDK的安装已经结束。但是如果需要正常使用的话，还需要配置环境变量
# Linux的环境变量和Windows的环境变量配置大同小异
```

##### 4.1. 环境变量配置文件

```
1. /etc/profile : 系统级的配置，针对所有的用户生效。每一个用户登录的时候都会加载这个文件。【选择这个】
2. ~/.bash_profile : 用户级别的配置，针对当前用户生效。登录特定用户的时候会加载这个文件。
```

##### 4.2. 编辑配置文件

```shell
[root@qianfeng01 ~]# vim /etc/profile
...前面内容省略...
...Shift + g 跳转到末尾，在下一行添加...
# Java Environment
export JAVA_HOME=/usr/local/jdk		# 你的JDK的安装路径
export PATH=$PATH:$JAVA_HOME/bin	# 配置path
```

##### 4.3. 重新引导

```shell
# 重新引导/etc/profile文件，使得刚才的配置生效
[root@qianfeng01 ~]# source /etc/profile
```



#### 5. 验证

```
在任意的路径下，使用javac，如果提示的不是“未找到命令”，则配置完成！
```



### 案例2: rpm安装MySQL

#### 1. 安装包准备

```
通过SSH工具，将MySQL的RPM安装包上传到Linux的 ~/softwares 目录下。

MySQL的RPM安装包，可以在MySQL的官网下载，也可以直接使用课程提供的资料。
注意事项:
- 在官网下载安装包的时候，注意版本号
- mysql-8.0.26-1.el7.aarch64.rpm-bundle.tar
- 这里的el7，表示适用于CentOS7的版本。注意，不要下载el6的。
- aarch64: 这个表示arm版本的CentOS，如果是amd64架构的，可以选择x64。
```



#### 2. 环境准备

```shell
# CentOS7中，系统默认采用的数据库是mariadb，这个数据库与MySQL冲突！
# 因此，在安装MySQL之前，需要先将其卸载！
[root@qianfeng01 ~]# rpm -qa | grep mariadb			# 查询是否已经安装了mariadb
mariadb-libs-5.5.68-1.el7.aarch64				    # 查询结果。如果没有这个结果，说明没有安装。

# 强制卸载mariadb
# --nodeps: 强制卸载，RPM卸载程序的时候，如果这个程序被其他的程序依赖，是无法卸载的。
#           此时，就需要使用--nodeps，忽略依赖，强制卸载。
# 下面的卸载命令中，卸载的包是上方查询到的包
[root@qianfeng01 ~]# rpm -e mariadb-libs-5.5.68-1.el7.aarch64 --nodeps
```



#### 3. 安装MySQL

```shell
# 安装MySQL, 其实就需要安装 mysql-community-server, 但是它依赖其他的包
[root@qianfeng01 mysql]# rpm -ivh mysql-community-common-8.0.26-1.el7.aarch64.rpm
[root@qianfeng01 mysql]# rpm -ivh mysql-community-client-plugins-8.0.26-1.el7.aarch64.rpm
[root@qianfeng01 mysql]# rpm -ivh mysql-community-libs-8.0.26-1.el7.aarch64.rpm
[root@qianfeng01 mysql]# rpm -ivh mysql-community-libs-compat-8.0.26-1.el7.aarch64.rpm
[root@qianfeng01 mysql]# rpm -ivh mysql-community-client-8.0.26-1.el7.aarch64.rpm
[root@qianfeng01 mysql]# yum install -y net-tools
[root@qianfeng01 mysql]# rpm -ivh mysql-community-server-8.0.26-1.el7.aarch64.rpm
```



#### 4. MySQL的配置

##### 4.1. 启动MySQL服务

```shell
# 查看MySQL服务的运行状态
[root@qianfeng01 ~]# systemctl status mysqld
# 如果MySQL服务没有开启，则开启
[root@qianfeng01 ~]# systemctl start mysqld
```



##### 4.2. 登录到MySQL

```shell
# 在第一次开启MySQL服务的时候，会自动生成一个随机的密码
[root@qianfeng01 ~]# grep password /var/log/mysqld.log			# 到mysqld.log文件中查找password
2020-12-16T07:47:14.117739Z 1 [Note] A temporary password is generated for root@localhost: pVLJs6&o(QQe

# 使用这个随机密码登录到MySQL
[root@qianfeng01 ~]# mysql -u root -p
pVLJs6&o(QQe		# 这里用自己的密码登录
```



##### 4.3. 修改密码

```shell
# 1. 修改MySQL的密码策略（安全等级）
#    MySQL默认的密码安全等级有点高，在设置密码的时候，必须同时包含大小写字母、数字、特殊字符，以及对位数有要求
show variables like '%validate_password%';			# 查看密码策略
set global validate_password.policy=LOW;			# 修改密码策略等级为LOW
set global validate_password.length=4;				# 密码的最小长度
set global validate_password.mixed_case_count=0;	# 设置密码中至少要包含0个大写字母和小写字母
set global validate_password.number_count=0;		# 设置密码中至少要包含0个数字
set global validate_password.special_char_count=0;	# 设置密码中至少要包含0个特殊字符

# 2. 修改密码
alter user root@localhost identified by '123456';

# 3. 远程授权
create user root@'%' identified by '123456';
grant all privileges on *.* to 'root'@'%' with grant option;
```



#### 5. 重新登录

```
试着退出MySQL，用新的密码重新登录。如果可以登录成功，那么MySQL的安装就到此结束！
MySQL在安装完成后，是不需要配置环境变量的。因为RPM的安装特性，启动脚本会被放到/usr/bin下。这个路径本来就在环境变量里面，不需要单独配置。
```





## 六 教学总结

通过本篇内容的学习，大家可以掌握Linux的安装、Linux常用的命令和Linux的软件的安装。Linux是我们以后的课程中使用到的环境，是必须需要熟练掌握的。Linux的命令和软件的安装，在后续的课程中每一天都在使用！

本章重点内容:

- 虚拟机的安装: Windows和Mac平台的不同软件，按自己的需求安装即可
- Linux的安装: 安装Minimal的版本，分区的规划
- 网络的选择: 了解桥接、NAT、HostOnly的区别，重点掌握NAT模式
- 虚拟机网络的配置及SSH工具的使用: 后续的课程中不会在Linux本身中进行操作，需要使用SSH工具远程连接。企业中也是这样的
- Linux的命令: 核心重点！
- Linux的软件安装: 核心重点！



## 七 课后作业

**1. Linux中，用来分隔目录名和文件名的分隔符是什么**

```
/
```

**2. 如何回到家目录**

```
cd ~
cd /root 或者 cd /home/qianfeng01
cd #
```

**3. 如何切换到上一个工作路径**

```
cd -
```

**4. 如何查看logfile文件的后10行**

```
tail logfile
```

**5. 删除一个非空的文件夹 ~/data，如何实现**

```
rm -rf ~/data
```

**6. 将~/data/log重命名为/log.COMPLETE**

```
mv ~/data/log /log.COMPLETE
```

**7. 在vi编辑器中，怎么快速定位到第100行**

```
:100
100G
```

**8. 在vi编辑器中，如何用new替换掉单词old**

```
:s/old/new/g
```

**9. 在vi编辑器中，如何用/usr/local替换掉原来的/opt/software**

```
:s#/usr/local#/opt/software#g
```

**10. 如何将~/data下所有的.xml文件替换为.xml.bak**

```
rename .xml .xml.bak ~/data/*
```

**11. 查找/usr/local/hadoop/share下所有的xml文件并拷贝到~**

```
find /usr/local/hadoop/share/doc -name "*.xml" -exec cp {} ~ \;
```

**12. 如何实现每分钟向ntp.aliyun.com同步时间**** 

```
[root@qianfeng01 ~]# crontab -e
* * * * * /usr/sbin/ntpdate -u ntp.aliyun.com
```

**13. 如何实现qianfeng01免密登录到qianfeng02**

```
1. 在qianfeng01节点生成自己的秘钥
   ssh-keygen -t rsa
   
2. 将自己的公钥发送给qianfeng02，并追加到qianfeng02的authronized_keys中
   ssh-copy-id qianfeng02
```

**14. 编写Shell脚本，模拟登录**

```
请输入用户名: 【此处输入用户名】
请输入密码: 【此处输入密码，密文输入】
【如果用户名为root，密码为123456，则提示登录成功，否则登录失败】
```

```shell
#!/bin/bash

read -p "请输入用户名: " USERNAME
read -p -s "请输入密码: " PASSWORD

if [ $USERNAME == "root" -a $PASSWORD == "123456" ]
then
    echo "登录成功"
else 
    echo "登录失败"
fi
```

**15. 编写Shell脚本，计算100以内的偶数和**

```shell
#!/bin/bash

SUM=0
for NUM in {1..100}
do
    if [ $[NUM%2] == 0 ]
    then 
        SUM=$(( $SUM + $NUM ))
    fi
done

echo $SUM
```

```shell
#!/bin/bash

SUM=0
for ((NUM=0;NUM<=100;NUM+=2))
do
    SUM=$(( $SUM + $NUM ))
done

echo $SUM
```

```shell
#!/bin/bash

NUM=0
while [ $NUM -le 100 ] 
do
    SUM=$(($SUM + $NUM))
    $NUM=$(($NUM + 2))
done
```

**16. 编写shell脚本，使用循环和read命令，给一个数组NAMES赋值10个元素。当输入10个名字的时候，不再输入；或者当输入的过程中，有exit的时候，也不再输入。最后将输入中所有的元素输出。**

```shell
#!/bin/bash

for COUNT in {1..10}
do
    read NAME
    if [ $NAME == "exit" ]
    then
        break
    fi
    NAMES[$COUNT]=$NAME
done

echo ${NAMES[*]}
```

**17. 定义一个cal函数，可以计算任意三个数字的乘积。**

```shell
#!/bin/bash

function calcu {
    echo $(( $1 * $2 * $3 ))
}

calcu $1 $2 $3
```



**18. 已知 bash mvfile.sh  /root  /root/gzs  .gz可以把/root下所有的.gz文件移动到/root/gzs中，实现这个脚本**

```
#!/bin/bash

`find $1 -name "*$3" -exec mv {} $2 \;`
```





## 八 解决方案

### 1. Linux中、如何调整文件最大打开数

```
修改linux的软硬件限制文件/etc/security/limits.conf. 

在文件尾部添加如下代码： 
* soft nofile 65536
* hard nofile 131072

修改完成后可以使用ulimit查看即可。

注:
*  代表任何用户
soft 代表软件
nofile 最大文件
noproc 最大进程
65536 数量
```



### 2. linux如何查看系统负载,内存.硬盘使用情况

```
系统负载命令:uptime、w、top
[root@node242 ~]# uptime
 12:06:18 up 6 days, 12:51,  3 users,  load average: 0.00, 0.01, 0.05


[root@node242 ~]# w
 12:06:42 up 6 days, 12:51,  3 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    10.0.151.251     08:53    3:11m  4:38   0.03s -bash
root     pts/2    10.0.157.236     11:39   27:03   0.01s  0.01s -bash
root     pts/3    10.0.151.253     11:47    2.00s  0.05s  0.01s w

[root@node242 ~]# top
top - 12:07:29 up 6 days, 12:52,  3 users,  load average: 0.00, 0.01, 0.05
Tasks: 249 total,   1 running, 248 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.1 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem : 98824672 total, 73614424 free, 21389376 used,  3820868 buff/cache
KiB Swap:  4194300 total,  4194300 free,        0 used. 76497328 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 7706 root      20   0   10.4g   1.1g  15900 S   1.0  1.1  76:26.86 java
 5461 root      20   0 3129584 459292  19332 S   0.7  0.5  55:37.83 java
......

load average分别对应于过去1分钟，5分钟，15分钟的负载平均值。

内存命令:free、top
[root@node242 ~]# free
              total        used        free      shared  buff/cache   available
Mem:       98824668    21388812    73435172        9616     4000684    76496944
Swap:       4194300           0     4194300


磁盘命令:df
[root@node242 ~]# df
Filesystem               1K-blocks     Used  Available Use% Mounted on
/dev/mapper/centos-root   52403200  7870980   44532220  16% /
devtmpfs                  49398856        0   49398856   0% /dev

[root@node242 ~]#
[root@node242 ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   50G  7.6G   43G  16% /
devtmpfs                  48G     0   48G   0% /dev

[root@node242 ~]# df -h /home
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-home  1.8T   48G  1.8T   3% /home


查看io资源命令：iostat  (需要安装)
```



### 3. linux  >  和 >> 的区别,常用查看日志命令

```
> : 覆盖文件，如果文件没有将会创建，并且将内容写到文件中，可以用于删除数据重新写入数据场景。
>> : 在文件末尾追加，如果文件没有将会创建，用于原文件数据不能丢失场景。
```



### 4. 请列举几个常用的Linux命令

```
man :帮助命令

sudo : 获取root权限

jps : 查看java进程

ps -ef  : 查看进程

ps -aux : 查看进程

netstat -nltcp : 监听端口

free : 内存查看

top :动态 查看服务器资源信息

uptime : 查看系统运行时长和负载率

w : 查看系统运行时长和负载率

df : 查看磁盘情况

df -h /home : 查看home的使用情况

iostat :  查看i/o情况

tar :

zip :

unzip :

gzip:

gunzip:
rpm :

yum :

等等。
```



### 5. 用shell脚本怎么替换字符串

```
vi /home/test/rep.sh
#!/bin/bash

ori_str=$1
rep_str=$2
grep_word=$3
file_dir=$4
sed -i "s/${ori_str}/${rep_str}/g" `grep "$grep_word" -rl $4`  #数据源未目录或者文件即可

测试：
[root@hadoop01 test]# chmod a+x /home/test/rep.sh
[root@hadoop01 test]# ./rep.sh abc 123 abc /home/test/
```



### 6. shell脚本编程：将select max(id) from table的值赋值到一个变量中；

```
vi /home/test/my.sh
#!/bin/bash

mysql_var=`mysql --skip-column-names -uroot -proot -e "use test;select max(id) from stu"`
echo $mysql_var

测试:
chmod a+x /home/test/my.sh
/home/test/my.sh  运行脚本
```



### 7. shell脚本编程：将mysql数据库中五个库中的五张表合并到一张表中，表的结构都相同；

```
vi /home/test/mm.sh
#!/bin/bash

`mysql --skip-column-names -uroot -proot -e "insert into test.a1(id,name) select from ( select id,name from test1.a1 union all select id,name from test2.a1 union all select id,name from test3.a1 union all select id,name from test4.a1 union all select id,name from test5.a1)"`


测试:
chmod a+x /home/test/mm.sh
/home/test/mm.sh  运行脚本
```

