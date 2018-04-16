# android so + windows7 + IDA pro 6.8 动态调试--第一篇 环境搭建
## 1. 环境准备（第一次需要）

```
	
	//推送 android_server（IDA的dbgsrv目录下）到目标目录。
	adb push xxx:\android-server  /data/local/tmp/android_servier
 	
	adb shell
	
	su
	
	cd /data/local/tmp

	chmod 777 android_server
	

```

## 2. 调试步骤
### 2.1 root权限运行android_server, android_server正在监听23496端口
	
    Microsoft Windows [版本 6.1.7601]
	版权所有 (c) 2009 Microsoft Corporation。保留所有权利。
	
	D:\Programs\android-tools\IDA_Pro_v6.8\dbgsrv>adb shell
	shell@hammerhead:/ $ su
	root@hammerhead:/ # chmod 777 /data/local/tmp/android_server
	root@hammerhead:/ # /data/local/tmp/android_server
	IDA Android 32-bit remote debug server(ST) v1.19. Hex-Rays (c) 2004-2015
	Listening on port #23946...

**请注意这里的IDA Android 32-bit，后面打开ida的时候也要用32位，否则会在attach步骤看不到要debug的app。**

### 2.2 另开一个cmd，执行adb forward命令。
> adb forword 是执行转发命令，是把PC端xxx端口的数据, 转发到Android端的某个端口上.该命令主要用于实现pc和App的socket通讯。

    Microsoft Windows [版本 6.1.7601]
	版权所有 (c) 2009 Microsoft Corporation。保留所有权利。
	
	F:\master>adb forward tcp:23946 tcp:23946
	
	F:\master>



### 2.3 调试模式启动App  adb shell am start -D -n 包名/类名


    
	F:\master>adb shell am start -D -n com.yaotong.crackme/com.yaotong.crackme.MainActivity
	Starting: Intent { cmp=com.yaotong.crackme/.MainActivity }
	
	F:\master>

这个时候，App启动后，处于 waiting for Debugger状态, 等待其他的进程attch


### 2.4 祭出IDA大法, attch到目标进程。

依次点击菜单栏：Debugger-->attach --> Remote ArmLinux/Android Debugger，显示如图：

![](http://opbly0y7j.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180416163447.png)

其中Hostname输入：localhost, Port默认端口是23946，不用改变。点击Ok。

![](http://opbly0y7j.bkt.clouddn.com/wx041601.png)

这时候会弹出进程列表，选择要调试的进程，点击OK。

说明：如果没有弹出进程列表，而是弹出其他的错误，请按照如下步骤检查：

- android_server没有启动
- android_server没有在root权限下启动
- 没有执行adb forward命令
- ida启动的不是32位程序
- 端口输入错误或者端口不一致  

### 2.5 设置Debugger Options  
> Debugger--> Debugger Options 
 
![](http://opbly0y7j.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180416165340.png)

这三个打钩。


### 2.5 jdb调试的Attach

    
	F:\master>adb shell
	shell@hammerhead:/ $ ps | grep com.yaotong.crackme
	u0_a86    18652 279   1490888 37988 ffffffff 00000000 t com.yaotong.crackme
	
	127|shell@hammerhead:/ $ exit
	
	F:\master>adb forward tcp:8700 jdwp:18652 //进程号
	
	F:\master>jdb -connect com.sun.jdi.SocketAttach:hostname=localhost,port=8700
	设置未捕获的java.lang.Throwable
	设置延迟的未捕获的java.lang.Throwable
	正在初始化jdb...
	>

先通过ps命令查看进程号，

然后执行 adb forward tcp:8700 jdwp:进程号 

再执行 jdb -connect com.sun.jdi.SocketAttach:hostname=localhost,port=8700


### 2.6 点击IDA的运行按钮，就会断住进程了。
![](http://opbly0y7j.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180416170453.png)




## 3. 参考资料
[adb forward实现PC和App的Socket通讯](https://www.jianshu.com/p/fee5b31774be "adb forward实现PC和App的Socket通讯")

