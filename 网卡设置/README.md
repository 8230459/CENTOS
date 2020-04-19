### 虚拟机查找网卡
```
cd /etc/sysconfig/network-scripts
ls
```
### 设置网卡
```
vi ifcfg-enp0s3
BOOTPROTO=STATIC
IPADDR=192.168.0.34
NETMASK=255.255.0.0
GATEWAY=192.168.0.1
```
### 重启网卡
```
service network restart
```
### 虚拟机设置与主机互联并可以连接外网
- 1.关闭系统
- 2.设置 =》 网络 =》 选择桥接网卡、本地联网的网卡
- 3.设置 =》 网络 =》 网卡2 =》 启用网络连接 =》 选择网络地址转换（NAT）