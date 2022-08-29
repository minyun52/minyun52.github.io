# Apache_ssl.conf

Date: August 26, 2022

```bash
SSLPassPhraseDialog exec:/usr/libexec/httpd-ssl-pass-dialog
```

아파치가 시작될 때 여러 인증서와 개인키 파일을 읽는다. 개인키 파일들은 보통 암호화 처리가 되어있어서 mod_ssl 모듈은 복호화하기 위해서 passphrase를 관리자에게 요청한다. 이 요청은 2가지 타입으로 가능하다.

1. builtin
    1. 기본값
2. exec : / path / to / program
    1. 아파치가 시작할때 개인키 파일을 읽은 외부 프로그램을 지정하는 방식

```bash
SSLSessionCache         shmcb:/run/httpd/sslcache(512000)
SSLSessionCacheTimeout  300
```

SSL Session Cache를 어떻게 보관할지 정하는 설정.

병렬처리요청에 속도를 향상시키기 위한 옵션들이 있다.

1. none
    1. 기본값
    2. global/interprocess SSL Session Cashe를 비활성화한다.
    3. 기능상 단점은 없지만 눈에 띄는 속도 저하가 생길 수 있다.
2. dbm: /path/to/datafile
    1. 로컬 저장공간의 DBM hashfile를 사용해서 서버 프로세스의 로컬 OpenSSL 메모리 캐시와 동기화 한다.
    2. 서버의 입출력 속도가 약간 증가하고, 클라이언트의 요청 속도가 눈에 띄게 빨라진다.
    3. 가장 권장하는 옵션
3. shm:/path/to/datafile(size)
    1. RAM의 공유 메모리 세그먼트의 내부 고성능 해시 테이블을 사용한다.
    2. 모든 플랫폼에서 사용할 수 없다.

SSLSessionCacheTimeout은 캐시의 타임 아웃 시간을 설정한다.

```bash
SSLRandomSeed startup file:/dev/urandom  256
SSLRandomSeed connect builtin
```

서버가 시동할 때 혹은 새로운 SSL 연결이 이루어질 때 Pseudo Random Number Generator(PRNG)의 seeding을 위한 하나 혹은 여러개의 seed source를 설정한다

startup file:/path/to/source

PRNG의 seeding을 위해 지정한 경로의 외부 파일을 사용한다.

지정한 바이트에 따라 entropy를 생성한다.

connect builtin

실행 시 최소한의 CPU cycle을 소비하며, 결점없이 사용할 수 있다.

그다지 효과적인 소스는 아니기 때문에, 추가적인 seeding source를 지정하는게 좋다.

```bash
SSLCryptoDevice builtin
```

SSL 툴킷이 engine을 지원하는 경우에 사용 가능하고 하드웨어 가속보드를 사용할 수 있는 설정

```bash
DocumentRoot "/var/www/html/goodluckmin.ze.to"
ServerName www.goodluckmin.ze.to:443
```

https로 들어오는 요청의 루트 디렉터리 경로와 서버이름을 설정

```bash
ErrorLog logs/ssl_error_log
TransferLog logs/ssl_access_log
LogLevel warn
```

SSL 가상 호스트의 로그 경로를 설정

httpd.conf의 로그 경로를 상속 받지 않는다.

```bash
SSLEngine on
```

SSL 가상 호스트 활성화/비화설화 여부 설정

```bash
SSLProtocol all -SSLv3
```

클라이언트가 접속이 가능한 프로토콜을 설정한다 

SSLv2은 비활성화가 기본값이다.

```bash
SSLCipherSuite HIGH:MEDIUM:!aNULL:!MD5:!SEED:!IDEA
```

클라이언트가 사용할 수 있는 ciphers들을 설정한다.

이를 위해 콜론이나 콤마(,)로 구분되는 SSLeay cipher specification들로 이루어진 스트링을 값으로 지정한다.

! : 리스트로부터 cipher를 완전히 제거한다

none : 리스트에 cipher를 포함시킨다

+: cipher를 리스트에 포함시키며 또한 리스트이 현재 위치로 가져온다.

-: 리스트로부터 cipher를 제거한다(이후에 다시 포함시킬수 있다.)

