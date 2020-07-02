# CentOS 7 에서 설치하기

## CentOS 7 에서 설치하기

### 1. httpd 설치

```text
$ yum install -y httpd
```

### 2. 방화벽 80 포트 허용

```text
$ firewall-cmd --permanent --zone=public --add-port=80/tcp
```

### 3. 방화벽 새로고침

```text
$ firewall-cmd --reload
```

설치가 완료되면,  /var/www/html 폴더 안에 리소스 파일을 담아두고 사용하실 수 있습니다.  
별도로 다른 폴더도 공유하고 싶다면, /etc/httpd/conf/httpd.conf 파일을 찾아서 편집하실 수 있습니다.

### 4. 아이피 접속

예\) http://192.168.136.140

아래와 같은 화면이 뜬다면 성공한 것입니다.

![](.gitbook/assets/.jpg.jpeg)

