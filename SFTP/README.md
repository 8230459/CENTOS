
### 创建用户
```
useradd -s /sbin/nologin -m andy
passwd andy
chown root:andy /home/andy
chmod 755 /home/andy
```
| 不能在用户目录/home/andy设置操作权限，必须新建data目录
```
mkdir /home/andy/data
chown root:andy /home/andy/data
chmod 777 /home/andy/data
```
### 删除用户目录隐藏文件
```
cd /home/andy
rm -rf .bash_logout .bash_profile .bashrc
ls -al
```
### 修改sshd配置
```
vi /etc/ssh/sshd_config
Subsystem sftp internal-sftp
Match User last
ChrootDirectory /home/%d
X11Forwarding no
AllowTcpForwarding no
ForceCommand internal-sftp
systemctl restart sshd.service
```