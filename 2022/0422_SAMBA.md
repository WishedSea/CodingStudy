### SAMBA
> Windows 운영체제를 사용하는 PC에서 Linux 또는 UNIX 서버에 접속하여 파일이나 프린터를 공유하여 사용할 수 있도록 해주는 소프트웨어  
> #### 설치 명령어
>> ```bash
>> ~$ sudo yum install samba
>> ~$ sudo yum install samba-common
>> ~$ sudo yum install samba-client
>> ```
> #### 설정 파일 (/etc/samba/smb.conf, 샘플파일: /etc/samba/smb.conf.example)
>> ***설정파일을 모두 설정한 이후에는 무조건 `testparm` 명령어를 실행시켜 conf파일에 문법적 오류가 없는지 확인한다.***  
>> 설정파일 내의 주석은 리눅스 주석인  ***#*** 와 윈도우 주석인  ***;*** 가 모두 허용된다.
>> 기본 설정파일 내용은 아래와 같다. [Default SMB Conf IMG](/image/220422_001_default_smb_conf.png)
>> ```bash
>> # See smb.conf.example for a more detailed config file or
>> # read the smb.conf manpage.
>> # Run 'testparm' to verify the config is correct after
>> # you modified it.
>> 
>> [global]                         # 삼바 서버의 전체적인 환경설정을 담당하는 섹션
>> workgroup = SAMBA                # 윈도우의 작업 그룹에 해당하는 항목으로 공유 그룹명을 지정
>> server string = Samba Server     # 서버의 설명을 설정
>> netbios name = MYSERVER          # 윈도우에서 이름으로 접속할 대 관련 이름을 지정
>> interface = lo eth0 x.x.x.x/24   # 네트워크 인터페이스 별로 접근을 제어할 때 설정
>> hosts allow = 127. 192.168.12.   # 접속을 허용할 호스트 
>> hosts deny = 172.                # 접속을 불허할 호스트 
>> log file = /var/log/samba/log.$m # 서버에 접속하는 호스트의 로그를 기록하는 파일 지정
>> max log size = 50                # 로그 파일의 최대 크기를 설정하는 부분(단위 KB) 값 초과시 .old라는 확장자를 가지는 파일로 저장 후 새 파일이 생성됨, 0일때 무제한 
>> 
>> security = user                  # 삼바 서버에 접속할 때 인증모드를 설정하는 항목
>>                                  # user : 접속시 사용자 ID, PW입력
>>                                  # domain 또는 ads : 윈도우 도메인 컨트롤러에 전달하여 유효한 사용자 여부를 확인할 대 사용
>> 
>> passdb backend = tdbsam          # security가 user일때 PW를 설정하는 방식 
>> 
>> printing = cups                   
>> printcap name = cups             
>> load printers = yes              
>> cups options = raw                
>> 
>> [homes]                          # 각 사용자들이 자신의 홈 디렉터리로 접근할 때의 권한을 설정하는 섹션
>> comment = Home Directories       # 간단한 설명
>> valid users = %S, %D%w%S         # 허용 유저 목록
>> browseable = No                  # 공유이름을 브라우저에 표시할 수 있게 하는 기능
>> read only = No                   # 쓰기설정
>> inherit acls = Yes               # Inheritance ACLs? (ACLs 상속?) 좀더 찾아볼것
>> 
>> [printers]                       # 프린터 관련 권한을 설정하는 섹션
>> comment = All Printers           
>> path = /var/tmp                  
>> printable = Yes                  
>> create mask = 0600               
>> browseable = No                  
>> 
>> [print$]                         
>> comment = Printer Drivers        
>> path = /var/lib/samba/drivers    
>> write list = @printadmin root    
>> force group = @printadmin        
>> create mask = 0664               
>> directory mask = 0775            
>> ```
>> 
> #### SMBCLIENT
>> 리눅스 및 유닉스에서 사용하는 삼바 클라이언트 명령으로 윈도 서버로 접근할 때 사용된다.
>> ```bash
>> ~$ smbclinet -L localhost
>> ````
>> 
### SAMBA는 실제로 문제를 풀어보며 공부를 해야할것 같다. 설치된 버전과 명령어가 잘 안맞는 것 같다.