```
SSLCertificateFile /etc/letsencrypt/live/goodluckmin.ze.to/cert.pem
SSLCertificateKeyFile /etc/letsencrypt/live/goodluckmin.ze.to/privkey.pem
SSLCertificateChainFile /etc/letsencrypt/live/goodluckmin.ze.to/chain.pem
SSLCACertificateFile /etc/letsencrypt/live/goodluckmin.ze.to/fullchain.pem
```

인증서 관련 파일들의 경로를 지정한다.

```bash
SSLVerifyDepth  10
```

클라이언트 인증에 대한 인증서 확인 수준을 설정한다.

4가지 옵션

1. none : 클라이언트 인증서를 필요로 하지 않는다.
2. optional : 유효한 인증서는 선택 사항이다.
3. require : 클라이언트는 유효한 인증서를 제시해야 한다.
4. optional_no_ca : 클라이언트는 유효한 인증서를 제시해야 한다. 그러나 검증할 필요는 없다.

SSLVerifyDepth는 클라이언트가 유효한 인증서를 판단하기 까지의 mod_ssl이 검증하는 깊이를 설정한다.

```bash
<Location />
SSLRequire (    %{SSL_CIPHER} !~ m/^(EXP|NULL)/ \
            and %{SSL_CLIENT_S_DN_O} eq "Snake Oil, Ltd." \
            and %{SSL_CLIENT_S_DN_OU} in {"Staff", "CA", "Dev"} \
            and %{TIME_WDAY} >= 1 and %{TIME_WDAY} <= 5 \
            and %{TIME_HOUR} >= 8 and %{TIME_HOUR} <= 20       ) \
           or %{REMOTE_ADDR} =~ m/^192\.76\.162\.[0-9]+$/
</Location>
```

SSLRequire를 통해서 디렉터리별 접근 권한을 설정할 수 있다.

boolean 표현식으로 이 접근 권한을 설정한다. 문법은 C언어와 Perl를 사용한다.

```bash
<Files ~ "\.(cgi|shtml|phtml|php3?)$">
    SSLOptions +StdEnvVars
</Files>
<Directory "/var/www/cgi-bin">
    SSLOptions +StdEnvVars
</Directory>
```

SSL Engine에 옵션을 설정

1. StdEnvVars 
    1. 기본 SSL/TLS과 관련된 환경 변수들을 내보낸다.
2. FakeBasicAuth
    1. 접근 관리에 기본 Auth/DBMAuth 방식을 사용한다.
3. ExportCertData
    1. 2가지 추가적인 환경 변수(SSL_CLIENT_CERT와 SSL_SERVER_CERT)를 내보낸다. 
    2. 2가지 환경 변수에는 서버와 클라이언트의 PEM 처리 된 인증서를 가지고 있다.
    3. 인증서들을 CGI 스크립트로 가져올 수 있게 해준다.
4. StrictRequire
    1. SSLRequireSSL이나 SSLRequire 접근을 모두 차단한다.
5. OptRenegotiate
    1. SSL 지시문이 디렉터리별로 지정되어 있으면 SSL 연결 재협상 핸들링을 최적화시켜준다.

```bash
BrowserMatch "MSIE [2-5]" \
         nokeepalive ssl-unclean-shutdown \
         downgrade-1.0 force-response-1.0
```

첫 번째 매개변수와 브라우저가 보낸 User-Agent 헤더가 일치할 경우 환경 변수를 설정하거나 해제할 수 있다.

기본 아파치 SSL 설정은 IE에 보수적이다. 기본적으로 IE와 https를 사용할 때는 모든 연결 유지를 비활성화 시킨다.

downgrade-1.0 force-response-1.0 → 자바 VM이 요청을 HTTP/1.1 포맷으로 만든다 하지만 응답은 반드시 HTTP/1.0 포맷으로 나가야한다.

이렇게 하는 이유는 아파치를 요청이 HTTP/1.0으로 들어왔다라고 속이기 위해서이다.

```bash
CustomLog logs/ssl_request_log \
          "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
```

서버별 로그 커스텀 로그 파일의 루트 경로 설정