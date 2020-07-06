# SSL 인증서 적용하기



{% embed url="https://wiki.centos.org/HowTos/Https" %}



```text
yum install mod_ssl openssl
```



```text
# Generate private key 
openssl genrsa -out ssl.key 2048

# Generate CSR 
openssl req -new -key ssl.key -out ssl.csr

# Generate Self Signed Key
openssl x509 -req -days 365 -in ssl.csr -signkey ssl.key -out ssl.crt

# Copy the files to the correct locations
cp ssl.crt /etc/pki/tls/certs
cp ssl.key /etc/pki/tls/private/ssl.key
cp ssl.csr /etc/pki/tls/private/ssl.csr
```



셀리눅스 사용시 참고사항

```text
restorecon -RvF /etc/pki
vi +/SSLCertificateFile /etc/httpd/conf.d/ssl.conf
SSLCertificateFile /etc/pki/tls/certs/ca.crt
SSLCertificateKeyFile /etc/pki/tls/private/ca.key
/etc/init.d/httpd restart
```



SSL 인증서 및 https 포트 상단에 추

```text
NameVirtualHost *:443

<VirtualHost *:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/ssl.crt
    SSLCertificateKeyFile /etc/pki/tls/private/ssl.key
    <Directory /톰캣폴더/webapps>
      Options Indexes FollowSymLinks
      AllowOverride none
      Require all granted
    </Directory>
    DocumentRoot /톰캣폴/webapps/ROOT
    ServerName localhost

    JkMount /* ajp13
</VirtualHost>
```











































우분투 버전별 정리

센토스 버전별 정

