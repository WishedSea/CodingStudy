## IPTABLES
> - 리눅스상에서 방화벽을 설정하는 도구
> - 커널 2.4 이전 버전에서 사용되던 IPCHAINS를 대신하는 도구
> - 커널상에서 netfilter 패킷필터링 기능을 사용자 공간에서 제어하는 수준으로 사용가능
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
>>    <td>net</td>
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
> ### 매치 옵션
> ### 타겟
> ### 연결 추적
> ### 명령어 옵션
>> |명령어|설명|
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
> ### iptables 출력
