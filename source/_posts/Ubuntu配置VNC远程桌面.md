# Ubuntu����������VNCԶ������

------

## sshԶ�̵�½����
Ҫʵ��VNCԶ��������ƣ�������Ҫȷ���ܷ�����sshԶ�������½��������һ������ɣ�
```sudo apt-get install openssh-server```
Ȼ��ȷ��һ��sshserver�Ƿ������ɹ���
```ps -e | grep ssh` ���� `netstat -tlp```
���ֻ��ssh-agent��ssh-server��û����������Ҫ���룺 ```/etc/init.d/ssh start```
�������sshd��˵��ssh-server�Ѿ������ˡ�
���˿�����Ϊssh�Ѿ���װ���ˣ�Ĭ�϶˿ں�Ϊ22�������޸ġ�

������ȷ��ipv4��ַ������Զ�̷��ʸ÷�������```ifconfig```,�Ǻ�ip(���磺```192.168.xxx.xxx```)��Ȼ����԰ε���ʾ�����������ˡ�

������Ϊ��ѡ�
Ϊ����sshԶ�̵�½���죨�������˻�����Ҫ�Ⱥþòų���������������⣩�����������������
```sudo gedit /etc/ssh/sshd_config```
�ҵ� GSSAPI options ��һ�ڣ�����������ע�͵���
```# GSSAPIAuthentication yes```
```# GSSAPIDelegateCredentials no```
Ȼ������ssh����
```sudo /etc/init.d/ssh restart```

## vnc�ͻ���vncviewer�İ�װ��ʹ��
��vncviewer��װ����WindowsϵͳΪ������Windows�ϰ�vncviewer�����װһ�£�����������ܹ����صõ���

��xshell��װ���ڷ��������������ssh������ڱ��صĵ�������Ҫ��װ�����й��߽��е�½���Ƽ�ʹ��xshell���ߣ�����putty���ߡ�xshell��������ذ�װûʲô��˵�ģ����������һ�����ذ�װ���С���xshellΪ����
װ��xshell������������sshԶ�̵�½���
```ssh 192.168.xxx.xxx```
�ڸ��ݵ�����ʾ������������˻��������룬����½�ɹ���

��ʱ��ֻ��������������Զ�̵�½��������������ʵ��ֱ��Զ��������ƣ���Ҫ�ڷ������ϰ�װvncviewer��Ӧ�ķ�������VNCServer�Լ����ڿ��ӻ�������ϵͳxfce4��

## xfce4 + VNCServerԶ������
xfce4��һ����������Ubuntu���滷�������ƵĻ���gnome��unity�ȵȡ�xfce4��װʮ�ּ򵥣�
��xshell�����ssh��½�����룺
```sudo apt-get install xfce4```
Ȼ��װVNCServer:
```sudo apt-get install vnc4server```
��װ��ɺ��һ������vncserver������vncserver�ķ���i���룬���룺
```vncserver```
����ǵ�һ�ΰ�װvncserver������ʾ����vnc�������룬������ʾҪ���߾��У���ס���롣�����󽫲���һ���˿�Ϊ`:1`�����棬
������/home/username/.vnc/�����ļ����²���xstartup�����ļ���

Ȼ����Ҫ��vncserver�������ļ�xstaup������xfce4���滷�����ã�
```sudo vi ~/.vnc/xstartup```
���������vi�༭���Ľ��棬�������޸��������ݡ�
��i������༭����״̬���ƶ�����޸�����Ϊ���£�
```#!/bin/sh
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
����󣬰�```Esc```�˳��༭״̬��Ȼ������```:wq!```ǿ�Ʊ��沢�Ƴ�vi�༭����

���ˣ�vncserver���ý�����������vnc�ͻ���VNCViewer��ʹ�á�

## VNCViewerԶ�������ճ�ʹ��
�ճ�ʹ��VNCViewer����Զ�����棬���ȴ�xshell���������д���ssh��½��������Ȼ������vncserver��
```vncserver :5```
���ߣ�
```vncserver -geometry 1920x1080 :5```
�������˿ں�Ϊ```:5```���������ã���������xxx:5.log��¼����������ʧ�ܣ����˿ں������Լ��趨����Ҫ��֤С��100������ͬʱʹ�ø÷������������˲��ظ�������ʧ�ܣ��ɶ��Լ��Ρ�`-geometry`�����趨��Զ�����洰�ڵķֱ��ʣ�ע��1920��1080֮������ĸx���ǳ˺�*��
Ȼ������VNCViewer��ʼԶ�����棬����VNCViewer���½�һ��Զ�����棨�°�VNCViewer���ɰ治�ã����༭����VNCServer��ַ������Ϊ```����ip:VNC�˿ں�```���磺
```192.168.xxx.xxx:5```
�°�VNCViewer��Ҫ�����������˻�����˫����ʼ���ӣ��ɰ治�ã�ֱ�ӵ�connect���ӡ�
Ȼ������VNC�������룬�������ӳɹ���

���ճ�ά����
�������ʹ�ø�VNCĳ���˿ںţ�����```:5```���ˣ�����ɱ��,��xshell��ssh��½�����������룺
```vncserver -kill :5```

