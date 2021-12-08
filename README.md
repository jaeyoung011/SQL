# SQL
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

# 6. 






