# Ubuntu服务器配置VNC远程桌面

------

> * ssh远程登陆服务
> * vnc客户端vncviewer的安装与使用
> * xfce4 + VNCServer远程桌面
> * VNCViewer远程桌面日常使用

<!--more-->
## ssh远程登陆服务
要实现VNC远程桌面控制，首先需要确定能否利用ssh远程命令登陆服务器，一条命令即可：
```bash
sudo apt-get install openssh-server
```
然后确认一下sshserver是否启动成功：
```bash
ps -e | grep ssh` 或者 `netstat -tlp
```
如果只有ssh-agent那ssh-server还没有启动，需要输入： `/etc/init.d/ssh start`
如果看到sshd那说明ssh-server已经启动了。
到此可以认为ssh已经安装好了，默认端口号为22，无需修改。

接下来确定ipv4地址，用于远程访问该服务器：`ifconfig`,记好ip(例如：`192.168.xxx.xxx`)，然后可以拔掉显示器和鼠标键盘了。

【以下为可选项】
为了让ssh远程登陆更快（在输入账户名后要等好久才出现输入密码的问题），可以输入如下命令：
```bash
sudo gedit /etc/ssh/sshd_config
```
找到 GSSAPI options 这一节，将下面两行注释掉：
```bash
# GSSAPIAuthentication yes
# GSSAPIDelegateCredentials no
```
然后重启ssh服务：
```bash
sudo /etc/init.d/ssh restart
```

## vnc客户端vncviewer的安装与使用
【vncviewer安装】以Windows系统为例，在Windows上把vncviewer软件安装一下，网上随便搜能够下载得到。

【xshell安装】在服务器主机上配好ssh服务后，在本地的电脑上需要安装命令行工具进行登陆，推荐使用xshell工具，或者putty工具。xshell软件的下载安装没什么可说的，网上随便搜一个下载安装就行。以xshell为例：
装好xshell后启动，输入ssh远程登陆命令：
```bash
ssh 192.168.xxx.xxx
```
在根据弹窗提示依次输出主机账户名和密码，即登陆成功。

这时候，只做到了用命令行远程登陆服务器，还不能实现直接远程桌面控制，需要在服务器上安装vncviewer对应的服务器端VNCServer以及用于可视化的桌面系统xfce4。

## xfce4 + VNCServer远程桌面
xfce4是一个轻量级的Ubuntu桌面环境，类似的还有gnome、unity等等。xfce4安装十分简单：
在xshell命令窗口ssh登陆后输入：
```bash
sudo apt-get install xfce4
```
然后安装VNCServer:
```bash
sudo apt-get install vnc4server
```
安装完成后第一次启动vncserver来设置vncserver的服务i密码，输入：
```bash
vncserver
```
如果是第一次安装vncserver，会提示设置vnc服务密码，按照提示要求走就行，记住密码。结束后将产生一个端口为`:1`的桌面，
并且在/home/username/.vnc/隐藏文件夹下产生xstartup配置文件。

然后需要在vncserver的配置文件xstaup中设置xfce4桌面环境配置：
```bash
sudo vi ~/.vnc/xstartup
```
这里进入了vi编辑器的界面，我们来修改配置内容。
按i键进入编辑输入状态，移动光标修改内容为如下：
```bash
#!/bin/sh
# Uncomment the following two lines for normal desktop:
# unset SESSION_MANAGER
# unset DBUS_SESSION_BUS_ADDRESS
# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
# x-window-manager &
x-session-manager & xfdesktop & xfce4-panel &
xfce4-menu-plugin &
xfsettingsd &
xfconfd &
xfwm4 &
```
改完后，按`Esc`退出编辑状态，然后输入`:wq!`强制保存并推出vi编辑器。

至此，vncserver配置结束。下面是vnc客户端VNCViewer的使用。

## VNCViewer远程桌面日常使用
日常使用VNCViewer进行远程桌面，首先打开xshell，在命令行窗口ssh登陆服务器，然后启动vncserver：
```bash
vncserver :5
```
或者：
```bash
vncserver -geometry 1920x1080 :5
```
将产生端口号为`:5`的桌面配置（输入后产生xxx:5.log记录，否则启动失败），端口号数字自己设定，但要保证小于100，且与同时使用该服务器的其他人不重复，否则失败，可多试几次。`-geometry`参数设定了远程桌面窗口的分辨率，注意1920与1080之间是字母x而非乘号*。
然后，利用VNCViewer开始远程桌面，启动VNCViewer，新建一个远程桌面（新版VNCViewer，旧版不用），编辑设置VNCServer地址，构成为`主机ip:VNC端口号`，如：
```bash
192.168.xxx.xxx:5
```
新版VNCViewer需要设置下主机账户名，双击开始连接；旧版不用，直接点connect连接。
然后输入VNC服务密码，即可连接成功。

【日常维护】
如果不再使用该VNC某个端口号（比如`:5`）了，建议杀掉,在xshell中ssh登陆服务器后，输入：
```bash
vncserver -kill :5
```

