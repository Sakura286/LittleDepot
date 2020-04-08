## 前言

2020.4.7 尽量通俗、详细地，对像我自己这样的小白阐释一下我这次的刷机过程

## 背景

**机型**：小米8 (dipper)

**操作系统**：Windows 10 1909 专业版

## 需要的文件

先别急着下，等看到后看如果自己需要的话再回来拿

1. [ROM: xiaomi.eu_multi_MI8_20.3.26_v11-10](https://xiaomi.eu/community/threads/miui-11-0-stable-release.52628/)
2. [Adb与Fastboot](https://dl.google.com/android/repository/platform-tools-latest-windows.zip)
3. [TWRP for dipper v3.3.1.1](https://dl.twrp.me/dipper/)
3. [Universal Adb Driver](https://github.com/koush/UniversalAdbDriver)
3. [Magisk v20.4](https://github.com/topjohnwu/Magisk/releases)
4. [Riru v19.8](https://github.com/RikkaApps/Riru/releases)
5. [EdXposed YAHFA v0.4.6.2](https://github.com/ElderDrivers/EdXposed/releases)

## 目录

1. 安装ADB与FASTBOOT
2. 刷入第三方Recovery
3. 刷入并安装ROM
4. 完成谷歌验证
5. 刷入Magisk
6. 安装Xposed框架

## 正文

### 1 安装ADB与FASTBOOT

> 本节绝大部分翻译自[LineageOS的wiki页面](https://wiki.lineageos.org/adb_fastboot_guide.html)，主要内容是下载ADB并将其添加进环境变量，完成后再安装通用驱动

1. 从谷歌[下载zip包](https://dl.google.com/android/repository/platform-tools-latest-windows.zip)

2. 将zip包解压到一个目录下，**为举例说明**，这里使用`c:\install`目录

3. 将该目录底下的`platform-tools`文件夹添加进环境变量

	1. 在桌面的“此电脑”上右键，点击“属性”
	
	2. 在“系统“窗口里，点击左侧的“高级系统设置”
	
	5. 在“系统属性”窗口里，点击下方的“环境变量”
	
	6. 在“环境变量”窗口里，双击系统变量里的“Path”变量
	
	7. 在新弹出的“编辑环境变量”里，点击“新建”
	
	8. 输入`c:\install\platform-tools`，确定，完成
	
9. 安装[universal adb driver](https://github.com/koush/UniversalAdbDriver)

10. **重启电脑**

### 2 刷入第三方Recovery

> 本节绝大部分内容翻译自[LineageOS的wiki页面](https://wiki.lineageos.org/devices/dipper/install)，主要内容是进入fastboot模式刷入第三方Recovery - TWRP，在此之前请一定**[确保手机的BootLoader已经解锁](http://www.miui.com/unlock/index.html)**

1. [下载TWRP](https://dl.twrp.me/dipper/)，下载后文件的命名应该是这种格式`twrp-x.x.x-x-dipper.img`，此处使用了`twrp-3.3.1-1-dipper.img`

2. 通过USB数据线把手机连到电脑上

3. 手机关机后，同时按住`下音量键`+`电源键`直到屏幕上出现小米的Logo后松开，之后屏幕出现“FASTBOOT”字样，进入fastboot模式

4. 在**img文件所在的文件夹**[打开命令提示符](https://jingyan.baidu.com/article/2a13832898b997074a134f27.html)，输入命令

```cmd
fastboot flash recovery <recovery_filename>.img
```

	> 注意`<recovery_filename>.img`只是示例用的文件名，具体的文件名还需要自己进行修改

5. 按住`上音量键`+`电源键`直到屏幕上再次出现小米Logo后松开，屏幕出现“TEAMWIN”字样，进入Recovery模式

### 3 在Recovery里安装ROM

> 本节主要内容是[对手机进行三清](https://zhuanlan.zhihu.com/p/93368864)后，将PC端的ROM文件发送到手机上，在手机上进行卡刷

1. 去[MIUI的欧洲论坛](https://xiaomi.eu/community)上下载对应ROM，**一定要[区分好自己手机型号](https://c.mi.com/thread-1911812-1-0.html)**，例如举例使用的小米8，多见`dipper`及`MI8`等代号，此处使用了`xiaomi.eu_multi_MI8_20.3.26_v11-10`

2. 确保已经进入了Recovery模式

	> 可以先在“Setting”-"语言"里设置好语言

3. 点击“清除”-“格式化Data分区”，完成格式化过程

4. 点击“高级清除”，选择“Cache”，“System”与“Dalvik Cache”

5. 在**ROM所在的文件夹**打开命令提示符，输入命令

```cmd
adb push <filename>.zip /sdcard/
```

	> 同刷Recovery时的操作，`<filename>.zip`需要自己进行修改

6. 上面的命令执行完毕后，回到主菜单，点击“安装”，选择刚刚导入的zip文件

7. 待执行完毕后，重启手机，进入系统

### 4 完成谷歌验证

> 本节的主要内容是通过设置局域网代理，让手机连接wifi时便可以在电脑端梯子的协助下访问谷歌，完成谷歌验证进入系统；如果身在外网环境，或者路由器翻墙已设置好请无视此节

1. PC端设置，请在设置开始之前**确保PC端已有一把可以访问谷歌的梯子**，并且手机和电脑在同一个局域网相连
(简单说手机和电脑连在同一个路由器上)，这里以[V2rayN v3.4](https://github.com/2dust/v2rayN/releases)为例

	1. 打开v2rayN主界面，点击“参数设置”；
	
	2. 在“参数设置”窗口，切到“v2rayN设置”选项卡，**勾选“允许来自局域网的链接”**，确定
	
	3. 在v2rayN主界面最下方的状态栏里，获得本地HTTP端口号`10809`（默认设置，具体情况可能有变化）
	
	4. 打开命令提示符，输入命令`ipconfig`，记录下本机的ipv4地址
	
2. 手机端设置

	1. 连接所需wifi
	
	2. 待连接完成后，点击右方的右箭头，进入wifi高级设置页面
	
	3. “代理”项选择“手动”，“主机名”填入刚才获得的ipv4地址，“端口号”填入之前步骤获得的端口号，确定

	> 注：卡验证的原因是欧洲版ROM使用Google服务，然而新装的手机没有梯子无法访问谷歌。设置局域网代理的原理是，通过给wifi设置代理，可以把手机端发送接收的信息的任务交给PC的一个特定端口；这时给梯子打开允许局域网连接的功能，并让梯子监听这个端口，那么就建立起了手机  --> 路由器 --> PC --> 梯子 --> 外网的信息传输链条

### 5 刷入Magisk

> 本节的主要内容是刷入Magisk并检验系统是否root，具体安装方法同第3节的操作

1. [下载Magisk](https://github.com/topjohnwu/Magisk/releases)，此处使用了Magisk v20.4版本

	> 在Recovery里安装Magisk后会自动在系统里安装Magisk Manager，所以无需下载Magisk Manager.apk

2. 在下载magisk的文件夹处打开命令提示符，输入命令

```cmd
adb push <magisk_filename>.zip /sdcard/
```

	> 同刷Recovery时的操作，`<magisk_filename>.zip`需要自己进行修改

3. 重启手机，进入系统，会新出现一个名为“Magisk Manager”的APP

4. 去Play商店下载Root Checker，运行后验证系统是否已经root成功

### 安装Xposed框架

> 本节的主要内容是通过Magisk安装Riru与EdXposed模块

1. 下载[Riru](https://github.com/RikkaApps/Riru/releases)与[EdXposed YAHFA](https://github.com/ElderDrivers/EdXposed)到手机内置存储，此处使用了Riru v19.8与EdXposed YAHFA v0.4.6.2

2. 打开Magisk Manager，点击左侧菜单，选择“模块”

3. 点击“+”号，在文件管理里找到下载好的Riru与EdXposed的zip压缩包，依次安装

4. 下载并安装[EdXposed Manager](https://github.com/ElderDrivers/EdXposedManager/releases)，此处使用了EdXposed Manager v4.5.7

5. 重启手机

	> - EdXposed是可用于安卓8~安卓10的Xposed框架，需要Riru模块的支持，而Riru模块的安装又需要用到Magisk
	> - 网传的EdXposed Installer[已经不适用于新版本的EdXposed]
(https://github.com/ElderDrivers/EdXposed#companion-applications)，请直接安装EdXposed Manager
