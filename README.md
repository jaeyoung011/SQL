# SQL

#### 참조 : 코드잇 , Leetcode, 블로그  (추가 참조시 추가)

Practice_SQL

문법 정리


# 1. 이스케이핑(escaping) 문제
Like 문

'문자로서의 %'를 나타내려면 어떻게 해야 할까?

### 1) ' (작은따옴표) 이스케이핑 
- '%\'%'

### 2) _(언더바) 이스케이핑
- '%\_%'

### 3) “(큰따옴표) 이스케이핑
- '%\"%\"%'

### 4) % 이스케이핑
- '%\%%'


# 정규식 관련 문법
- https://velog.io/@gillog/MySQL-REGEXPRegular-Expression%EC%A0%95%EA%B7%9C-%ED%91%9C%ED%98%84%EC%8B%9D
- https://wikidocs.net/1642
- https://dev.mysql.com/doc/refman/8.0/en/regexp.html#operator_regexp


#2 NULL 값들의 활용

# 2. NULL 값들의 활용

```

select * from copang_main.member
    WHERE height IS NULL
    OR weight IS NULL
    OR address IS NULL;
```


- 이렇게 빠져있는 고객 들한테 채워달라는 메일을 써달라 할수 있다.

### COALESCE

```
select
    COALESCE(height, '####')
    COALESCE(weight, '----')
    COALESCE(address, '@@@')
FROM copang_main.member;
```

- COALESCE -> NULL 이 있으면 저단어로 대체 해라.


# 3.데이터 전처리

### 예시1

```
### 나이 전처리 
SELECT AVG(age) FROM copang_main.member
WHERE age BETWEEN 5 AND 100;

### 주소가 이상할때
SELECT address FROM ccopang_main.member
WHERE address NOT LIKE '%호'
```


# 4. CONCAT

```
select
    email,
    height AS 키,
    weight AS 몸무게,
    weight / ((height/100) * (height/100)) as BMI
    FROM copang_main.member;
    
# CONCAT 함수를 써서 컬럼들을 합해서 볼수도 있다.
select
    email,
    CONCAT(height, 'cm', ', ', weight, 'kg') AS '키와 몸몸무게',
    weight / ((height/100) * (height/100))
FROM copang_main.member
```
# 5. 컬럼의 값 변화해서 보기  CASE
- CASE 활용 하기

```
select
    email,
    CONCAT(height, 'cm', ', ', weight, 'kg') AS '키와 몸무게',
    weight / ((height/100) * (height/100)) as BMI,
    
(CASE
    WHEN weight IS NULL OR height IS NULL THEN '비만 여부 알 수 없음'
    WHEN weight / ((height/100) * (height/100)) >= 25 then '과체중 또는 비만'
    WHEN weight / ((height/100) * (height/100)) >= 18.5
        AND ((height/100) * (height/100)) < 25
        THEN '정상'
    ELSE '저체중'
END) AS obesity_check

FROM copang_main.member;
ORDER BY obesity_check ASC;
```

# 6. NULL을 다른 값으로 변환하는 다양한 함수들

### 1.COALESCE 함수
- 정규함수이다 , MYSQL 뿐만 아니라 다른 SQL 문법에도 적용 가능

```
SELECT COALESCE(weight , 'N/A') FROM tb1

# 다른 방식 사용
SELECT COALESCE(weight, weight * 2.5, 'N/A') FROM tb1
```

### 2. IFNULL 함수
```
SELECT IFNULL(weight, 'N/A') FROM tb1
# 그러니까 weight 컬럼이 NULL이면 'N/A'를 출력하고, NULL이 아니면 height 컬럼의 값을 그대로 출력하죠.
```

### 3. IF 함수
- IF 함수는 가장 첫 번째 인자로 어떤 조건식이 옵니다. 만약 그 조건식의 결과가 True라면 두 번째 인자를 리턴하고, False라면 세 번째 인자를 리턴.

```
SELECT IF(height IS NOT NULL, height, 'N/A') FROM tb1
```
### 4. CASE 함수

### 5 활용 
```
select name, price, price/cost, 
case when price/cost >=1 and price/cost <1.5 then 'C.저효율 메뉴'
    when price/cost >= 1.5 and price/cost < 1.7 then 'B.중효율 메뉴'
    when price/cost >=1.7 then 'A.고효율 메뉴'
end as 'efficiency'
from pizza_price_cost
order by efficiency DESC, price ASC
LIMIT 6
```

# 7.

