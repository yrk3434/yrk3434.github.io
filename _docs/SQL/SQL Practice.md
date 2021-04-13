---
title: SQL practice
category: SQL
order: 1
---

# 데이터 분석을 위한 중급 SQL 문제풀이

### REFERENCE
SQL 함수 리스트
https://dev.mysql.com/doc/refman/8.0/en/numeric-functions.html

코드 연습 사이트
https://www.hackerrank.com/
https://leetcode.com/

# 문제풀이 핵심 함수 개념
## 숫자를 다루는 함수

**[소수점 이하 숫자 다루기]**

|함수명|내용|
|:------:|:------|
|ROUND(컬럼명/값,n)|값을 소수점 이하 n자리수로 반올림<br/>eg. ROUND(8.765,2) ->8.77|
|TRUNCATE(컬럼명/값,n)|값을 소수점 이하 n자리수까지만 남기고 나머지 버림<br/>eg. TRUNCATE(8.765,2) ->8.76|
|CEIL(컬럼명/값)|값의 소수점 이하를 올림해 정수 반환<br/>eg. CEIL(8.765) ->9|
|FLOOR(컬럼명/값)|값의 소수점 이하를 버림해 정수 반환<br/>eg. FLOOR(8.765) ->8|

**[간단한 연산하기]**
|함수명|내용|
|:------:|:------|
|POWER(컬럼명/값,n)<br/>POW(컬럼명/값,n)|값을 n제곱해서 반환<br/>eg. POWER(2,3)=8, POW(2,2)=4<br/><br/> 값의 n제곱근 구하기 = 값의 1/n제곱 구하기 = POWER(칼럼명/값, 1/n) <br/>eg. POWER(8,$\frac{1}{3}$)=2 |
|SQRT(칼럼명/값)|값의 제곱근을 반환<br/>SQRT(4)=2|
|MOD(컬럼명/값, n)<br/>컬럼명/값%n|값을 n으로 나누었을 때의 나머지를 반환<br/>eg. MOD(4,2)=0, 4 % 2=0|
|ABS(컬럼명/값, n)|값의 절대값을 반환 <br/> eg. ABS(-4.5)=4.5|

## 문자열을 다루는 함수

|함수명|내용|
|:------:|:------|
|LOWER(컬럼명/문자열)|모든 문자를 소문자로 반환<br/> eg. SELECT LOWER(MemeberID) FROM tb_datarian|
|UPPER(컬럼명/문자열)|모든 문자를 대문자로 반환|
|REPLACE(컬럼명/문자열, 패턴1, 패턴2)|문자열에 포함된 패턴1을 패턴2로 대체해 반환<br/>eg. SELECT REPLACE(MemberID,'A','B') FROM tb_datarian|
|CONCAT(컬럼명/문자열1,컬럼명/문자열2)|여러 개의 문자열을 차례대로 연결해 하나의 문자열로 반환 <br/> SELECT CONCAT(ID, ':', MemberID) AS Member FROM to_datarian|
|그 외|LEFT(), LENTH(), LIKE, |

- 참고. SELECT문은 데이터 자체를 바꾸는 게 아니라 데이터를 가져오기만 함(OUTPUT 값만 바꿔 출력)

DATASET: ***to_datarian***
|ID|MemberID|
|:---:|:---:|
|1|A01|
|2|A02|
|3|A03|
```
SELECT LOWER(MemeberID) FROM tb_datarian
```
|ID|MemberID|
|:---:|:---:|
|1|a01|
|2|a02|
|3|a03|
```
 SELECT REPLACE(MemberID,'A','B') FROM tb_datarian
 ```
|ID|MemberID|
|:---:|:---:|
|1|B01|
|2|B02|
|3|B03|
```
SELECT CONCAT(ID, ':', MemberID) AS Member FROM to_datarian
```
|Member|
|:---:|
|1:A01|
|2:A02|
|3:A03|

# SET1
Set 1은 Leetcode 문제들로 구성되어 있습니다.

아래 4개 문제를 푼 뒤에, 해설 강의로 넘어가시면 됩니다.

1.  Leetcode 595. Big Countries: [https://leetcode.com/problems/big-countries/](https://leetcode.com/problems/big-countries/)
2.  Leetcode 620. Not Boring Movies: [https://leetcode.com/problems/not-boring-movies/](https://leetcode.com/problems/not-boring-movies/)
3.  Leetcode 182. Duplicate Emails: [https://leetcode.com/problems/duplicate-emails/](https://leetcode.com/problems/duplicate-emails/)
4.  Leetcode 175. Combine Two Tables: [https://leetcode.com/problems/combine-two-tables/](https://leetcode.com/problems/combine-two-tables/)


## 1번, 2번 문제 풀이
**[1번]**
```
SELECT name, population, area -- 출력할 변수명
FROM world  -- 데이터셋명
WHERE area > 3000000 -- 조건
OR population > 25000000
```
**[2번]**
```
# 1. 홀짝 판별
# 2. 지루한 영화는 거르기
# 3. Rating으로 정렬하기

SELECT id, movie, description, rating
FROM cinema
WHERE MOD(id,2) = 1 --odd numbered ID 
-- 또는 WHERE id%2 = 1
AND description != 'boring'
ORDER BY rating DESC
```
## 3번, 4번 문제 풀이
**[3번]**
### GROUP BY, HAVING
```
SELECT Email
	, COUNT(id) AS cnt
FROM Person
GROUP BY Email
```
output:
|Email|cnt|
|:-----:|:-----:|
|a@b.com |2|
|c@b.com|1|

GROUP BY를 연산한 컬럼을 가지고  필터링하려면 HAVING 사용 
```
SELECT Email -- COUNT(id) AS cnt는 출력하지 않을 것이므로 제거
FROM Person
GROUP BY Email
HAVING COUNT(Id) >=2
```
**[4번]**
### LEFT JOIN
```
SELECT FirstName, LastName, City, State
FROM Person
	LEFT JOIN Person.PersonId = Address.PersonId
```
Address 테이블에 정보 없어도 출력하기 -> Person 테이블 기준 LEFT JOIN
input: (Person 테이블)
|PersonId|FirstName|LastName|
|:-----:|:-----:|:-----:|:-----:|
3|Sunmi |Yoon|

output:
|FirstName|LastName|City|State|
|:-----:|:-----:|:-----:|:-----:|
|Sunmi |Yoon|Null|Null|

# SET2
1.  Japan Population: [https://www.hackerrank.com/challenges/japan-population/problem](https://www.hackerrank.com/challenges/japan-population/problem)
2.  Weather Observation Station 2: [https://www.hackerrank.com/challenges/weather-observation-station-2/problem](https://www.hackerrank.com/challenges/weather-observation-station-2/problem)
3.  Weather Observation Station 18: [https://www.hackerrank.com/challenges/weather-observation-station-18/problem](https://www.hackerrank.com/challenges/weather-observation-station-18/problem)
4.  New Companies: [https://www.hackerrank.com/challenges/the-company/problem](https://www.hackerrank.com/challenges/the-company/problem)

## 1번, 2번 문제 풀이
**[1번]**
내 풀이
```
SELECT SUM(POPULATION) 
FROM CITY
WHERE COUNTRYCODE = 'JPN'
```

**[2번]**
내 풀이
```
SELECT ROUND(SUM(LAT_N),2) as lat,
    ROUND(SUM(LONG_W),2) as lon
FROM STATION;
```
