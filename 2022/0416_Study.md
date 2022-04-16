## 프로그래머스 1단계 스킬체크 문제
1. 숫자가 들어있는 배열을 입력받아 평균값 계산
```python3
def solution(arr):
  return sum(arr)/len(arr)
```

2. 입력 값을 조건에 맞춰서 출력  
  조건 1. 영문자는 소문자로 변경  
  조건 2. 숫자, 영어와 특수문자 ' - ', ' _ ', ' . ' 만 허용  
  조건 3. '.' 이 여러개가 연속될 수 없음  
  조건 4. '.'은 처음과 끝에 올 수 없음  
  조건 5. 공백이라면 'a' 추가  
  조건 6. 15자 이하, '.'은 끝에 올 수 없음  
  조건 7. 3글자 이하라면 마지막 자리에 있는 글자를 3글자가 될때까지 추가  
```python3
def S2(s1): #조건 2. 숫자, 영어와 특수문자 ' - ', ' _ ', ' . ' 만 허용  
    arrS2 = ''
    for i in s1:
        if str.isalnum(i) or i=='-' or i=='_' or i=='.':
            arrS2 += i
    return arrS2

def S3(s2): #조건 3. '.' 이 여러개가 연속될 수 없음
    arrS3 = ''
    for i in s2:
        if len(arrS3) == 0:
            arrS3 += i
            continue
        if arrS3[-1] == '.' and i == '.':
            pass
        else:
            arrS3 += i
    return arrS3

def S4(s3): #조건 4. '.'은 처음과 끝에 올 수 없음
  return s3.strip('.')
"""
    while(True):
        if s3[0] == '.':
            s3 = s3[1:]
        else:
            break
        if len(s3) == 0:
            return ''
"""

    while(True):
        if s3[-1] == '.':
            s3 = s3[:-1]
        else:
            break
        if len(s3) == 0:
            return ''
    return s3

def S5(s4): #조건 5. 공백이라면 'a' 추가
    if len(s4) == 0:
        return 'a'
    return s4

def S6(s5): #조건 6. 15자 이하, '.'은 끝에 올 수 없음
    if len(s5) >= 16:
        s5 = s5[:15]
    return s5.rstrip('.')
"""
    while(True):
        if s5[-1] == '.':
            s5 = s5[:-1]
        else:
            break
    return s5
"""

def S7(s6): #조건 7. 3글자 이하라면 마지막 자리에 있는 글자를 3글자가 될때까지 추가
    if 2 >= len(s6):
        while 2 >= len(s6):
            s6 += s6[-1]
    return s6

def solution(new_id):
    s1 = str.lower(new_id) # 조건 1.알파벳은 소문자만
    s2 = S2(s1)
    s3 = S3(s2)
    s4 = S4(s3)
    s5 = S5(s4)
    s6 = S6(s5)
    return S7(s6)
```
  
