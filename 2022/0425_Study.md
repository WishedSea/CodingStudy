### 기능개발 (Python3)
> 각 기능은 진도가 100%일때 서비스에 반영 가능하다.  
> 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있으나 앞의 기능과 함께 배포한다.  
> 입력값은 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 speeds가 주어질때 각 배포마다 몇개의 기능이 배포되는지 return 하라.  
>> 입력값
>>> progresses = [93, 30, 55]
>>> speeds = [1, 30, 5]
>> 출력값
>>> [2, 1]
>> 풀이
>>> ```python3
>>> def solution(progresses, speeds):
>>>    answer = []
>>>    days = 1
>>>    cnt = 0 # daily fin progress cnt
>>>    
>>>    while len(progresses) > 0:
>>>        if 100 > progresses[0] + days*speeds[0]:
>>>            if cnt != 0:
>>>                answer.append(cnt)
>>>            cnt = 0
>>>            days += 1
>>>            continue
>>>        else:
>>>            cnt += 1
>>>            progresses = progresses[1:]
>>>            speeds = speeds[1:]
>>>    answer.append(cnt)
>>>    
>>>    return answer
>>> ```

### 나머지가 1이 되는 수 찾기
> 입력값 정수 n
> ```python3
> def solution(n):
>    x = 2
>    while n-1 > x:
>        if (n-1) % x == 0:
>            return x
>        else:
>            x += 1
>    return x
> ```

### 이름이 있는 동물의 아이디(SQL)
> 동물 보호소에 들어온 동물 중, 이름이 있는 동물의 ID를 조회하는 SQL문을 작성하라.  
> 단, ID는 오름차순 정렬되어야 한다.
>> ```mysql
>> select ANIMAL_ID
>> from ANIMAL_INS
>> where NAME is not null
>> order by ANIMAL_ID;
>> ```
