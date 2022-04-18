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
>> ServerRoot "/etc/httpd"       # 웹 서버가 설치된 디렉터리
>> Listen 80                     # 웹 서버의 포트번호
>> Timeout 60                    # 클라이언트 요청에 대한 타임아웃 값(단위: 초)
>> KeepAlive On                  # 지속적인 접속을 허가할 것인지 말 것인지를 설정하는 부분
>>                               # 하나의 프로세스가 실행되었을 때 처음부터 끝까지 이 프로세스를 요청한 사용자에게 작업을 할 수 있도록 하는 것
>> MaxKeepAliveRequests 100      # KeepAlive가 설정되어 있을 때 접속기간동안 처리할 수 있는 요청 갯수.(0 = inf.)
>> KeepAliveTimeout 15           # KeepAlive가 설정되어 있을 때 클라이언트의 요청이 타임아웃 되는 시간(단위: 초)
>> PidFle run/httpd.pid          # 웹 서버 동작시 root 권한으로 실행되는 아파치 PID 저장 경로
>> ServerName www.example.com:80 # 웹서버의 도메인명이나 포트를 기입하는 곳
>> DocumentRoot "/var/www/html"  # HTML문서가 저장될 곳(기본값)
>> DirectoryIndex index.html     # "index.html"을 기본 페이지로 인식하게 한다.
>>                               # "MAIN.html"을 기본페이지로 인식하게 하려 할경우 "index.html"을 "MAIN.html"로 변경한다.
>> 
>> ```

참고자료 1: https://2mukee.tistory.com/282  
참고자료 2: [북스홀릭] 최신 기출문제를 수록한 리눅스마스터 1급 2차 실기 정복하기
