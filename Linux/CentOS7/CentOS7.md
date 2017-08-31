## YUM
安装程序：yum install softname1 softname2...  
安装程序组：	yum groupinsall group1 group2...  
显示所有已经安装和可以安装的程序包：yum list   
删除程序：yum remove softname1 softname2...  
清除缓存目录：yum clean all  
显示所有可安装的程序组：yum grouplist    

## SSH
安装  
yum install ssh  
初始化密钥  
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa

## 网络设置


重启网络服务


## JRE
下载  
wget jdk.rpm  
安装  
rpm -ivh jdk.rpm   
添加环境变量，在/etc/profile文件末尾加上这些：
export JAVA_HOME=/usr/java/jdk1.8.0_*
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
最后加载一下
source /etc/profile  
验证安装  
java -version