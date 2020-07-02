# Tomcat 설치하기

## wget 패키지 설치

```text
yum install -y wget
```

## 다운로드

```text
wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.0.22/bin/apache-tomcat-8.0.22.tar.gz
```

## 압축풀기

```text
tar -zxvf apache-tomcat-8.0.22.tar.gz
```

## 폴더이동

```text
mkdir -p /usr/local/tomcat
mv ./apache-tomcat-8.0.22 /usr/local/tomcat
```

## 서비스 등록

```text
cd /etc/init.d
touch tomcat
chmod 755 tomcat
echo '#!/bin/sh' >> tomcat
echo '# chkconfig: 345 85 15' >> tomcat 
echo '# description: apache tomcat 8.x' >> tomcat
echo '# processname: tomcat' >> tomcat
echo 'export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk' >> tomcat
echo 'export JRE_HOME=/usr/lib/jvm/jre-1.8.0-openjdk' >> tomcat
echo 'export CATALINA_HOME=/usr/local/tomcat/apache-tomcat-8.0.22' >> tomcat
echo 'export JAVA_OPTS="-server -Xms512m -Xmx512m"' >> tomcat
echo 'export PATH=$PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin' >> tomcat
echo 'case "$1" in' >> tomcat
echo '  start)' >> tomcat
echo '  echo -n "Starting tomcat: "' >> tomcat
echo '  $CATALINA_HOME/bin/catalina.sh start' >> tomcat
echo '  echo' >> tomcat
echo '  ;;' >> tomcat
echo '  stop)' >> tomcat
echo '  echo -n "Shutting down tomcat: "' >> tomcat
echo '  $CATALINA_HOME/bin/catalina.sh stop' >> tomcat
echo '  echo' >> tomcat
echo '  ;;' >> tomcat
echo '  restart)' >> tomcat
echo '  $0 stop' >> tomcat
echo '  sleep 2' >> tomcat
echo '  $0 start' >> tomcat
echo '  ;;' >> tomcat
echo '  *)' >> tomcat
echo '  echo "Usage: $0 {start|stop|restart}"' >> tomcat
echo '  exit 1' >> tomcat
echo 'esac' >> tomcat
echo 'exit 0' >> tomcat
chkconfig --add tomcat
```

## 포트허용

```text
firewall-cmd --permanent --zone=public --add-port=8080/tcp
firewall-cmd --reload
```

## 실행하기

```text
service tomcat start
```

