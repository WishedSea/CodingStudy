### 환경구축
> 1. AWS에 CeontOS7 인스턴스 생성(접근을 위해 키를 생성)
> 2. AWS S3객체에 Public으로 업로드(임시방편) 
> 3. Ubuntu에서 wget 명령어를 사용 하여 ssh.pem을 다운받음
> 4. 명령어 실행
> 
> ```bash
> ~$ ssh -i "ssh.pem" centos@[PUBLIC-DNS]
> ```
> #### 실행결과
> ![CentOS_version](/image/220418_001_CentOS_version.png)

### HTTP
> #### 주요 HTTP 요청 메소드
>> - GET    : 서버에서 자료를 가져오는 요청 
>> - POST   : 서버에 정보를 저장하는 요청
>> - HEAD   : HTTP헤더 정보만 가져오는 요청
>> - PUT    : 서버에 자료를 올릴때 사용
>> - DELETE : 서버의 자료를 삭제할 때 사용
>
> 방금 생성된 CentOS 서버에는 Apache가 설치되어 있지않아 기본적인 Apache HTTP Server를 설치해 주었다.(2022-04-18기준 설치된 Apache 버전: 2.4.6)
> ```bash
> ~$ sudo yum install httpd
> ```
> 기본 설정파일의 경로: /etc/httpd/conf/httpd.conf  
> Defalut 설정파일(Apache 2.2기준): [sample-httpd.conf](https://gist.github.com/gaspanik/4946172/)
> #### 설정 파일 내용
>> ```bash
>> ServerRoot "/etc/httpd"        # 웹 서버가 설치된 디렉터리
>> Listen 80                      # 웹 서버의 포트번호
>> Timeout 60                     # 클라이언트 요청에 대한 타임아웃 값(단위: 초)
>> KeepAlive On                   # 지속적인 접속을 허가할 것인지 말 것인지를 설정하는 부분
>>                                # 하나의 프로세스가 실행되었을 때 처음부터 끝까지 이 프로세스를 요청한 사용자에게 작업을 할 수 있도록 하는 것
>> MaxKeepAliveRequests 100       # KeepAlive가 설정되어 있을 때 접속기간동안 처리할 수 있는 요청 갯수.(0 = inf.)
>> KeepAliveTimeout 15            # KeepAlive가 설정되어 있을 때 클라이언트의 요청이 타임아웃 되는 시간(단위: 초)
>> PidFle run/httpd.pid           # 웹 서버 동작시 root 권한으로 실행되는 아파치 PID 저장 경로
>> ServerAdmin AAAA@NAME.EX       # 서버 관리자의 이메일 주소
>> ServerName www.example.com:80  # 웹서버의 도메인명이나 포트를 기입하는 곳
>> DocumentRoot "/var/www/html"   # HTML문서가 저장될 곳(기본값)
>> DirectoryIndex index.html      # "index.html"을 기본 페이지로 인식하게 한다.
>>                                # "MAIN.html"을 기본페이지로 인식하게 하려 할경우 "index.html"을 "MAIN.html"로 변경한다.
>> User apache
>> Group apache                   # 실행되는 httpd 데몬의 사용자 및 그룹 권한
>>
>> <Directory />                  # 전체 디렉터리에 대한 기본 옵션 및 권한
>> Option FollowSymLinks          # 디렉터리의 심볼릭 링크 사용 허용
>> AllowOverride None             # 유저 인증파일 미사용
>> </Directory>
>>
>> <Directory "/var/www/html">    # /var/www/html 디렉터리에 대한 옵션
>> Options Indexes FollowSymLinks # 지정된 인덱스 파일이 없을 경우 해당 디렉터리 파일 목록 출력
>> AllowOverride None             # 유저 인증파일 미사용
>> Order Allow,Deny               # 호스트 접근시 Allow 설정 확인 후 Deny 설정 확인
>> Deny from all                  # 모든 호스트의 접근 불허
>> Allow From 192.168.10          # 192.168.10.0/24의 호스트에 대한 접근 허가
>> </Directory>
>> 
>> <IfModule mod_userdir.c>       # 사용자 계정별 디렉터리 여부
>> UserDir disable
>> UserDir public_html
>> </IfModule>
>>
>> <IfModlue prefork.c>           # 클라이언트 요청이 들어올 경우를 대비하여 자식 프로세스 생성
>> StartServers 8                 # 웹 서버 가동시 처음에 실행시킬 프로세스의 수
>> MinSpareServers 5              # 최소로 유지해야할 여분의 프로세스 수
>> MaxSpareServers 5              # 최대로 제한 되어야할 여분의 프로세스 수
>> ServerLimit 256                # 서버 동작 중 처리할 수 있는 최대 클라이언트의 값
>> MaxClient 256                  # 시작할 수 있는 최대 서버 프로세스의 수
>> MaxRequestsPerChild 4000       # 자식 프로세스가 소별 전까지 처리할 수 있는 프로세스의 수 (0일때 inf)
>> </IfModule>
>>
>> <IfModule worker.c>            # 초기에 시작하는 프로세스 수 지정 후 페이지 요청시 쓰레드로 처리
>> StartServers 4                 # 웹 서버 가동시 처음에 실행시킬 프로세스의 수
>> MaxClients 300                 # 동시에 접속 할 수 있는 클라이언트의 수
>> MinSpareThreads 25             # 최소로 유지해야 할 여분의 쓰레드의 수
>> MaxSpareThreads 75             # 최대로 제한 되어야할 여분의 쓰레드의 수
>> ThreadsPerChild 25             # 하나의 프로세스가 처리할 수 있는 최대 쓰레드의 수
>> MaxRequestsPerChild 0          # 자식 프로세스가 소멸 전까지 처리할 수 있는 처리의 수 (0일때 inf)
>> </IfModule>
>>
>> LoadModule NAME_module modules/~~ # 동적 모듈 / 필요할때만 요청되는 모듈
>> ```
> #### 옵션 항목
>> |항목|설명|
>> |---|---|
>> |NONE|모든 접근 거부
>> |ALL|MultiViews 옵션을 제외한 모든 옵션 부여 (기본 값)
>> |includes|서버측의 추가적인 정보를 제공|
>> |indexes|지정된 파일이 없을 경우 디렉터리의 파일 목록 출력|
>> |FollowSymLinks|디렉터리의 심볼릭 링크 사용 허용|
>> |ExecCGI|CGI 스크립트 실행 허용|
>> |IncludesNoEXEC| SSI는 허용하지만 EXEC명령과 CGI 스크립트 #include 불허
>> |MultiViews|all 옵션시 지정된 목록의 Multiviews 허용. 유사한 파일의 이름을 찾아주는 기능|
> #### AllowOverride 항목 (AcessFileName 지시자)
>> |항목|설명|
>> |---|---|
>> |NONE|유저 인증파일 미사용|
>> |ALL|지시자로 설정한 파일 사용 및 관련 지시자 사용|
>> |AuthConfig|사용자 인증 지시자 사용 허용|
>> |FileInfo|문서 유형 제어 지시자 사용 허용|
>> |Indexes|디렉터리 인덱싱을 제어하는 지시자 사용 허용|
>> |Limit|호스트 접근을 제어하는 지시자 사용 허용|
>> |Options|Options와 XBiHack과 같은 지시자 사용 허용|



참고자료 1: https://2mukee.tistory.com/282  
참고자료 2: [북스홀릭] 최신 기출문제를 수록한 리눅스마스터 1급 2차 실기 정복하기
