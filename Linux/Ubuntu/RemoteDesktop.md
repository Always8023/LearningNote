## 1、设置Ubuntu 16.04系统允许远程控制
在 Dash 中打开“桌面共享”，设置“允许控制桌面”和“要求输入此密码”  

## 2、运行dconf-editor，把加密选项去掉
$ sudo apt-get install dconf-editor  
$ dconf-editor  
依次展开org->gnome->desktop->remote-access  
这里也可以直接设置远程控制选项，但重要的是将“requre-encryption”去掉。
