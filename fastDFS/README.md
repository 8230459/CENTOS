
```
yum -y install wget perl cpan gcc gcc-c++ zlib zlib-devel pcre pcre-devel openssl openssl-devel libevent libevent-devel unzip
```
### 下载，解压
```
cd ~
wget https://github.com/happyfish100/libfastcommon/archive/V1.0.39.tar.gz -SO libfastcommon.tar.gz
wget https://github.com/happyfish100/fastdfs/archive/V5.11.tar.gz -SO fastdfs.tar.gz
wget https://github.com/happyfish100/fastdfs-nginx-module/archive/V1.20.tar.gz -SO fastdfs-nginx-module.tar.gz
wget http://nginx.org/download/nginx-1.14.1.tar.gz
```

```
tar -xf libfastcommon.tar.gz fastdfs.tar.gz fastdfs-nginx-module.tar.gz nginx-1.14.1.tar.gz
```

```
cd ~/libfastcommon-1.0.39
./make.sh clean
./make.sh && ./make.sh install
```

```
cd ~/fastdfs-5.11
./make.sh clean
./make.sh && ./make.sh install
```

```
which fdfs_trackerd
ls /etc/fdfs
```

```
cp ~/fastdfs-5.11/conf/* /etc/fdfs
ls /etc/fdfs
```

```
vi /etc/fdfs/tracker.conf
base_path=/home/fdfs
http.server_port=9270
```

```
vi /etc/fdfs/storage.conf
base_path=/home/fdfs
store_path0=/home/fdfs
tracker_server=192.168.0.33:22122
http.server_port=8888
```

```
vi /etc/fdfs/client.conf
base_path=/home/fdfs/client
tracker_server=192.168.0.33:22122
http.tracker_server_port=9270
```

```
vi /root/fastdfs-nginx-module-1.20/src/mod_fastdfs.conf
tracker_server=192.168.0.33:22122
store_path0=/home/fdfs
```

```
mkdir /home/fdfs/data  /home/fdfs/client  /home/fdfs/logs
```

### 安装nginx

```
vi ~/fastdfs-nginx-module-1.20/src/config
```
### 修改以下2项如下
```
ngx_module_incs="/usr/include/fastdfs /usr/include/fastcommon/"
CORE_INCS="$CORE_INCS /usr/include/fastdfs /usr/include/fastcommon/"
```

```
cd ~/nginx-1.14.1
./configure --prefix=/usr/local/nginx --add-module=/root/fastdfs-nginx-module-1.20/src
make && make install
```
```
cp /root/fastdfs-nginx-module-1.20/src/mod_fastdfs.conf /etc/fdfs/mod_fastdfs.conf
```

### 启动nginx
```
/usr/local/nginx/sbin/nginx
/usr/local/nginx/sbin/nginx -s reload
/usr/local/nginx/sbin/nginx -s stop
```
### 开放80端口
```
firewall-cmd --zone=public --add-port=80/tcp --permanent
systemctl restart firewalld.service
```

```
/usr/local/nginx/sbin/nginx –v
ps -ef | grep nginx
netstat -ntlp
```

```
vi /usr/local/nginx/conf/nginx.conf
server {
    listen 8777;
    location /M00 {
        root /home/fdfs/data;
        ngx_fastdfs_module;
    }
}
```

### 开放8777端口
```
firewall-cmd --zone=public --add-port=8777/tcp --permanent
systemctl restart firewalld.service
```

### 启动fdfs
```
fdfs_trackerd /etc/fdfs/tracker.conf start
fdfs_storaged /etc/fdfs/storage.conf start
```

- 若 fdfs_storaged: 未找到命令
- 则 cp /root/fastdfs-5.11/storage/fdfs_storaged /usr/bin/fdfs_storaged
```
ps -ef | grep fdfs
```
### 测试上传
```
fdfs_test /etc/fdfs/client.conf upload ~/1.png
http://192.168.0.34:8777/M00/00/00/。。
```

### 开机nginx自启
```
vi /etc/init.d/nginx"
# 粘贴nginx内容并保存

chmod a+x /etc/init.d/nginx
/etc/init.d/nginx start
/etc/init.d/nginx stop
chkconfig --add /etc/init.d/nginx
service nginx start
service nginx stop
service nginx restart
chkconfig nginx on
```

