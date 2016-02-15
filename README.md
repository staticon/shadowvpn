Linux服务端

使用如下命令依次每行输入进行安装，碰到需要输入[y/N]的提示时，输入y回车：

echo "deb http://shadowvpn.org/debian wheezy main" > /etc/apt/sources.list.d/shadowvpn.list
apt-get update
apt-get install shadowvpn
service shadowvpn restart

Windows客户端

编译Windows客户端 在VPS终端依次逐行执行下面命令进行windows客户端编译，碰到需要输入[y/N]的提示时，输入y回车。
apt-get install build-essential mingw-w64
cd /root
wget https://github.com/clowwindy/ShadowVPN/releases/download/0.1.6/shadowvpn-0.1.6.tar.gz
tar xzvf shadowvpn-0.1.6.tar.gz
cd shadowvpn-0.1.6/
./configure --host=i686-w64-mingw32 --enable-static
make
编译完成后的程序在 /root/shadowvpn-0.1.6/src/shadowvpn.exe 

安装TUN/TAP 驱动
在VPS终端执行下面命令，下载 TUN/TAP 驱动：

cd /root
wget http://build.openvpn.net/downloads/releases/tap-windows-9.9.2_3.exe
wget http://build.openvpn.net/downloads/releases/tap-windows-9.21.0.exe
下载的驱动在 /root 目录， 32 位windows系统请从 VPS 下载 tap-windows-9.9.2_3.exe 并安装， 64位系统请下载 tap-windows-9.21.0.exe 并安装。

安装后去网络和共享中心的适配器设置，找到刚刚安装的TAP-Windows Adapter虚拟网卡，重命名为 ShadowVPN

配置VPN客户端 在VPS终端执行下面命令，下载配置文件
cd /root
wget https://raw.githubusercontent.com/clowwindy/ShadowVPN/master/samples/windows/client.conf
wget https://raw.githubusercontent.com/clowwindy/ShadowVPN/master/samples/windows/client_up.bat
wget https://raw.githubusercontent.com/clowwindy/ShadowVPN/master/samples/windows/client_down.bat
下载的配置文件在 /root 目录，请使用filezilla 将 client.conf client_up.bat client_down.bat 三个文件下载下来和 shadowvpn.exe 文件放到同一个目录。

修改 client.conf

找到 server=127.0.0.1 行，修改为 server=VPS的IP 找到 intf=Local Area Connection 2 行，修改为 intf=ShadowVPN
修改 client_up.bat

找到 SET orig_intf="Local Area Connection" 行，修改为： SET orig_intf="本地连接"
修改 client_down.bat

找到 SET orig_intf="Local Area Connection" 行，修改为： SET orig_intf="本地连接"
运行VPN客户端 打开系统 开始 -> 所有程序 -> 附件 -> 命令提示符， 右键选择 以管理员身份运行，然后进入本地 VPN 的目录执行 shadowvpn.exe -c client.conf 即可完成 VPN 连接，需要终止连接，直接 Ctrl + C 中断即可。
也可以直接选中 shadowvpn.exe 鼠标右键，“创建快捷方式”。然后点选所创建的快捷方式，按鼠标右键选择 “属性” ，将目标由 shadowvpn.exe 修改为 shadowvpn.exe -c client.conf ，点确定退出，再次点选所创建的快捷方式，按鼠标右键，选择 “以管理员身份运行”。

