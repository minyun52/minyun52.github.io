# Apache에 SSL 모듈 설치 / 환경설정

## mod_ssl이란?

openssl을 이용하여 SSL이나 TLS 프로토콜 암호화를 해주는 Apache 모듈이다.

그러면 openssl은 무엇일까?

컴퓨터 네트워 상의 보안 통신을 위한 암호화 라이브러리이다.

암호화 보안 프로토콜인 TLS나 SSL 프로토콜 구현을 제공하고 있다.

## Apache에 SSL 모듈 설치

먼저 openssl이 있는지 확인한다.

```bash
openssl version
```

위 명령어를 실행하고 OpenSSL 1.0.2k-fips 26 Jan 2017 이 설치되어있는 걸 확인했다.

이제 mod_ssl을 설치해준다.

```bash
sudo yum install mod_ssl
```

설치를 완료하고 모듈이 정상적으로 설치 되었는지 확인해본다.

```bash
ll /usr/lib64/httpd/modules | grep ssl
```

위 명령어를 통해 검색 결과로 mod_ssl.so가 나오면 정상적으로 설치가 된 것이다.

## 가상 호스트에 SLL 인증서 설정

가상호스트에 SSL 인증서를 설정하려면 개인키와 인증서에 대한 설정이 필요하다.

### Let’s Encrypt로 SSL 인증서 발급

lets encrypt는 ACME 프로토콜을 사용하여 도메인명의 유효성을 확인하고 인증서를 발급한다.

따라서, ACME 클라이언트 소프트웨어를 하나 설치해야한다.

그중에서 certbot 사용이 가장 권장되어진다. 

```bash
sudo yum install certbot python2-certbot-apache
```

위 명령어로 아파치용 certbot을 설치한다.

이제 certbot을 통해서 인증서를 발급해본다.

인증서 발급에는 다양한 옵션들이 있다.

1. standalone 
    1. 80 or 443 포트를 사용할 수 있어야한다.
    2. 도메인이 서버에 연결되어 있어야한다.
    3. 절차는 간소하나 웹서버를 중지 시키고 설치해야한다.
2. webroot
    1. 웹서버 중단없이 발급한다.
    2. 웹서버에 미리 설정을 해야한다.
3. manual
    1. 수동으로 지침에 따라 인증서를 가져와 도메인 검증을 직접 수행한다.
    2. 자동 새로 만들기를 지원하지 않는다.
4. apache
    1. Apache를 사용하여 인증서 획득 및 설치를 자동화한다.
5. nginx
    1. Nginx로 인증서 획득 및 설치를 자동화한다.

인증서 발급에는 standalone 옵션으로 진행하였다.

먼저 웹서버를 중지시킨다.

```bash
sudo systemctl stop httpd
```

certbot standalone 옵션으로 인증서를 발급 받는다.

```bash
sudo certbot --standalone -d goodluckmin.ze.to certonly
```

인증서를 발급 받고자하는 도메인을 -d 옵션 뒤에 설정한다.

certonly 옵션은 certbot이 웹서버 구성파일을 직접 수정하지 못하도록 한다.

```bash
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/goodluckmin.ze.to/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/goodluckmin.ze.to/privkey.pem
   Your certificate will expire on 2022-11-21. To obtain a new or
   tweaked version of this certificate in the future, simply run
   certbot again. To non-interactively renew *all* of your
   certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

이메일을 입력하고 정보에 동의하면 위와 같은 설치 성공 문구가 나온다.

인증서와 인증키가 저장된 경로가 나오고

인증서 만료일도 나온다.

certbot renew라는 명령어를 통해서 모든 인증서를 갱신할 수 있다라는 문구도 적혀있다.

### 생성한 인증서를 Apache 웹서버에 설정

443 포트에 대한 인증서 설정은 ssl.conf 파일내에서 설정한다.

```bash
sudo vi /etc/httpd/conf.d/ssl.conf
```

```bash
<VirtualHost _default_:443>
    SSLCertificateFile /etc/letsencrypt/live/goodluckmin.ze.to/cert.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/goodluckmin.ze.to/privkey.pem
    SSLCertificateChainFile /etc/letsencrypt/live/goodluckmin.ze.to/chain.pem
    SSLCACertificateFile /etc/letsencrypt/live/goodluckmin.ze.to/fullchain.pem
</VirtualHost>
```

새로 발급받은 키와 인증서들을 VirtualHost *default*:443 밑에 정의해준다.

기존에 존재하는 설정은 주석처리를 해주었다.

다시 웹서버를 가동 시켜준다.

```bash
sudo systemctl start httpd
```

그리고 설정한 도메인으로 접속을 해본다.

![ssl2](/img/ssl2.png)

발급 받은 인증서를 확인할 수 있다.