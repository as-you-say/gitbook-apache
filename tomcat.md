# Tomcat 설치하기



```text
1. OPEN JDK
- yum 패키지로 설치

2. 톰캣
- wget 으로 압축파일 원하는버전 다운
- 압축 풀기
- 서비스 등록 (수동)
- 켜기
- 8080 포트 페이지 확인
- 끄기
- server.xml 8080 막기

3. 아파치
- yum 패키지로 설치
- 켜기
- 80 포트 페이지 확인

4. 톰캣 아파치 커넥터
- 압축풀기
- 컴파일
- 설치
- mod_jk.so 복사해서 아파치 모듈에 넣기

5. 아파치
- workers.properties 파일 생성
worker.list=worker1
worker.worker1.type=ajp13         # AJP1.3 프로토콜을 사용
worker.worker1.host=localhost     # 톰캣은 local에서 돌고 있습니다.
worker.worker1.port=8009	       # 연결할 톰캣의 포트 번호

- httpd.conf 설정 추가
LoadModule jk_module            modules/mod_jk.so
<IfModule mod_jk.c>
        JkWorkersFile /usr/local/apache/apache2.0.64/conf/workers.properties
        JkLogFile /usr/local/apache/apache2.0.64/logs/mod_jk.log
        JkLogLevel info
        JkLogStampFormat "[%a %b %d %H:%M:%S %Y]
        JkShmFile /usr/local/apache/apache2.0.64/logs/mod_jk.shm
        JkMount /* worker1
</IfModule>

- 아파치 재시작
service httpd restart

- 80 포트 접속시 톰캣페이지가 보이는지 확인
```

