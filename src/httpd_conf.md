# httpd.conf

```bash
ServerRoot “/etc/httpd”
```

- 디렉터리 트리의 최상위를 말한다.
- 여기에 서버의 설정, 에러, 로그 파일들이 저장된다.
- 경로 마지막에 “/”를 추가하면 안된다.

```bash
Listen 80
```

- 특정 IP주소와 포트를 묶어주는 역할을 한다.

```bash
Dynamic Shared Object (DSO) Support
```

- Apache와 호환되는 모듈을 로드하는 지시자이다.

```bash
Include conf.modules.d/*/.conf
```

- Apache는 설정 파일을 분산시켜 저장하여 필요한 설정 파일만 로드할 수 있다.
- 이때 Include를 사용한다.

```bash
User apache
Group apache
```

- httpd가 실행될때 사용하는 이름

```bash
‘Main’ server configuration
```

- 메인 서버에
- virtual host를 생상할 때 이 메인 서버 설정들이 각 virtual host의 기본값으로 상속된다.

```bash
ServerAdmin root@localhost
```

- 서버 내부 오류가 발생하면 띄우는 안내문에 출력될 서버 관리자 연락처
- 주로 이메일 주소를 적는다.

```bash
ServerName www.example.com:80
```

- 기본적으로 주석 처리되어있다.
- 주석을 해제하고 지정하는 것을 권장한다.
- 서버를 식별하기 위한 호스트 이름을 지정하는 설정.
- IP주소를 대신 적을 수 있다.

```bash
<Directory {디렉터리 명}>
		AllowOverride none
    Require all denied
</Directory>
```

- 각 디렉터리에 고유한 설정을 적용하기 위한 블록이다.
- 디렉터리 명은 서버 자체의 실제 디렉터리 절대 경로를 적는다.
- 별도 설정이 없으면 자식 디렉터리들도 속성을 상속받는다.
- AllowOverride none, Required all denied
    - 모든 디렉터리의 접근을 금지
    - Apache는 모든 디렉터리의 접근을 금지시키고 서비스를 제공할 디렉터리만 새로 설정하여 접근을 허용시킨다.
    - AllowOverride
        - 해당 디렉터리 설정 내용을 별도의 외부파일에서 재설정 또는 덮어쓸 수 있는지 결정
    

```bash
DocumentRoot “/var/www/html”
```

- 웹 페이지의 루트를 지정한다.
- 모든 요청은 여기에 지정한 디렉토리로 간다.
- 심볼릭 링크나 aliases를 통해 다른 위치로 보낼 수 있다.

```bash
<Directory “/var/www”>
	AllowOverride None
	Require all granted
</Directory>
```

- var 밑 www라는 디렉터리의 설정
- 설정 덮어쓰기 불가
- 모든 접근 허용

```bash
<Directory “/var/www/html”>
	Options Indexes FollowSymLinks
	AllowOverride None
	Require all granted
</Directory>
```

- Options에는 다음과 같은 인자가 올 수 있다.
- Indexes : 디렉터리 경로가 요청되고 DirectoryIndex에 맞는 파일이 없으면 해당 디렉터리의 목록을 출력한다.
- FollowSymLinks : 해당 디렉터리 내에서 심볼릭 링크를 가능하게 한다.
- Includes : SSI(Server Side Include)를 허용한다.
- ExecCGI : CGI 가능
- All : MultiViews 옵션을 제외한 모든 옵션을 적용한다.
- None : 어떠한 옵션도 적용하지 않는다.
- [더 많은 Options](https://httpd.apache.org/docs/2.4/mod/core.html#servername)

```bash
<IfModule dir_module>
    DirectoryIndex index.html
</IfModule>
```

- DirectoryIndex → 요청에서 파일명이 생략되었을 때, 웹 서버가 기본적으로 찾게 되는 파일을 정의한다.
- 여러 파일명이 들어갈 수 있다.

```bash
<Files “.ht*”>
	Require all denied
</Files>
```

- Files에서는 파일에 대한 접근 설정을 정의한다.
- .ht로 시작하는 모든 파일에 접근을 제한했다.

```bash
ErrorLog “logs/error_log”
```

- 에러 로그 파일의 위치를 지정한다.
- virtual host들에 따로 에러 로그 위치를 지정하지 않으면 여기에 저장이 된다.

```bash
LogLevel warn
```

- error log에 남겨지는 로그의 숫자를 지정하는 설정이다.

```bash
<IfModule log_config_module>
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    LogFormat "%h %l %u %t \"%r\" %>s %b" common

    <IfModule logio_module>
      
      LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
    </IfModule>

    CustomLog "logs/access_log" combined
</IfModule>
```

log_config_module이 있으면 실행된다.

LogFormat 맨 뒤에 작성되어 있는 문장이 포맷의 이름이 된다.

| 포맷 | 의미 |
| --- | --- |
| %h | 원격호스트 명 |
| %l | 원격 로그인 명 |
| %u | 원격 사용자 |
| %t | 요청 시간 |
| %r | 요청 메시지의 첫번째 라인 |
| %>s | 최종 상태 |
| %b | 응답 바이트 수 |
| %{Refer}i | 레퍼러 정보 |
| %{User-Agent}i | 사용자의 브라우저 정보 |

```bash
<IfModule mime_module>
    TypesConfig /etc/mime.types

    AddType application/x-compress .Z
    AddType application/x-gzip .gz .tgz

    AddType text/html .shtml
    AddOutputFilter INCLUDES .shtml
</IfModule>
```

웹서버는 이미지나 동영상 등 다양한 파일을 처리할 수 있다.

MIME 타입 및 확장자의 조합은 ‘/etc/mime.types’에 등록되어 있다.

AddType을 통해서 파일 확장자를 추가 할 수 있다.

```bash
ErrorDocument
```

- 오류 페이지를 출력할 때 사용자 정의 페이지를 보여주기 위해서 사용한다.

```bash
EnableSendfile
```

- 운영체제가 sendfile을 지원하면 아파치에서 전송할 파일을 직접 읽지 않을 수 있다.
- 이렇게되면, read와 send를 따로 할 필요가 없어서 아파치 성능이 매우 빨라진다.
- 그러나 sendfile을 사용하면 웹서버의 안정성이 떨어진다.
- 기본 설정값은 on이다.

```bash
<IfModule mod_http2.c>
    Protocols h2 h2c http/1.1
</IfModule>
```

- mod_http2.c → HTTP2를 기본으로 활성화 시킨다.

```bash
IncludeOptional conf.d/*.conf
```

- /etc/httpd/conf.d 디렉터리에 설정 파일이 있으면 불러온다.