
### 安装配置
```
yum install -y haproxy
vi /etc/haproxy/haproxy.cfg
```
```
listen stats
        mode http
        bind 0.0.0.0:1080
        stats enable
        stats hide-version
        stats uri /haproxyadmin?stats
        stats realm Haproxy\ Statistics
        stats auth admin:admin
        stats admin if TRUE

backend mysqlservers
        balance leastconn
        server dbsrv1 192.168.0.33:3306 check port 3306 rise 1 fall 2 maxconn 300
        server dbsrv2 192.168.0.34:3306 check port 3306 rise 1 fall 2 maxconn 300
```
### 启动
```
/usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg
firewall-cmd --zone=public --add-port=1080/tcp --permanent
systemctl restart firewalld.service
```
> 若服务器IP是192.168.0.33，访问监控页面就是192.168.0.33:1080/haproxyadmin?stats，登录账号密码admin/admin