# CentOS yum 사용법

yum(Yellowdog Updater Modified) 명령어는 CentOS에서 패키지 관리를 하는 명령어이다.

## 명령어

- 기본 명령어 구조

```markdown
yum [옵션] [명령] [패키지명]
```

- 명령어 도움말 보기

```markdown
yum -h
```

- 해당 패키지 설치

```markdown
yum install [패키지명]
```

- 해당 패키지 업데이트

```markdown
yum update [패키지명]
```

- 해당 패키지 목록 보기

```markdown
yum list
```

- 해당 패키지 삭제

```markdown
yum erase [패키지명]
```

# yum 동작 방식 및 저장소 관리

/etc/yum.conf와 /etc/yum.repos.d/ 디렉토리를 설정 파일로 참조한다.

일반적으로 yum.conf는 특별히 설정할 것이 없다.

yum.repos.d 디렉터리 하위에 있는 파일들이 중요 설정 파일이다.

왜냐면, 이 디터리 안에 패키지 저장소의 서버 정보가 담겨있기 때문이다.

### yum 동작 흐름

1. 명령어 입력
2. yum.repos.d 디렉토리에서 설정파일을 통해 저장소 서버 주소를 확인
3. 패키지 목록 파일 요청
4. 패키지 목록 파일 다운로드
5. 설치 여부 판단
6. 패키지 다운로드 및 설치

### CentOS-Base.repo 파일 구조

```bash
[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#released updates
[updates]
...생략...

#additional packages that may be useful
[extras]
...생략...

#additional packages that extend functionality of existing packages
[centosplus]
...생략...
enabled=0
```

[base]

원본 패키지 저장소를 의미한다.

원본 패키지는 CentOS가 릴리즈 되는 시점에 제작된 버전의 패키지를 의미한다.

배포판을 처음 설치할때 같이 설치되는 패키지들이 이곳에 저장되고 DVD 이미지에 담겨있다.

[updates]

이후 버그 수정이나 기능 수정 등으로 업데이트 된 패키지의 경우 업데이트 패키지 저장소에 별도로 저장관리된다.

yum 은 패키지를 설치할때 기본적으로 updates 저장소를 이용한다.

[extras], [centoplus]

추가로 배포된 패키지인경우 extras에서 관리한다.

centoplus 항목이 존재하지만 기본적으로 enabled=0으로 설정이 꺼져있고 사용하지 않는다.

### CentOS-Base.repo 설정 구성

```bash
[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

name : 저장소의 이름

mirrorlist : baseurl 속성이 생략된 경우 이곳에 명시된 url을 사용한다. 기본적으로 CentOS 공식 서버 url이 지정되어있다.

baseurl : 패키지 저장소의 url이며, http, ftp, file 프로토콜을 사용가능하다.

gpgcheck : GPG(GNU Privacy Guard) 키가 들어있는 저장소의 url을 적는다. 패키지를 인증하는데 사용하는 암호화 서명이다.

enabled : 이 저장소 설정 여부를 사용할 것인지 여부를 지정한다.