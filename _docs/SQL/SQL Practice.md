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
|:------:|:------:|
|POWER(컬럼명/값,n)<br/>POW(컬럼명/값,n)|값을 n제곱해서 반환<br/>eg. POWER(2,3)=8, POW(2,2)=4<br/><br/> 값의 n제곱근 구하기 = 값의 1/n제곱 구하기 = POWER(칼럼명/값, 1/n) <br/>eg. POWER(8,$\frac{1}{3}$)=2 |
|SQRT(칼럼명/값)|값의 제곱근을 반환<br/>SQRT(4)=2|
|MOD(컬럼명/값, n)<br/>컬럼명/값%n|값을 n으로 나누었을 때의 나머지를 반환<br/>eg. MOD(4,2)=0, 4 % 2=0|
|ABS(컬럼명/값, n)|값의 절대값을 반환 <br/> eg. ABS(-4.5)=4.5|

## 문자열을 다루는 함수

|함수명|내용|
|:------:|:------:|
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

위 칼럼명 지정하는 lat, lon을 alias라 함


## 3번, 4번 문제 풀이
**[3번]**
내 풀이
```
SELECT TRUNCATE(ABS(MAX(LAT_N)-MIN(LAT_N))+ABS(MAX(LONG_W)-MIN(LONG_W)),4)
FROM STATION
```

**[4번]**

Amber's conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy:  ![](https://s3.amazonaws.com/hr-challenge-images/19505/1458531031-249df3ae87-ScreenShot2016-03-21at8.59.56AM.png)

Given the table schemas below, write a query to print the  _company_code_,  _founder_  name, total number of  _lead_  managers, total number of  _senior_  managers, total number of  _managers_, and total number of  _employees_. Order your output by ascending  _company_code_.

###  JOIN & GROUPBY 동시에, DISTINCT
내 풀이, 문제 어려워

```
SELECT company_code, founder_name, COUNT(distinct leader_manager)
FROM lead_manager 
LEFT JOIN lead_manager.company_code=Company.company_code
GROUP BY leader_manager.company_code
```

강의 풀이: Hierarchy 제대로 파악할 것(예외 케이스까지 파악하자)
* COUNT(DISTINCT ...): 중복없이 출력하기
* INNER JOIN vs LEFT JOIN: 출력 목적 제대로 파악

```
# 출력물: company_code, founder, #lead_M, #senior_M, #M , #employees

# ver1.(오류) conpany, employee table로만 쿼리 작성
# C1, Monica, 1, 1, 1, 2 (count distinct)
# C2, Samantha, 1, 1, 2, 2

SELECT C.company_code, 
       , C.founder
       , COUNT(DISTINCT E.lead_manager_code) ‘중복이 있을 수 있다고 했으니 COUNT DISTINCT 이용
       , COUNT(DISTINCT E.senior_manager_code),
       , COUNT(DISTINCT E.manager_code)
FROM Company C
     INNER JOIN Employee E ON C.company_code = E.company_code
GROUP BY C.company_code, C.founder
ORDER BY C.company_code
‘ 위 코드는 employee를 가지지 않은 매니저가 1명이라도 존재하면 문제에서 의도한 쿼리를 만들 수 없음, 예제에서 SM2는 employee를 가지지 못함

# ver2. (답) 모든 쿼리 이용, senior_M 수는 senior_M 테이블 이용 
# C1, Monica, 1, 2, 1, 2 <-ver1과 다름
# C2, Samantha, 1, 1, 2, 2

SELECT C.company_code
     , C.founder
     , COUNT(DISTINCT LM lead_manager_code) ‘ lead_manager_code 가진 테이블이 여러개므로 LM 테이블 명시하기
     , COUNT(DISTINCT SM.senior_manager_code)
     , COUNT(DISTINCT M.manager_code)
     , COUNT(DISTINCT E.employee_code)
FROM Company C
     LEFT JOIN Lead_Manager LM ON C.compnay_code = LM.company_code
     LEFT JOIN Senior_Manager SM ON LM.lead_manager_code = SM.lead_manager_code
     LEFT JOIN Manager M ON SM.senior_manager_code = M.senior_manager_code
     LEFT JOIN Employee E ON M.manager_code = E.manager_code
GROUP BY C.company_code, C.founder
ORDER BY C.company_code
```

# Set 3

