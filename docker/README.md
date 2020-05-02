
```
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
```
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
```
yum install docker-ce docker-ce-cli containerd.io
systemctl start docker
systemctl restart docker
systemctl stop docker
systemctl status docker
docker run hello-world
```