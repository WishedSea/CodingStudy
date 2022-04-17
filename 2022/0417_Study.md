## DataBase 기초

#### SELECT  
```MySQL
SELECT      -- 출력될 항목
FROM        -- 참조된 테이블 명
WHERE       -- 조건
GROUP BY    -- 그룹을 뭘로 묶을 것인가?
HAVING      -- 그룹 조건
ORDER BY    -- 정렬 조건
```

### MYSQL 
> #### 설명 
>> "ANIMAL_INS" 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블 입니다.  
>> "ANIMAL_INS" 테이블 구조는 다음과 같습니다.  
>> |NAME|TYPE|NULLABLE|의미|
>> |:---:|:---:|:---:|:---:|
>> |ANIMAL_ID|VARCHAR(N)|FALSE|아이디|
>> |ANIMAL_TYPE|VARCHAR(N)|FALSE|생물 종|
>> |DATETIME|DATETIME|FALSE|보호 시작일|
>> |INTAKE_CONDITION|VARCHAR(N)|FALSE|보호 시작 시 상태|
>> |NAME|VARCHAR(N)|TRUE|이름|
>> |SEX_UPON_INTAKE|VARCHAR(N)|FALSE|성별 및 중성화 여부|  
>
> #### Q1
>> 동물 보호소에 들어온 모든 동물의 아이디와 이름을 ANIMAL_ID 순으로 조회하는 SQL문을 작성해주세요.
>
> #### A1
>> ```MySQL
>> SELECT ANIMAL_ID, NAME  -- 아이디와 이름
>> FROM ANIMAL_INS         -- 참조된 테이블 명
>> ORDER BY ANIMAL_ID;     -- ANIMAL_ID 순(ACS 생략)
>> ```
>
> #### Q2
>> 동물 보호소에 가장 먼저 들어온 동물의 이름을 조회하는 SQL 문을 작성해주세요.
>
> #### A2
>> ```MYSQL
>> SELECT NAME                          -- 이름 
>> FROM ANIMAL_ID                       -- 참조된 테이블 명 
>> WHERE DATETIME=(SELECT MIN(DATETIME) -- 조건 : 가장 먼저 들어온 동물 
>>                 FROM ANIMAL_INS);    -- 서브쿼리 : DATETIME이 제일 빠른 것 
>> ```
>
> #### Q3
>> 가장 최근에 들어온 동물은 언제 들어왔는지 조회하는 SQL 문을 작성해주세요.
>
> #### A3
>> ```MYSQL
>> SELECT MAX(DATETIME) -- 최근이다 == 제일 크다 
>> FROM ANIMAL_INS;
>> ```
>
> #### Q4
>> 동물 보호소에 들어온 동물 중 고양이와 개가 각가 몇마리 인지 조회하는 SQL 문을 작성해주세요
>
> #### A4
>> ```MYSQL
>> SELECT ANIMAL_TYPE, COUNT(*)
>> FROM ANIMAL_INS
>> GROUP BY ANIMAL_TYPE
>> ORDER BY ANIMAL_TYPE;
>> ```
>
> #### Q5
>> 동물 보호소에 들어온 동물 이름 중 두번 이상 쓰인 이름과 해당 이름이 쓰인 횟수를 조회하는 SQL문을 작성해주세요. 이때 결과는 이름이 없는 동물은 집계에서 제외하며, 결과는 이름순으로 조회해주세요
>
> #### A5
>> ```MYSQL
>> SELECT NAME, COUNT(*)    -- 이름, 횟수
>> FROM ANIMAL_INS          -- 참조된 테이블
>> WHERE NAME IS NOT NULL   -- 이름이 없는경우 제외
>> GROUP BY NAME            -- 이름이 같은것끼리 묶음
>> HAVING COUNT(NAME) >= 2  -- 같은이름이 2번 이상
>> ORDER BY NAME;           -- 이름순
>> ```
