# 以太网接口
以太网接口由系统使用ethX的命名约定标识，其中X表示数值。 第一个以太网接口通常标识为eth0，第二个为eth1，以此类推。

### 识别以太网接口
要快速识别所有可用的以太网接口，可以使用ifconfig命令：  
ifconfig -a | grep eth  

另一个可以帮助识别网络接口的命令是lshw。 在下面的示例中，lshw显示了一个逻辑名称为eth0的单个以太网接口以及总线信息，驱动程序详细信息和所有支持的功能：  
sudo lshw -class network  

### 以太网接口的逻辑名称
接口逻辑名称配置在文件/etc/udev/rules.d/70-persistent-net.rules中。 如果要控制哪个接口特定的逻辑名称，请找到匹配接口物理MAC地址的行，并将NAME=ethX的值修改为所需的逻辑名称，然后重新启动系统以提交更改。

### 以太网接口设置
ethtool是一种显示和更改以太网卡设置（如自动协商，端口速度，双工模式和LAN唤醒）的程序。 默认情况下不安装，但可以在存储库中进行安装：  
sudo apt install ethtool

以下是如何查看以太网接口的支持功能和配置设置的示例：  
sudo ethtool eth0

使用ethtool命令进行的更改是临时的，重新启动后将丢失。 如果要保留设置，只需在接口配置文件/etc/network/interfaces中将所需的ethtool命令添加到pre-up语句。

以下是将eth0接口配置为全双工模式运行、端口速度为1000Mb/s的示例：   
auto eth0  
iface eth0 inet static  
pre-up /sbin/ethtool -s eth0 speed 1000 duplex full  

# IP寻址
### 临时IP地址分配
对于临时网络配置，您可以使用诸如ip，ifconfig和route之类的标准命令，这些命令也可以在大多数其他GNU/Linux操作系统上找到。这些命令允许您配置立即生效的设置，但它们不是永久的，重新启动系统后设置将丢失。

要临时配置IP地址，可以按照以下方式使用ifconfig命令，只需修改IP地址和子网掩码，以满足您的网络需求：  
sudo ifconfig eth0 10.0.0.100 netmask 255.255.255.0

要验证eth0的IP地址配置，可以按照以下方式使用ifconfig命令：  
ifconfig eth0

要配置默认网关，可以按照以下方式使用route命令。 修改默认网关地址以匹配您的网络需求：  
sudo route add default gw 10.0.0.1 eth0

要验证您的默认网关配置，可以按照以下方式使用route命令：  
route -n

如果您需要DNS进行临时网络配置，则可以在/etc/resolv.conf文件中添加DNS服务器IP地址。 一般来说，直接编辑/etc/resolv.conf是不推荐的，这是一个临时和非持久性的配置。下面的示例显示了如何将两个DNS服务器输入到/etc/resolv.conf中，这些服务器应该更改为适合您网络的服务器：  
nameserver 8.8.8.8  
nameserver 8.8.4.4

如果您不再需要此配置，并希望从接口清除所有IP配置，则可以使用ip命令带上flush选项：  
ip addr flush eth0

### 动态IP地址分配（DHCP）
要将服务器配置为使用DHCP进行动态地址分配，请将dhcp方法添加到文件/etc/network/interfaces中相应接口的inet地址族语句中：  
auto eth0  
iface eth0 inet dhcp

配置完成后，可以通过ifup命令手动启动接口，该命令通过dhclient启动DHCP进程：  
sudo ifup eth0

### 静态IP地址分配
要将系统配置为使用静态IP地址，请将静态方法添加到文件/etc/network/interfaces中相应接口的inet地址族语句，方法需要设置地址、网络掩码和网关：  
auto eth0  
iface eth0 inet static  
address 10.0.0.100  
netmask 255.255.255.0  
gateway 10.0.0.1  

配置完成后，可以通过ifup命令手动启动接口：  
sudo ifup eth0

要手动禁用该接口，可以使用ifdown命令：  
sudo ifdown eth0

### 环回接口
环回接口由系统标识为lo，默认IP地址为127.0.0.1。 可以使用ifconfig命令查看：  
ifconfig lo

默认情况下，/etc/network/interfaces中应该有两行负责自动配置您的环回接口。建议您保留默认设置，除非您有什么特别目的。两个默认行的示例如下所示：  
auto lo  
iface lo inet loopback

# 名称解析
### DNS客户端配置
传统上，/etc/resolv.conf是一个静态配置文件，很少需要通过DCHP客户机更改。如今，计算机可以经常从一个网络切换到另一个网络，并且resolvconf框架现在正用于跟踪这些更改并自动更新解析器的配置。它作为提供域名服务器信息的程序和需要名称服务器信息的应用程序之间的中介。Resolvconf通过与网络接口配置相关的一组挂钩脚本填充信息。用户最显着的区别是，对/etc/resolv.conf手动完成的任何更改都将丢失，因为每次触发resolvconf时，之前的配置都会被覆盖。相反，resolvconf使用DHCP客户端挂钩和/etc/network/interfaces来生成名称服务器和域的列表，放入/etc/resolv.conf文件中，该文件现在是一个符号链接：  
/etc/resolv.conf -> ../run/resolvconf/resolv.conf

如果想要配置域名解析，请在文件/etc/network/interfaces中添加适用于您网络的DNS服务器的IP地址。您还可以添加可选的DNS后缀以匹配您的网络域名。对于每个其他有效的resolv.conf配置选项，您可以在文件对应的节中包含以dns-开头的同名选项。例如：  
iface eth0 inet static  
    address 192.168.3.3  
    netmask 255.255.255.0  
    gateway 192.168.3.1  
    dns-search example.com  
    dns-nameservers 192.168.3.45 192.168.8.10

搜索选项也可以同时使用多个域名，以便按照输入的顺序追加DNS查询。例如，您的网络可能有多个子域进行搜索; 父域example.com，以及两个子域sales.example.com和dev.example.com，则配置为：  
dns-search example.com sales.example.com dev.example.com

如果您尝试使用server1的名称ping主机，系统将按以下顺序自动查询DNS的完全限定域名（FQDN）：  
server1.example.com --> server1.sales.example.com --> server1.dev.example.com

### 静态主机名
静态主机名是本地定义的文件/etc/hosts中的主机名到IP映射。默认情况下，hosts文件中的条目将优先于DNS。这意味着如果您的系统尝试解析主机名，并且它匹配/ etc/hosts中的条目，则不会尝试在DNS中查找记录。在某些配置中，特别是当不需要Internet访问时，与有限数量的资源通信的服务器可以方便地设置为使用静态主机名而不用DNS。

以下是主机文件的示例，其中通过简单的主机名，别名及其等效的完全限定域名（FQDN）识别了多个本地服务器：  
127.0.0.1	localhost  
127.0.1.1	ubuntu-server  
10.0.0.11	server1 server1.example.com vpn  
10.0.0.12	server2 server2.example.com mail  
10.0.0.13	server3 server3.example.com www  
10.0.0.14	server4 server4.example.com file
