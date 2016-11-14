#smart-pi
基于树莓派3的智能设备
 
### 2016.10.17 23:42
要给自己找点事情做，手上有一块闲置了很久的树莓派3，可以用它来做些什么有趣的东西。
初步打算做一个语音识别，然后在加上一些乱七八糟的东西上去。
关于语言的选用：java可能会支持的库比较多，python不熟悉，不过感觉python会是最佳的选择。


#打包相关文档

@Author : 孟雷 MengLei

* change log：
    * `2016.11.14 20:17`
        * 增加patch更新方法描述
    * `2016.10.31 10:07`
        * issues : "imageConverter.exe意外停止" 已经修复
        * 增加描述
        * 增加 "验证和替换导出的游戏资源"
        * 增加验证美术资源的步骤
    * `2016.8.23 10:50`
        * 添加此文件
        * 一键打包资源脚本，增加使用软件“好压”命令行的方式来压缩。(需要安装好压并修改脚本)
        * 一键打包资源脚本，增加将打包并压缩好的资源自动上传到192.168.1.150服务器中。

------------------------
**导出android/ios资源步骤：**

1. 更新到需要的版本
1. 运行artres/datatools/目录下的load_cfg.bat
1. 运行artres/目录下，"导出游戏资源.bat(android)" / "导出游戏资源-IOS.bat(ios)"。此步骤会生成artres/distribute目录
1. 运行artres/目录下，"build_kpk_android.bat" / "build_kpk_ios.bat"，会生成artres/packages目录。此目录下包含几个*.kpk文件，用来打新包使用。
1. 运行artres/目录下，"build_webshare_android.bat" / "build_webshare_ios.bat"，会生成artres/webshare-android(webshare-ios)目录。此目录下的文件用来做热更新使用。
1. 运行artres/目录下，"build_webshare_android.bat" / "build_webshare_ios.bat"，会生成artres/webshare-android(webshare-ios)目录。此目录下的文件用来做热更新使用。

`注:`
* 在artres/目录下增加了两个"一键打包android/ios资源.bat"脚本，在运行该脚本之前只需要更新到所需要的版本就可以
* 打包磁盘所需空间需要在2GB以上。
* 一键打包脚本中，增加了输出当前svn版本信息到文件，需要安装svn的命令行程序

**导出android安装包**

1. 进入到android工程目录，这里以sanguo_longtu_apk为例，后面简称"sanguo"
1. 更新sanguo工程，并使用最新的so文件。(在sanguo/libs/armeabi-v7a/目录下)
1. 将上面步骤导出的kpk和version.txt文件拷贝到sanguo/assets/packages/目录下
1. 修改配置sanguo/AndroidManifest.xml文件
1. 使用eclipse导出apk安装包。

`注:`
* 修改AndroidManifest.xml，可能会修改整个工程的包名，这时候修改出错的java文件，引用R.java的包名即可。

**替换导出的更新资源**

    - 当做基于某个已经导出过的版本做更新时，若已经打好了distribute，
    而这时候突然有一个脚本需要更新，可以使用如下的操作来减少更新的时间

1. 拷贝需要替换的单个lua脚本到distribute/目录下的对应文件夹
1. 运行build_kpk 或者build_webshare 来导出对应android/ios的游戏资源（此步骤一般是做更新）

**验证导出的游戏资源**

    - 验证导出的webshare文件的真名

主干版本中，增加了`build_unpack_kpk.bat`和`build_webshare_check.bat`两个脚本，前者是解压kpk，后者是对webshare文件夹下的单个文件做真名校验。
针对后者，需要以文本编辑器打开改脚本，修改需要校验的文件名和目录，双击运行即可找到对应的文件真名。

`注：`
* 注意文件的目录结构
* 若有点文件校验出来没有找到对应的真文件名字，那么这个文件可能是version.txt中kpk对应的一个文件。



**验证美术资源与策划配置**

1. 更新到版本，运行`datatools/loadcfg.bat`
1. 运行`导出游戏资源.bat`，运行该文件会输出mmrescpy.log，若美术资源（一般是动作或者特效）有问题（可能是策划配置的问题），则会在开头保存，并输出错误文件。
1. 以"Copy file failed(2)"开头，表示正常的过程，中途如果有"!!!"开头的error并打包脚本暂停了，那就表示有问题了。


**使用patch更新**

    - patch更新与webshare更新方法不冲突，在有patch的时候，优先使用patch更新
    使用此方法做更新，需要保留上一个版本的webshare（备份），并且打出最新的webshare
    
1. 备份上一个版本的webshare（脚本自动备份）
2. 导出最新的webshare（参考上面的过程）
3. 修改`build_patch_update_android/ios.bat`脚本，对应旧版本的webshare和新版本webshare目录
4. 运行上述脚本，会生成`patch-android/ios`目录
5. 将目录下的内容压缩并上传到服务器，同时该版本最新的webshare也需要上传。
6. 当手机更新的时候，如果处于这两个版本中间，就会优先使用patch更新。


----------------------
`issues:`

    * 暂时无

`附录`

* 使用软件好压的命令行参考文档： http://haozip.2345.com/help/help11-1.htm
