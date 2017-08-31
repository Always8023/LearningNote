## APT
普通安装：apt-get install softname1 softname2...  
重新安装：apt-get --reinstall install softname1 softname2...  
移除卸载：apt-get remove softname1 softname2...（移除软件包，当包尾部有+时，意为安装）  
清除卸载：apt-get remove --purge softname1 softname2...（同时清除配置）  
清除卸载：apt-get purge sofname1 softname2...（同上，也清除配置文件）  
更新APT源：apt-get update  
更新已安装的软件：apt-get upgrade
升级系统：apt-get dist-upgrade 

apt-cache search # ------(package 搜索包)  
apt-cache show #------(package 获取包的相关信息，如说明、大小、版本等)  
apt-get -f install # -----(强制安装, "-f = --fix-missing"当是修复安装吧...)  
apt-get autoremove --purge # ----(package 删除包及其依赖的软件包+配置文件等（只对6.10有效，强烈推荐）)    
apt-get dselect-upgrade #------使用 dselect 升级  
apt-cache depends #-------(package 了解使用依赖)  
apt-cache rdepends # ------(package 了解某个具体的依赖,当是查看该包被哪些包依赖吧...)  
apt-get build-dep # ------(package 安装相关的编译环境)   
apt-get source #------(package 下载该包的源代码)  
apt-get clean && apt-get autoclean # -----清理下载文件的存档 && 只清理过时的包   
apt-get check #-------检查是否有损坏的依赖  
apt-file search filename -----查找filename属于哪个软件包  
apt-file list packagename -----列出软件包的内容   
apt-file update --更新apt-file的数据库  
dpkg -S filename -----查找filename属于哪个软件包  
dpkg --info "软件包名" --列出软件包解包后的包名称  
dpkg -l --列出当前系统中所有的包，可以和参数less一起使用在分屏查看   
dpkg -s 查询已安装的包的详细信息  
dpkg -L 查询系统中已安装的软件包所安装的位  
dpkg -S 查询系统中某个文件属于哪个软件包  
dpkg -I 查询deb包的详细信息,在一个软件包下载到本地之后看看用不用安装(看一下呗)  
dpkg -i 手动安装软件包(这个命令并不能解决软件包之前的依赖性问题)  
dpkg -r 卸载软件包.不是完全的卸载,它的配置文件还存在  
dpkg -P 全部卸载(但是还是不能解决软件包的依赖性的问题)  
dpkg -reconfigure 重新配置  

## SSH
安装  
apt-get install ssh  
初始化密钥  
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa

## 网络设置
修改/etc/network/interfaces，格式：  
auto eth0  
face eth0 inet static  
address 192.168.222.111  
gateway 192.168.222.1  
netmask 255.255.255.0  
dns-nameservers 221.228.255.1 61.177.7.1  

重启网络服务
sudo /etc/init.d/networking restart

## JRE
下载  
wget jdk.tar.gz  
解压缩  
tar zxvf jdk.tar.gz  
复制到/usr/文件夹  
mv jdk/ /usr/  
添加环境变量，修改/etc/environment  
JAVA_HOME=/usr/jdk  
PATH="* * * *:/usr/jdk/bin"  
重置环境变量  
source environment  
验证安装  
java -version