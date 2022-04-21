## IPTABLES
> - 리눅스상에서 방화벽을 설정하는 도구
> - 커널 2.4 이전 버전에서 사용되던 IPCHAINS를 대신하는 도구
> - 커널상에서 netfilter 패킷필터링 기능을 사용자 공간에서 제어하는 수준으로 사용가능
> ### IP CHAIN 구성도
>  ![IP CHAIN 구성도]('https://webterror.net/wp-content/uploads/2015/02/IP_CHANE_%EA%B5%AC%EC%84%B1%EB%8F%84-1024x688.jpg')
> ### 패킷필터링이란?
>> 지나가는 패킷의 헤더를 보고 그 전체 패킷을 어떻게 처리할지 결정하는 것
> ### IPTABLES 관련 명령어
>> #### 설치 및 버전 확인 명령어
>>> ```bash 
>>> ~$ yum install iptables -y # 설치 명령어
>>> ~$ rpm -qa iptables        # 버전 확인 명령어
>>> ```
>>> #### 결과값
>>> ```bash
>>> sendmail-8.14.7-6.el7.x86_64
>>> ```
>> #### 상태 확인 명령어
>>> ```bash
>>> ~$ chkconfig --list
>>> ```
>>> #### 결과값
>>> ![iptables_chkconfig_result](/image/220420_001_iptables_chkconfig_result.png)
> ### 테이블 종류
>> <table style="text-align='center'"> 
>>  <tr>
>>    <td rowspan='2'>사슬(Chain)</td>
>>    <td colspan='4'>테이블(Table)</td>
>>  </tr>
>>  <tr>
>>    <td>filter</td>
>>    <td>nat</td>
>>    <td>mangle</td>
>>    <td>raw</td>
>>  </tr>
>>  <tr>
>>    <td>INPUT</td>
>>    <td>O</td>
>>    <td>O</td>
>>    <td>O</td>
>>    <td></td>
>>  </tr>
>>  <tr>
>>    <td>FORWORD</td>
>>    <td>O</td>
>>    <td></td>
>>    <td>O</td>
>>    <td></td>
>>  </tr>
>>  <tr>
>>    <td>OUTPUT</td>
>>    <td>O</td>
>>    <td>O</td>
>>    <td>O</td>
>>    <td>O</td>
>>  </tr>
>>  <tr>
>>    <td>PREPOUTING</td>
>>    <td></td>
>>    <td>O</td>
>>    <td>O</td>
>>    <td>O</td>
>>  </tr>
>>  <tr>
>>    <td>POSTROUTING</td>
>>    <td></td>
>>    <td>O</td>
>>    <td>O</td>
>>    <td></td>
>>  </tr>
>> </table>  
> ### FILTER 테이블의 사슬(Chain) 종류
>> |사슬(Chain)|기능|
>> |---|---|
>> |INPUT|<ul><li>패킷필터링 및 방화벽 관련 정책들을 설정하는 사슬</li><li>실제적인 접근 통제를 담당하는 역할을 수행</li><li>커널 내부에서 라우팅 계산을 마친 후 로컬 리눅스 시스템이 목적지인 패킷에 적용한다.</li></ul>|
>> |OUTPUT|<ul><li>다른 시스템으로의 접근을 차단할 때 사용하는 사슬</li><li>리눅스 시스템 자체가 생성하는 패킷을 제어하는 사슬</li></ul>|
>> |FORWARD|<ul><li>리눅스 시스템을 통과하는 패킷을 관리하는 사슬</li><li>한 네트워크를 다른 네트워크와 연결하기 위해 iptables의 방화벽을 사용해서 두 네트워크 간의 패킷이 방화벽을 통과하는 경우에 사용</li><li>NAT 기반으로 하나의 공인 IP를 사용하는 시스템의 접근 제어 정책을 설정할때 사용</li></ul>|
> ### NAT(Network Address Translation) 테이블의 사슬(Chain) 종류
>> |사슬(Chain)|기능|
>> |---|---|
>> |PREROUTING|DNAT(Destination NAT) => 외부 공인 IP를 내부 사설 IP로|
>> |POSTROUTING|SNAT(Source NAT) => 내부 사설 IP를 외부 공인 IP로|
> ### 매치 옵션
>> 패킷을 처리할 때 만족해야 하는 조건을 가르킴 => 이 조건을 만족시키는 패킷들만 규칙을 적용
>> |옵션|설명|
>> |---|---|
>> |-s(--source)|출발지 IP주소나 네트워크와 매칭|
>> |-d(--destination)|목적지 IP주소나 네트워크와 매칭|
>> |-p(--protocol)|특정 프로토콜과 매칭|
>> |-i(--in-interface)|입력 인터페이스와 매칭|
>> |-o(--out-interface)|출력 인터페이스와 매칭|
>> |--state|연결상태와 매칭|
>> |--string|애플리케이션 계층 데이터 바이트 순서와의 매칭|
>> |--comment|커널 메모리 내의 규칙과 연계되는 최대 256바이트 주석|
>> |-y(--syn)|SYN패킷을 허용하지 않는다|
>> |-f(--fragment)|두 번째 이후의 조각에 대해서 규칙을 명시|
>> |-t(--table)|처리될 테이블|
>> |-j(--jump)|규칙에 맞는 패킷을 어떻게 처리할 것인가를 명시|
>> |-m(--match)|특정 모듈과의 매치|
> ### 타겟
>> 패킷이 규칙과 일치할 때 동작을 취하는 것
>> |타겟|설명|
>> |---|---|
>> |ACCEPT|패킷을 받아들임|
>> |DROP|패킷을 전송된 적이 없던 것처럼 버림|
>> |REJECT|패킷을 버리고 이와 동시에 적절한 응답 패킷을 전송|
>> |LOG|패킷을 syslog에 기록|
>> |RETURN|호출 체인 내에서 패킷 처리를 계속|
>> REJECT -> 응답패킷: connection refused(연결 거부)
>> DROP -> 응답패킷: 없음.
> ### 연결 추적
>> - 네부 내트워크 상 서비스 연결 상태에 따라서 연결을 감시하고 제한 가능  
>> - 다음과 같은 연결 상태에 따라서 시스템 관리자가 연결을 허용하거나 거부 가능  
>> 
>> |연결 상태|설명|
>> |---|---|
>> |NEW|새로운 연결을 요청하는 패킷, 예) HTTP 요청|
>> |ESTABLISHED|기존 연결의 일부인 패킷|
>> |RELATED|기존 연결에 속하지만 새로운 연결을 요청하는 패킷, 예를 들면 접속 포트가 20인 수동 FTP의 경우 전송 포트는 사용되지 않는 1024 이상의 어느 포트라도 사용 가능하다.|
>> |INVALID|연결 추적표에서 어디 연결에도 속하지 않은 패킷|
>> 
>> - 상태에 기반한 iptables 연결 추적 기능은 어느 네트워크 프로토콜에서나 사용가능  
>> - UDP와 같은 상태를 저장하지 않는 프로토콜에서도 사용   
> ### 명령어 옵션
>> |옵션|설명|
>> |---|---|
>> |-A (--append)|새 규칙 추가|
>> |-D (--delete)|규칙 제거|
>> |-C (--check)|패킷 테스트|
>> |-R (--replace)|규칙 변경|
>> |-I (--insert)|규칙 삽입|
>> |-L (--list)|규칙 출력|
>> |-F (--flush)|chain으로부터 규칙을 모두 삭제|
>> |-Z (--zero)|모든 chain의 패킷과 바디트 카운터 값을 0으로 만듦|
>> |-N (--new)|새 chain 추가|
>> |-X (--delete-chain)|chain 삭제|
>> |-P (--policy)|기본정책 변경|
> ### 기본 동작
>> 1. 패킷에 대한 동작은 위에서부터 차례대로 각 규칙에 대해 검사 후 그 규칙과 일치하는 패킷에 대하여 타겟에 지정한 ACCEPT, DROP 등을 수행
>> 2. 규칙이 일치하고 작업이 수행되면, 그 패킷은 해당 규칙의 결과에 따라 처리하고 체인에서 추가 규칙을 무시
>> 3. 패킷이 체인의 모든 규칙과 매치하지 않아 규칙의 바닥에 도달하면 정해진 기본정책 수행
>> 4. 기본 정책은 policy ACCEPT, policy DROP으로 설정 가능
>>
>> 일반적인 기본 policy는 DROP으로 설정하고 특별히 지정된 포트와 IP주소등에 대해 ACCEPT를 수행
>> 
> ### iptables 출력
> ### 예문
>> #### DNS 서버 허용
>> ```bash
>> iptables -A INPUT -p tcp --dport 53 -j ACCEPT # tcp 53번 포트 허용
>> iptables -A INPUT -p udp --dport 53 -j ACCEPT # udp 53번 포트 허용
>> #### NULL 패킷 차단
>> ```bash
>> iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP # tcp프로토콜로 들어오는 패킷 전체가 NONE(NULL) 값으로 매칭되는 패킷은 DROP한다.
>> ```
>> #### SYN-FLOOD 공격 차단
>> ```bash
>> iptables -A INPUT -P tcp ! --syn -m state -state NEW -j DROP
>> ```
>> /etc/sysctl.conf를 수정하여 차단가능
>> ```bash
>> net.ipv4.tcp_syncookies = 1 
>> net.ipv4.conf.all.rp_filter = 1 
>> net.ipv4.conf.default.rp_filter = 1 
>> net.ipv4.tcp_max_syn_backlog = 8192 
>> net.ipv4.netfilter.ip_conntrack_max = 1048576
>> ```
>> #### XMAS 패킷 차단
>> ```bash
>> iptables -A INPUT -p tcp --tcp-flags ALL ALL -j DROP # tcp 프로토콜로 들어오는 모든 패킷이 모든 방식으로 들어올 경우 차단한다.
>> ```
>> #### 기타 사용법
>> ```bash 
>> iptables -nL --line-number                          # 실행 순번 확인
>> iptalbes -R INPUT 3 -p tcp --DPORT 2222 -j ACCEPT   # 순번 3의 행을 R(replace) 수정
>> iptables -A INPUT -i lo -j ACCEPT                   # loopback interface 모든 패킷 허용
>> iptables -A INPUT -i eth0 -j ACCEPT                 # eth0 interface 모든 패킷 허용
>> iptables -A INPUT -s 192.168.0.1 -j ACCEPT          # 192.168.0.1에 대해 모든 패킷 허용
>> iptables -A INPUT -s 192.168.0.0/24 -j ACCEPT       # 192.168.0.0 대역대에 대해 모든 패킷 허용(prifix대신 subnet mask값 적어도 됨)
>> # 신뢰하는 IP(192.168.0.1)와 MAC주소(00:00:00:00:00:00)에 대해 모든 패킷 허용
>> iptables -A INPUT -s 192.168.0.1 -m mac --mac-source 00:00:00:00:00:00 -j ACCEPT
>> iptables -A INPUT -p tcp -dport 1024:2048 -j ACCEPT # tcp 1024~2048포트에 대해 모든 패킷 허용
>> ```
>> 
>> #### 자동화 스크립트 (자주 수정하는경우 기본 설정으로 짜둘 수 있음)
>> ```bash
>> #!/bin/bash
>> # iptables 설정 자동화 스크립트
>> # 모든 Chain 정책 삭제
>> iptables -F
>> 
>> TCP 포트 22번을 SSH 접속을 위해 허용(원격 접속을 허용하기 위해 제일 먼저 설정)
>> iptables -A INPUT -p tcp -m tcp -dport 22 -j ACCEPT
>> 
>> # 기본 정책 설정
>> iptables -P INPUT DROP
>> iptables -P FORWARD DROP
>> iptables -P OUTPUT ACCEPT
>> 
>> # localhost 접속 허용
>> iptables -A INPUT -i lo -j ACCEPT
>>
>> established and relate 접속을 허용
>> iptables -A INPUT -m status --status ESTABLISHED,RELATE -j ACCEPT
>> 
>> # 80번 Apache포트 허용
>> iptables -A INPUT -p tcp -m tcp -dport 80 -j ACCEPT
>> 
>> # 설정 저장
>> /sbin/service iptables save
>> 
>> # 설정 내용 출력
>> iptables -L -v
>> ```


#### 출처 
> [IP Chain 구성도](https://webterror.net/2015/02/11/1622/)
> [[CentOS] 방화벽 설정 - iptables](https://webdir.tistory.com/170)
> [PREROUTING과 POSTROUTING](http://forum.falinux.com/zbxe/index.php?mid=lecture_tip&document_srl=872809)
