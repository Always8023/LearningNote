## 编译Android应用（这里以安卓应用为例）
### 01.配置应用的签名。使用如下命令来配置你的签名（keystore）：
$ keytool -genkey -v -keystore com.hanwintech.app.dyh.keystore -alias com.hanwintech.app.dyh -keyalg RSA -keysize 2048 -validity 10000  
注意：请使用你应用的名字来替代com.hanwintech.app.dyh  
这个命令可以生成一个新文件，在本示例中为com.hanwintech.app.dyh.keystore。  
在应用的整个生命周期中将重复使用同一个keystore，请保存好它。
### 02.使用Cordova编译应用。使用build命令编译一个应用的发布版本：
$ cordova build --release --prod android  
这个命令会生成一个新的apk文件。此时还未签名。
### 03.签名应用文件。现在我们要用之前创建的keystore文件来签名生成的未签名版本的应用。使用如下命令来签名：
$ jarsigner -verbose -keystore com.hanwintech.app.dyh.keystore -signedjar com.hanwintech.app.dyh.apk D:/Android/projects/DYH_Hybrid/platforms/android/build/outputs/apk/android-release-unsigned.apk com.hanwintech.app.dyh  
注意：这里请使用keystore生成文件名来替换com.hanwintech.app.dyh.keystore示例名称，
同时用真实的应用文件名替换掉unsigned_name.apk。
这个过程需要一点时间，期间会提示输入keystore的密码。命令会修改apk文件并对其进行签名。
### 04.apk已经生成，将它发到你的手机上就可以下载安装了，如果要将它发布到应用商店，那就去应用商店注册开发者账户吧！ 
