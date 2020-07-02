# OPEN JDK 설치하기

## 설치하기

```text
# OPEN-JDK 1.8.0 설치
yum install -y java-1.8.0-openjdk-devel
```

## 환경변수 추가하기

```text
echo 'JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk' >> /etc/profile
echo 'PATH=$PATH:$JAVA_HOME/bin' >> /etc/profile
source /etc/profile
```



