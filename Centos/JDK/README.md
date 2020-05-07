
### 配置环境变量
```
vi /etc/profile
```

```
# 末尾加入
JAVA_HOME=/root/jdk1.7
JRE_HOME=/root/jdk1.7/jre
PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
export JAVA_HOME JRE_HOME PATH CLASSPATH
```

```
source /etc/profile
```