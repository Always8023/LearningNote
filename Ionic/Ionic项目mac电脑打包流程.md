## Ionic项目mac电脑打包流程：
### 01.更新/安装node（自带npm）
### 02.将npm的源设置为淘宝的源
$ npm config set registry https://registry.npm.taobao.org --global  
$ npm config set disturl https://npm.taobao.org/dist --global
### 03.npm安装Ionic CLI和Cordova
$ npm install -g ionic cordova
### 04.打开Ionic项目文件夹
### 05.安装第三方包
$ npm install
### 06.添加ios平台
$ ionic cordova platform add ios
### 07.打包生成App
$ ionic cordova build ios --prod --release
### 08.发布
