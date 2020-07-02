# SSL 인증서 적용하기



{% embed url="https://wiki.centos.org/HowTos/Https" %}

안녕하세요~~~ 위의 링크를 참조할꺼에요!

우선 인증서를 발급받았을때 저희가 가지고 있는 파일은 딱 두가지 인데요!  
하나는 개인키 파일입니다~ 이건 rsa 방식으로 2048 이런식으로 길이를 지정해서 아무 문자열을 생성해줍니다.  
그리고 또하나는 crt 파일입니다. 이 파일은 말그대로 서명이 완료된 인증서입니다!  
위에서는 사설키를 생성하는 과정에서 csr 이라는 파일로 서명정보를 생성한 후 crt 파일을 만들어주는 방식을 채택하고 있습니다.  
따라서 직접 돈을 주고 인증서를 구매하신다면, 개인 키 파일 1개와 서명완료된 인증서파일 1개를 받으실 수 있습니다.  
물론 발급받을시 서명정보를 입력하기 때문에 그 정보가 인증서에 잘 담겨져 있습니다.  
그러므로 웹 서버에서는 개발시에는 사설 인증키를 적용하여 처리한 후, 개발 완료시에 인증서 및 도메인을 적용하여 웹서버 설정을 변경하시는 것을 추천합니다!

```text
yum install mod_ssl openssl
```



```text
# Generate private key 
openssl genrsa -out ca.key 2048 

# Generate CSR 
openssl req -new -key ca.key -out ca.csr

# Generate Self Signed Key
openssl x509 -req -days 365 -in ca.csr -signkey ca.key -out ca.crt

# Copy the files to the correct locations
cp ca.crt /etc/pki/tls/certs
cp ca.key /etc/pki/tls/private/ca.key
cp ca.csr /etc/pki/tls/private/ca.csr
```



셀리눅스 사용시 참고사항

```text
restorecon -RvF /etc/pki
vi +/SSLCertificateFile /etc/httpd/conf.d/ssl.conf
SSLCertificateFile /etc/pki/tls/certs/ca.crt
SSLCertificateKeyFile /etc/pki/tls/private/ca.key
/etc/init.d/httpd restart
```



SSL 인증서 및 https 포트 추

```text
NameVirtualHost *:443

<VirtualHost *:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/ca.crt
    SSLCertificateKeyFile /etc/pki/tls/private/ca.key
    <Directory /리소스 루트폴더>
    AllowOverride All
    </Directory>
    DocumentRoot /리소스 루트폴더
    ServerName localhost
</VirtualHost>
```











































우분투 버전별 정리

센토스 버전별 정