1.  Population Density Difference:  [https://www.hackerrank.com/challenges/population-density-difference/problem](https://www.hackerrank.com/challenges/population-density-difference/problem)
2.  Weather Observation Station 11:  [https://www.hackerrank.com/challenges/weather-observation-station-11/problem](https://www.hackerrank.com/challenges/weather-observation-station-11/problem)
3.  Weather Observation Station 13:  [https://www.hackerrank.com/challenges/weather-observation-station-13/problem](https://www.hackerrank.com/challenges/weather-observation-station-13/problem)
4.  Top Competitors:  [https://www.hackerrank.com/challenges/full-score/problem](https://www.hackerrank.com/challenges/full-score/problem)

**[1번]**

```
SELECT MAX(POPULATION)-MIN(POPULATION)
FROM CITY
```

**[2번]**
###  정규표현식
모음으로 시작하거나 끝나지 않는 값
내 답안: 런타임 오류

```
SELECT CITY
WHERE NOT SUBSTRING(CITY,1) IN ('a','e','i','o','u')
AND
SUBSTRING(CITY,-1) IN ('a','e','i','o','u')
FROM STATION
```

정답
- LEFT(string,1): 첫번째 글자

```
SELECT DISTINCT(CITY) ` 중복 출력하지 않기!!!
FROM station
WHERE LEFT(city,1) NOT IN (‘A’, ‘E’, ‘I’, ‘O’, ‘U’)
OR RIGHT(city,1) NOT IN (‘A’, ‘E’, ‘I’, ‘O’, ‘U’)

# 또는 
# NOT LIKE ‘A%’ #A로 시작하지 않음을 의미
```

**[3번]**
### 대소 비교
- greater than, less then은 >, < 
- 경계값 포함하면 BETWEEN ... AND

```
SELECT TRUNCATE(SUM(LAT_N),4)
WHERE LAT_N>38.7880 AND LAT_N<137.2345
FROM STATION

# 경계값을 포함할 때(>=, <=)
# WHERE LAT_N BETWEEN 38.7880 AND 137.2345 
```

**[4번]**
**다중 조인, 조건에 따른 출력**

Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective (출력1)_hacker_id_ and (출력2) _name_ of hackers who achieved full scores for _more than one_ challenge. (정렬1) Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then (정렬2) sort them by ascending _hacker_id_.

**내 답안**
오답

```
SELECT hacker_id, name
FROM HACKERS
LEFT JOIN HACKERS.hacker_id = Submissions.hacker_id 
LEFT JOIN HACKERS.hacker_id = Challenges.hacker_id
LEFT JOIN Challenges.difficulty_level = Difficulty.difficulty_level
WHERE SUBMISSIONS.score = Difficulty.score
```

**풀이** 
-  INNER JOIN으로 필터링 기준 테이블을 참조하자


출력대상: full_score = score & 1번 이상 만점
- Full score 대상자

|submission_id|hacker_id|challenge_id|score|difficulty_level|full_socre|
|:----:|:----:|:----:|:----:|:----:|:----:|
|94613|86870|71055|30|2|30|
|97397|90411|66730|100|6|100|
|97397|90411|71055|30|2|30|
- 1개 이상의 챌린지에서 만점을 받아야하므로 90411만 출력

```
# STEP1. submission table의 challenge_id을 참조해 challenge table로부터 difficulty_level 칼럼 생성
# STEP2. difficulty_level의 full score를 difficulty table을 참조해 score 칼럼 생성

SELECT H.hacker_id
     , H.name
#     , COUNT(DISTINCT s.submission_id) # 이 예제는 중복이 없어서 DISTINCT는 안써도 무방
FROM Submissions S
     INNER JOIN HACKER H ON S.hacker_id = H.hacker_id
     INNER JOIN Challenges C ON S.challenge_id = C.challenge_id
     INNER JOIN Difficulty D ON C.difficulty_level. = D.difficulty level # 각 챌린지의 full score 알아내기
WHERE S.score = D.score # full score인 submission 찾는 필터링
HAVING COUNT(DISTINCT s.submission_id)>1 # 2번 이상 만점 필터링 -> HAVING 절: 위 조건들 GROUP BY 결과물 수
ORDER BY COUNT(DISTINCT s.submission_id) DESC, H.hacker # 정렬순서: 만점 많이 받은 순, 해커이름 순     
```

# Set 4
1.  Weather Observation Station 3: [https://www.hackerrank.com/challenges/weather-observation-station-3/problem](https://www.hackerrank.com/challenges/weather-observation-station-3/problem)
2.  Weather Observation Station 19: [https://www.hackerrank.com/challenges/weather-observation-station-19/problem](https://www.hackerrank.com/challenges/weather-observation-station-19/problem)
3.  Placements: [https://www.hackerrank.com/challenges/placements/problem](https://www.hackerrank.com/challenges/placements/problem)
4.  Binary Tree Nodes: [https://www.hackerrank.com/challenges/binary-search-tree-1/problem](https://www.hackerrank.com/challenges/binary-search-tree-1/problem)

**[1번]**
**몫, 중복 없애기: DISTINCT**

```
# ID가 짝수인 CITY를 중복 없이 출력하기
SELECT DISTINCT CITY 
FROM STATION
WHERE ID%2 = 0
```

답안

```
SELECT DISTINCT CITY # 중복 제거
FROM station
WHERE MOD(ID,2) = 0 # even ID number
```

**[2번]**
**위/경도 최대, 최소값의 유클리디안 거리 구하기 & 소수점 4자리까지 보여주기**

cf. head 기능처럼 세 줄만 보여주고 싶으면 코드 뒤에 ```LIMIT 3``` 과 같이 덧붙이면 됨

```
SELECT TRUNCATE(SQRT(POWER(MAX(LAT_N)-MIN(LAT_N),2)+POWER(MAX(LONG_W)-MIN(LONG_W),2)),4)
FROM STATION
```

답안

```
SELECT MIN(LAT_N) AS a
     , MAX(LAT_N) AS b
     , MIN(LONG_W) AS c
     , MAX(LONG_W) AS d
     , ROUND(SQRT(POWER(a - b, 2) + POWER(c - d, 2)), 4)
FROM station 
```

수식이 복잡할 때 위와 같이 a, b, c, d 정의 후 a, b, c, d 자리에 원래 값 넣으면서 풀면 간단하다!

```
SELECT ROUND(SQRT(POWER(MIN(LAT_N) - MAX(LAT_N), 2) + POWER(MIN(LONG_W) - MAX(LONG_W), 2)), 4)
FROM station 
```


**[3번]**
### 연속 JOIN
친구의 월급이 더 높은 경우 

내 답안: 오답

```
SELECT S.ID
FROM (Students S 
LEFT JOIN Friends F ON S.ID = F.ID
LEFT JOIN Packages P1 on P1.ID = S.ID
LEFT JOIN Packages P2 on P2.ID = F.Friend_ID)
WHERE P1.Salary < P2.Salary
ORDER BY P2.Salary DESC
```

Friends, Students, Packages

|ID|Friend ID|Salary|Friends Salary|친구 연봉 더 높음|출력순서|
|:------:|:------:|:------:|:------:|:------:|:------:|
|1|2|15.20|10.06|-|-|
|2|3|10.06|11.55|o|3|
|3|4|11.55|12.12|o|2|
|4|1|12.12|15.20|o|1|
Friends - Students: ID로 조인
+ Pacakges: Friends ID로 조인

답안

```
SELECT S.name
FROM Friends F
     INNER JOIN Students S ON F.id = S.id
     INNER JOIn Packages P1 ON P1.id = F.id # 내 연봉
     INNER JOIN Packages P2 ON P2.id = F.friends_id # 친구 연봉
WHERE P1.salary < P2.salary # 친구 연봉이 본인 연봉보다 많은 사람 추출
ORDER BY P2.salary # 친구 연봉 순 정렬
```

**[4번]**
### CASE WHEN, Self JOIN
table BST
N: Node of binary tree
P: Parent of N

-   _Root_: If node is root node.
-   _Leaf_: If node is leaf node.
-   _Inner_: If node is neither root nor leaf node.

노드 특징
root, leaf 구하면 나머지는 inner
root: 부모가 없는 노드
leaf: 자식이 없는 노드, 부모가 아님, P 컬럼에서 등장하지 않음

답안
고급반에서 sub query 배우기
안되면 join으로 풀어보기

```
# STEP1
SELECT BST.N
     , BST.P
     , CASE 
           WHEN BST.P IS NULL THEN 'Root'
           ELSE 'Leaf, Inner'
        END
FROM BST
```

```
# STEP2
SELECT DISTINCT BST.N
#     , BST.P
     , CASE 
           WHEN BST.P IS NULL THEN 'Root'
           WHEN BST2.N IS NULL THEN 'Leaf'
           ELSE 'Inner'
        END
FROM BST
     LEFT JOIN BST AS BST2 ON BST.N = BST2.P
# BST를 self join
# 만약 N의 1번 노드가 P에는 1번 노드가 없으므로 NULL로 남음 -> BST2.N이 NULL 
ORDER BY BST.N
```

![](//placehold.it/800x600)
