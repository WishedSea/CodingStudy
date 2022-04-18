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
