# Code-server on AWS

### 들어가며

대학 입학시절 필자의 생각은 학업을 마치자 마자 군대에 가야겠다 라는 생각을 가지고 있었다.  
하지만 대학을 다니면서 알아본 결과 '산업기능요원'제도를 알게되었고, 졸업 후 취업을 목적으로 산업기능요원을 뽑는 회사에 이력서를 넣어보았다.  
하지만 졸업식 날짜는 20년 초이고, 갑작스런 코로나 펜데믹으로 인해 모든 행사가 취소되었다.  
펜데믹 상황으로 인해 대부분의 모든 회사는 인원을 적게 뽑기 시작한 것 같다.  
조금 더 많은 공부를 하기 위해 군대를 택한 필자는 지난 21년 3월 군대에 입대하였다.  
군대를 입대하고 자대까지 배치를 받은 상황에서 공부를 하려니 너무 막막하였고, 이런저런 환경으로 인해 졸업후 지금까지 약 2년간 제대로 된 공부를 하지 못하고 있었다.  
지난 1개월간 사이버지식정보방에서 원격이 불가능하여 매번 IDE를 설치하여 운용하였으나, 설치가 번거롭고 백업을 하려고 해도 어려운 환경이었다.  
이를 극복하기 위해 군대에서 코딩공부를 한 사람을 찾아보아 어떻게 하면 좋을지 여러 블로그를 찾아보았고, AWS(Amazon Web Servie)가 가입 후 1년간 무료라는 소식을 접하고 AWS에 Code-server를 설치하여 운용해보려 한다.

### 컴퓨터 환경
사이버지식정보방은 다음과 같은 환경을 가지고 있다.
- 매 부팅시 서버에 저장된 초기값으로 부팅이 되는 MaestroWeb 에이전트
- 실행창을 띄우지 못함 (필자의 경우 윈도우에서 실행창을 이용하는 경우가 많았다.)
- 제어판, 윈도우 설정 사용 불가 (제어판을 실행시켰을 경우 다음과 같은 창이 뜬다)  
![restrictions_screenshot](/image/220415_001_restrictions.png)
- 포트 제한
  - 확인된 사용 불가능 포트
    - 3389 (RDP)
    - 22 (SSH)
    - 23 (Telnet)

### AWS란?
- Amazon Web Service는 Amazon.com에서 개발한 클라우드 컴퓨팅 플랫폼중 IssA이다.
- 비즈니스와 개발자가 웹 서비스를 사용하여 확장 가능하고 정교한 애플리케이션을 구축하도록 지원하여준다.
- 현재 소규모 법인(회사) 및 개인을 포함한 다양한 사용자들이 이용하고 있으며, 클라우드 컴퓨팅의 장점을 이용하기 위해 많은 거대 기업에서도 활용하고 있다.

### 클라우드 컴퓨팅이란?
[**인터넷을 통해** IT 리소스와 애플리케이션을 온디맨드로 제공하는 서비스, **종량 과금제**]
- 기존의 물리적인 형태의 실물 컴퓨팅 리소스를 네트워크 기반 서비스 형태로 제공하는 것.
- 사용자로 하여금 네트워크 상에서 클라우드 서비스의 자원을 사용하는 것을 의미한다.
- 다음과 같이 3가지 분류로 나누기도 한다.
  - IaaS (Infrastructure as a Service)
    - AWS와 같이 Infrastructure를 제공하는 서비스
    - 가상 서버 또는 스토리지, 가상 네트워크 등의 리소스를 서비스 형태로 제공
    - 사용자는 물리적인 하드웨어를 직접 관리할 필요가 없으며, 직접적으로 서비스 이용을 통해 컴퓨터 리소스를 사용할 수 있다.
  - PaaS (Platform as a Service)
    - DB 또는 Application 서버 등의 미들웨어를 제공
    - HW/OS/MW에 대한 관리는 서비스 제공자가 하며, 사용자는 제공된 MW만 사용할 수 있다.
    - 기본 인프라를 관리 할 필요없이 프로그램을 실행할 수 있게 해준다.
  - SaaS (Software as a Service)
    - 소프트웨어 또는 애플리케이션의 기능만 제공

