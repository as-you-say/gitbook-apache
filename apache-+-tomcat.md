# Apache + Tomcat 연동하기

## wget 패키지 설치

```text
yum install -y wget
```

## 컴파일에 필요한 패키지 설치

```text
yum install -y gcc gcc-c++ make
```

## 커넥터 다운로드

```text
wget https://archive.apache.org/dist/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.48-src.tar.gz
```

## 커넥터 압축풀기

```text
tar -zxvf tomcat-connectors-1.2.48-src.tar.gz
```

## 커넥터 컴파일 및 설치

```text
cd tomcat-connectors-1.2.48-src/native
./configure --with-apxs=/usr/bin/apxs
make
make install
```

## 생성된 mod\_jk 모듈을 복사

```text
cp ./tomcat-connectors-1.2.48-src/native/apache-2.0/mod_jk.so /etc/httpd/modules/
```

## workers.properties 파일 생성

```text
cd /etc/httpd/conf
touch workers.properties
echo 'worker.list=worker1' >> workers.properties
echo 'worker.worker1.port=8009' >> workers.properties
echo 'worker.worker1.host=localhost' >> workers.properties
echo 'worker.worker1.type=ajp13' >> workers.properties
echo 'worker.worker1.lbfactor=1' >> workers.properties
```

## mod\_jk.conf 파일 생성

셀리눅스의 httpd 접근제한 때문에 JkShmFile 을 logs/mod\_jk.shm 으로 할 수 없다.

```text
vi mod_jk.conf

<IfModule jk_module>
    JkWorkersFile conf/workers.properties
    JkLogStampFormat "[%a %b %d %H:%M:%S %Y] "
    JkLogFile logs/mod_jk.log
    JkLogLevel info
    JkShmFile run/mod_jk.shm
    JkMountFile /* worker1
</IfModule>
```

## httpd.conf 수정

```text
vi /etc/httpd/conf/httpd.conf

ServerRoot "/etc/httpd"
Listen 80
Include conf.modules.d/*.conf
User nobody
Group nobody
ServerAdmin root@localhost
ServerName localhost
<Directory />
    AllowOverride none
    Require all granted
</Directory>
DocumentRoot "/var/www/html"
<Directory "/var/www">
    AllowOverride None
    Require all granted
</Directory>
<Directory "/var/www/html">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
<IfModule dir_module>
    DirectoryIndex index.html
</IfModule>
<Files ".ht*">
    Require all denied
</Files>
LogLevel warn
<IfModule log_config_module>
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    LogFormat "%h %l %u %t \"%r\" %>s %b" common
    <IfModule logio_module>
      LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
    </IfModule>
</IfModule>
<IfModule alias_module>
    ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"
</IfModule>
<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options None
    Require all granted
</Directory>
<IfModule mime_module>
    TypesConfig /etc/mime.types
    AddType application/x-compress .Z
    AddType application/x-gzip .gz .tgz
    AddType text/html .shtml
    AddOutputFilter INCLUDES .shtml
</IfModule>
AddDefaultCharset UTF-8
<IfModule mime_magic_module>
    MIMEMagicFile conf/magic
</IfModule>
EnableSendfile on
IncludeOptional conf.d/*.conf

# 추가모듈
<IfModule jk_module>
    JkWorkersFile conf/workers.properties
    JkLogStampFormat "[%a %b %d %H:%M:%S %Y] "
    JkLogFile logs/mod_jk.log
    JkLogLevel info
    JkShmFile run/mod_jk.shm
    JkMount /* worker1
</IfModule>
```

## Apache 재시작

```text
service httpd restart
```

