### 官网安装教程
```
https://about.gitlab.com/install/#centos-7
```
### 修改配置
```
vi /etc/gitlab/gitlab.rb
```
> 修改以下内容
```
# 这里用域名或本机IP地址都可以，用域名时客户机须设置host映射，这里和仓库的ssh路径对应
external_url 'http://192.168.1.3'
# 若服务器80端口已使用则修改以下nginx端口
nginx['listen_port'] = 80
# 解除注释
unicorn['listen'] = 'localhost'
# 若服务器8080端口已使用则修改以下unicorn端口
unicorn['port'] = 8080
```
### 操作
```
# 启动所有组件
gitlab-ctl start
# 停止所有组件
gitlab-ctl stop
# 重启所有组件
gitlab-ctl restart
# 启动服务
gitlab-ctl reconfigure
# 查看服务状态
gitlab-ctl status
# 查看日志
gitlab-ctl tail
```
### 配置客户机
>

> 关注以下3个文件
- C:\Windows\System32\drivers\etc\hosts
```
192.168.1.3	gitlab.cn
```
- C:\Users\Administrator\.ssh\config
```
Host gitee.com www.gitee.com
IdentityFile ~/.ssh/gitee
Host github.com www.github.com
IdentityFile ~/.ssh/github
Host gitlab.cn
IdentityFile ~/.ssh/gitlab
Host 192.168.1.3
IdentityFile ~/.ssh/gitlab
```
- C:\Users\Administrator\.ssh\known_hosts
```
gitlab.cn,192.168.1.3 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL3XCzfzPoNx01a4Y6mYHNtRg8emLiqnCGlR3iQsUDqs+yGq/9ot9FUvcQ4E1PH2IqZ61gi/uAPTEIju5VvXVCA=
```