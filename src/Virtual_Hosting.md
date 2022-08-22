# Virtual_Hosting

[참고 자료](https://thewebhacker.com/setting-up-apache-vhosts-on-aws-linux/)

1. 먼저 /etc/httpd/conf.d 밑에 virtual hosting을 설정할 vhosts.conf 파일을 만든다

```bash
cd /etc/httpd/conf.d
로 먼저 이동해준다.
sudo vi vhosts.conf
로 설정 파일을 만든다.
```

설정 파일에 다음과 같은 내용을 적어준다.

```bash
<VirtualHost *:80>

  ServerName minvhtest.com

  DocumentRoot /var/www/html/minvhtest.com

  <Directory /var/www/html/minvhtest.com>
    AllowOverride All
  </Directory>

</VirtualHost>
```

DocumentRoot에 지정해준 디렉터리를 동일한 경로에 만들어준다.

```bash
cd /var/www/html/
로 먼저 이동을 했다.
sudo mkdir minvhtest.com
으로 디렉터리를 생성했다.
```

[minvhtest.com](http://minvhtest.com) 디렉터리에 간단한 html 화면을 만들었다.

퍼블릭 아이피 주소나 virtual hosting에 설정한 ServerName으로 접속하면 해당 html이 나온다.

```bash
sudo vi index.html
```

원하는 간략한 html 코드를 입력한다.

그리고 Apache를 재시작 해준다.

```bash
sudo service httpd restart
```

해당 주소에 접속에 앞서서 로컬 환경에 hosts 파일에 새로운 도메인을 등록해준다.

나는 맥os 환경이라 아래와 같이 해주었다.

```bash
sudo vi /etc/hosts
```

hosts 파일을 열고 제일 하단에

퍼블릭 아이피 주소와 설정한 도메인 주소를 넣어준다.

그리고 접속을 해본다.

![vh1](/img/vh1.png)

왼쪽은 도메인 주소로 접속을 하였고

오른쪽은 퍼블릭 아이피 주소로 접속을 하였다.

동일한 화면이 나오면서 virtual hosting이 정상적으로 설정된 것을 확인했다.