### AWS환경
- OS : Ubuntu 20.14 LTS
- 키 페어 없이 진행
- Nginx를 통해 IP만을 통해 IDE에 접근
- 권장은 1GB RAM, 2CPUs 이지만 **무료인 1GB RAM, 1CPU를 사용하였다.**

### AWS인스턴스 생성
1. 메인화면에 솔루션 구축에서 가상 머신 사용을 클릭한다.  
![startVM](/image/220415_002_StartVM.png)
2. 서버명 설정, OS 이미지 선택
  - 이름 : 자신이 원하는 이름
  - 애플리케이션 및 OS 이미지(Amazon Machine Image) : Ubuntu
  ![VMImage](/image/220415_003_VMImage.png)
3. 키 페어(로그인) : 키 페어 없이 계속 진행(권장되지 않음)
4. 네트워크 설정 : 인터넷에서 HTTPs 트래픽 허용 추가 선택
  ![NetSet](/image/220415_004_NetSet.png)
5. 그 외에는 기본설정으로 진행하였다.
6. 인스턴스 시작

### Code-server 설치
1. 인스턴스 생성을 정상적으로 마쳤다면 다음과 같이 앞에서 만든 인스턴스가 실행중일 것이다.
  ![InstanceList](/image/220415_005_InstanceList.png) 
2. 생성한 인스턴스를 체크하고 연결버튼을 눌러 기본 사용자 이름인 ubuntu로 연결한다.
3. Code-server를 설치한다.
```
curl -fsSL https://code-server.dev/install.sh | sh
```
4. 인스턴스가 부팅이 될때 자동으로 켜질 수 있도록 등록한다.
```
sudo systemctl enable --now code-server@$USER
```
5. 위 명령어까지 실행하게 되면 Code-server는 동작중이지만 웹에서의 접근이 불가능한데 이는 localhost의 접속만 허용하기 때문이다. 외부에서 접속이 가능할 수 있도록 Nginx를 이용하여 설정하려 한다.
6. IDE 페이지 비밀번호 변경은 ~/.config/code-server/config.yaml에서 가능하다.
  - password: [사용할 비밀번호 입력]
```
sudo vi ~/.config/code-server/config.yaml
```

### Nginx 설치
1. Nginx를 설치한다.
```bash
sudo apt-get install nginx -y
```
2. /etc/nginx/sites-available/code-server 파일에 아래 설정 추가 (sudo 권한 필요)
```bash
server {
    listen 80;
    listen [::]:80;

    location / {
      proxy_pass http://localhost:8080/;
      proxy_set_header Host $host;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection upgrade;
      proxy_set_header Accept-Encoding gzip;
    }
}
```
3. 설정 적용
```
sudo rm /etc/nginx/sites-enabled/default
sudo ln -s ../sites-available/code-server /etc/nginx/sites-enabled/code-server
sudo systemctl reload nginx.service
```
설정이 제대로 적용되었다면 이후부터는 퍼블릭 IP 주소만을 입력하여 접근할 수 있다.
![VScodePage](/image/220415_006_VScodePage.png)

### Code-server 업데이트
업데이트는 설치때 사용한 명령어와 동일하다.
```bash
curl -fsSL https://code-server.dev/install.sh | sh
```

### 맺으며
필자는 원래 VSCode를 사용해 본적이 없다.  
주로 사용하던 Python IDE는 Spyder를 주로 사용하였고 그 마저도 없으면 Notepad++프로그램을 사용하였다.
익숙해 지는데 오래 걸릴지 모르지만 전역하는 그 날까지 계속해서 감을 잃지 않도록 노력해볼 계획이다.  
또한 아직 마크다운 문법에 대해 숙련도가 부족해 가독성이 떨어질 수 있다.  
이를 극복하기 위해 열심히 계속해서 공부할 것이다.

### 참고
AWS란? https://goddaehee.tistory.com/174 <br>
Code-server 설치 https://github.com/coder/code-server

