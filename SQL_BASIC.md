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

### 5.  활용 
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

# 7. DISTINCT
- SUBSTRING 함수를 잘 활용을 해야한다.

```
# 의미가 없다.
select DISTINCT(address) from copang_main.member;
```

### 주요 지역들의 고유값을 보고 싶으면 
```
select DISTINCT(SUBSTRING(address, 1, 2)) from copang_main.member;
# address 컬럼에서 1번째부터 2글자를 추출해라. 그러면 첫 두글자를 기준으로 distinct 가 된다.
```

# 8. 문자열 관련 함수들

### 1. LENGTH 함수
- LENGTH 함수는 문자열의 길이를 구해줍니다.

### 2. UPPER, LOWER 함수
- UPPER는 문자열을 모두 대문자로 바꿔서 보여주는 함수이고, LOWER는 문자열을 모두 소문자로 바꿔서 보여주는 함수입니다.

### 3. LPAD, RPAD 함수
- 이 두 함수는 문자열의 왼쪽 또는 오른쪽을 특정 문자열로 채워주는 함수입니다.
LPAD는 LEFT(왼쪽) + PADDING(채우기)의 줄임말, RPAD는 RIGHT(오른쪽) + PADDING(채우기)의 줄임말인데요.
예를 들어 LPAD(age, 10, ’0’)는 age 컬럼의 값을, 왼쪽에 문자 0을 붙여서 총 10자리로 만드는 함수입니다.
보통 어떤 숫자의 자릿수를 맞출 때 자주 사용하는 함수입니다. 아래 그림을 보면 무슨 뜻인지 바로 이해할 수 있습니다.
그런데 age 컬럼의 데이터 타입은 숫자를 나타내는 INT 형이었죠? 어떻게 숫자를 문자열 함수의 인자로 넣었는데 잘 작동한 걸까요?
비록 숫자이더라도 문자열 함수 안에 인자로 넣어주면 그 값이 자동으로 문자열로 형 변환이 되어 계산됩니다. 참고하세요.
RPAD 함수는 아래 그림처럼 LPAD 함수와 반대로 문자열의 오른쪽을 채워주는 함수입니다.


### 4. TRIM, LTRIM, RTRIM 함수
- 이 함수들은 문자열에 존재하는 공백을 제거하는 함수들입니다.
- LTRIM : 왼쪽 공백 삭제
- RTRIM : 오른쪽 공백 삭제
- TRIM : 왼쪽, 오른쪽 양쪽 다 공백 삭제

# 9. GROUP BY

### 1) GROUP BY 기초 1
```
SELECT gender FROM copang_main.member GROUP BY gender;
-젠더 컬럼을 기준으로 그룹핑 됐다.
distinct와 비슷하지만 전혀 다르다.

SELECT gender, COUNT(*) FROM copang_main.member GROUP BY gender;

SELECT gender, COUNT(*), AVG(height) FROM copang_main.member GROUP BY gender;

SELECT gender, 
    COUNT(*), 
    AVG(height),
    MIN(weight)
    FROM copang_main.member
    GROUP BY gender;

- GROUP BY 와 함께쓰면 각 그룹에 대해서 각각 사용되는걸 명심해야한다!
```
### 2) GROUP BY 기초 2
```
SELECT 
    SUBSTRING(address, 1, 2) as region,
    COUNT(*)
FROM copang_main.member 
GROUP BY SUBSTRING(address, 1, 2);
=> 서울에 살면 무조건 같은 그룹

### GROUP BY 에서 여러개의 컬럼을 사용 할수도 있다.

SELECT 
    SUBSTRING(address, 1, 2) as region,
    gender,
    COUNT(*)
FROM copang_main.member 
GROUP BY 
    SUBSTRING(address, 1, 2),
    gender;
=> 서울에 살더라도 여성인지, 남성인지에 따라 나누어 진다.
```

### 3) GROUP BY 기초3
```
#EX1)
SELECT 
    SUBSTRING(address, 1, 2) as region,
    gender,
    COUNT(*)
FROM copang_main.member 
GROUP BY 
    SUBSTRING(address, 1, 2),
    gender
HAVING region = '서울' # Having ~을 가지고 있는 : '서울'을 가지고 있는

#EX2)
SELECT 
    SUBSTRING(address, 1, 2) as region,
    gender,
    COUNT(*)
FROM copang_main.member 
GROUP BY 
    SUBSTRING(address, 1, 2),
    gender
HAVING 
    region = '서울'
    AND gender = 'm';
- 서울에 사는 남성 그룹만 조회

#EX3)
SELECT 
    SUBSTRING(address, 1, 2) as region,
    gender,
    COUNT(*)
FROM copang_main.member 
GROUP BY 
    SUBSTRING(address, 1, 2),
    gender
HAVING region IS NOT NULL
ORDER BY
    region ASC
    gender DESC;
```
### 4) GROUP BY 주의 사항
- (1) GROUP BY 절 뒤에 쓴 컬럼 이름들만, SELECT 절 뒤에도 쓸 수 있다.

- (2) 대신 SELECT 절 뒤에서 집계 함수에 그 외의 컬럼 이름을 인자로 넣는 것은 허용된다.

# 10. SELECT 문의 실행 순서

- 각 절들을, 더 앞에 나와야 하는 순서대로 써보겠습니다. 

##### SELECT, FROM, WHERE ,GROUP BY,HAVING ,ORDER BY,LIMIT 

- 사실은 아래의 순서대로 해석 및 실행된다는 사실입니다. 

#### FROM, WHERE ,GROUP BY, HAVING ,SELECT, ORDER BY, LIMIT 

#### 어떤 식으로 해석 및 실행되는지를 하나씩 차례대로 살펴보면 다음과 같습니다.

