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
- 生成git ssh密钥
```
ssh-keygen -t rsa -C "YOUR_EMAIL@YOUREMAIL.COM" -f ~/.ssh/gitlab
```
- 将 C:\Users\Administrator\.ssh\.ssh\gitlab.pub中的内容复制至gitlab网站上
- 设置host，C:\Windows\System32\drivers\etc\hosts
```
192.168.1.3	gitlab.cn
```
- 修改 C:\Users\Administrator\.ssh\config
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
- 测试连接SSH
```
ssh -T git@gitcafe.com
```
- 查看 C:\Users\Administrator\.ssh\known_hosts
```
gitlab.cn,192.168.1.3 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL3XCzfzPoNx01a4Y6mYHNtRg8emLiqnCGlR3iQsUDqs+yGq/9ot9FUvcQ4E1PH2IqZ61gi/uAPTEIju5VvXVCA=
```

### 自动构建与部署
> 安装gitlab-runner
```
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-ci-multi-runner/script.rpm.sh | sudo bash
yum install gitlab-ci-multi-runner
gitlab-runner register
chmod -R 777 /home/gitlab-runner
vi /etc/systemd/system/gitlab-runner.service
gitlab-runner status
gitlab-runner run
ps aux|grep gitlab-runner
```
> 安装nodejs
```
# 访问网站查看下载连接
http://nodejs.cn/download/
wget https://npm.taobao.org/mirrors/node/v14.0.0/node-v14.0.0-linux-x64.tar.xz
tar -xvf node-v14.0.0-linux-x64.tar.xz
ln -s /root/nodejs/bin/node /usr/local/bin/node
ln -s /root/nodejs/bin/npm /usr/local/bin/npm
npm install -g cnpm --registry=https://registry.npm.taobao.org
ln -s /root/nodejs/bin/cnpm /usr/local/bin/cnpm
```
> 处理node版本冲突
```
find / -name node
/usr/local/bin/node -v
/usr/bin/node -v
ln -s /usr/local/bin/node /usr/bin/node
```
> 安装yarn
```
curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
yum install -y yarn
yarn -version
```
> 免密gitlab服务器登录nginx服务器
```
# gitlab服务器上：
# 修改主机名称
vi /etc/hostname
reboot
uname -n
# 创建公匙
cd ~/.ssh
ssh-keygen -t dsa -P '' -f id_dsa
# 一路确定
cat id_dsa.pub >> authorized_keys
# nginx服务器上：
cd ~/.ssh
scp gitlab服务器IP:/root/.ssh/authorized_keys ./authorized_keys.gitlab
cat authorized_keys.gitlab >> authorized_keys
# gitlab服务器上测试免密连接：
ssh -tt root@nginx服务器ip
logout
```

### 卸载gitlab
```
ps aux | grep gitlab
# 查看 /opt/gitlab/service 的进程号
kill -9 进程号
rpm -e gitlab-ee
find / -name gitlab | xargs sudo rm -rf
reboot
ps aux | grep gitlab
```