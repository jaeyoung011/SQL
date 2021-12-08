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