- FROM : 어느 테이블을 대상으로 할 것인지를 먼저 결정합니다. 
- WHERE : 해당 테이블에서 특정 조건(들)을 만족하는 row들만 선별합니다. 
- GROUP BY : row들을 그루핑 기준대로 그루핑합니다. 하나의 그룹은 하나의 row로 표현됩니다.
- HAVING : 그루핑 작업 후 생성된 여러 그룹들 중에서, 특정 조건(들)을 만족하는 그룹들만 선별합니다. 
- SELECT : 모든 컬럼 또는 특정 컬럼들을 조회합니다. SELECT 절에서 컬럼 이름에 alias를 붙인 게 있다면, 이 이후 단계(ORDER BY, LIMIT)부터는 해당 alias를 사용할  수 있습니다.
- ORDER BY : 각 row를 특정 기준에 따라서 정렬합니다. 
- LIMIT : 이전 단계까지 조회된 row들 중 일부 row들만을 추립니다. 



# 11.서브쿼리 

### 1) 예시
```
SELECT i.id, i.name, AVG(star) AS avg_star
FROM item AS i LEFT OUTER JOIN review AS r
on r.item_id = i.id
GROUP BY i.id, i.name
HAVING avg_star < (SELECT AVG(star) FROM review) #sub_query
ORDER BY avg_star DESC;
# subquery 는 () 를 꼭 써주어야한다!
```

### 2) SELECT 절에 있는 서브쿼리
# 가장 비싼 것 구할때

```
SELECT id,
    name, 
    price,
    (SELECT MAX(price) FROM item)
FROM copang_main.item;
# 가장 비싼 컬럼으로 price 와의 차이를 비교하는대 쓸수있다.

SELECT id,
    name, 
    price,
    (SELECT AVG(price) FROM item) AS avg_price
FROM copang_main.item;
#평균이랑 얼마나 차이나는 지 알아볼때 쓸수있다.
```

### 3) WHERE 절에 있는 서브 쿼리

```
#EX1)
SELECT
    id,
    name,
    price,
    (SELECT AVG(price) FROM item) AS avg_price
FROM copang_main.item
WHERE price > (SELECT AVG(price) FROM item);

#EX2) 가장 비싼 아이템
SELECT id, name, price
FROM item
WHERE price = (SELECT MAX(price) FROM item);

#EX3) 가장 싼 아이템
SELECT id, name, price
FROM item
WHERE price = (SELECT MIN(price) FROM item);
```
### 4) WHERE 절에 있는 서브쿼리2

```
#Q) 코팡의 상품 중에서 리뷰가 최소 3개이상 달린 상품들의 정보만 보고 싶다면?
# 시작을 JOIN 으로 생각했다면 , 시작은 좋다!
SELECT * FROM item
WHERE id IN #IN 이 있으며 그뒤에있는 값들중 하나라도 같으면 조건 만족 
(
SELECT item_id
FROM review
GROUP BY item_id HAVING count(*) >=3
)
```

### 5) WHERE col > ANY(SOME) , ALL 의미

- SUBQUERY 결과값중 단 하나의(ANY) 값보다도 크다면 True를 리턴
- SUBQUERY 모든 값보다(ALL) 커야 True를 리턴

### 6) FROM 절에 있는 서브쿼리

```
# 서브쿼리가 테이블 형태의 결과를 리턴하기도 한다.

SELECT
    SUBSTRING(address, 1, 2) AS region,
    COUNT(*) AS review_count
FROM review AS r LEFT OUTER JOIN member as m
on r.mem_id = m.id
GROUP BY SUBSTRING(address, 1, 2)
HAVING region is NOT NULL
    AND region != '안드';

# subquery 로 만들어보자

SELECT 
    AVG(review_count),
    MAX(review_count),
    MIN(review_count)
FROM
(
SELECT
    SUBSTRING(address, 1, 2) AS region,
    COUNT(*) AS review_count
FROM review AS r LEFT OUTER JOIN member as m
on r.mem_id = m.id
GROUP BY SUBSTRING(address, 1, 2)
HAVING region is NOT NULL
    AND region != '안드') AS review_count_summary;  

# 그러면 컬럼별로 AVG, MAX, MIN 이 각각 도출 된다.

#derived table -> subquery로 새롭게 도출된 테이블
*주의* derived table 에는 꼭 alias를 붙여 주어야 한다!
```

### 7)  EXISTS, NOT EXISTS와 상관 서브쿼리
- in 과 비슷하지만 성능은 더 좋다.

### 8) 예시

```

-- SELECT MAX(copang_report.price) max_price, AVG(copang_report.star) avg_star,
-- count(distinct(copang_report.email)) distinct_email_count
-- FROM 
-- (
-- select i.price , r.star , m.email from member m
-- inner join item i on i.id = m.id
-- inner join review r on i.id = r.id
-- ) as copang_report

#오답 => 조인의 기준이 이상하기때문이다.

#정답
SELECT MAX(copang_report.price) max_price, AVG(copang_report.star) avg_star,
count(distinct(copang_report.email)) distinct_email_count
FROM 
(
select i.price , r.star , m.email from review r
inner join item i on r.item_id = i.id
inner join member m on m.id = r.mem_id
) as copang_report

#member 는 review 의 member_id 와
#item 는 review 의 item_id와 맞추어 주어야 join 이확실하게 된다.
```
#### 9) 서브쿼리의 중첩과 그 문제점.

- LEFT OUTER JOIN 을 줄여쓰면 LEFT JOIN 이 된다.
- INNER JOIN 을 줄여쓰면 그냥 JOIN 이 된다.


# 추가 문법들

## UNION , UNION ALL
- UNION 은 중복 없이 
- UNION ALL 은 중복도 포함해서 나타내 

