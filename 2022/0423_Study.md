## 코딩테스트 연습

1. 배열의 i번째부터 j번째까지 자르고 정렬했을때 k번째 있는 수 구하기
> |입력변수|입력값|
> |---|---|
> |array|[1, 5, 2, 6, 3, 7, 4]
> |commands|[[2, 5, 3]]|
> 결과값 : 5
> 
> ```python3
> def solution(array, commands):
>   result = []
>   for i, j, k in commands:
>     result.append(sorted(array[i-1:j])[k-1])
>   return result
> ```
> ### 다른 풀이
> ```python3
> def solution(array, commands):
>   return list(map(lambda x:sorted(array[x[0]-1:x[1]])x[2]-1), commands))
> ```

2. 0또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수구하기
> 입력변수 : numbers -> [6,10,2]
> 결과값 : "6210"
> 
> ```python3
> def solution(numbers):
>   def solution(numbers):
>     return str(int(''.join(s for s in sorted(map(str, numbers), key=lambda x: x*3, reverse=True))))
> ```
> #### 풀이 설명
>> 입력값이 [9, 99, 999, 90, 990, 900]일때 문자열로 치완후 3회 반복했을때 문자열로 정렬이 되기 때문에 ['9', '99', '999', '990, '90', '900']으로 정렬된다.  
>> 마지막 str(int())는 '0000~0000'인 경우가 있을수 있기 때문에 0으로 만들기 위함이다.
