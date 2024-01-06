---
title: Pyspark
category: Project
order: 2
use_math: true
comments: true
---


# Contents
1. [전처리 실습](#1-전처리-실습) <br/>
2. [머신러닝 실습](#2-머신러닝-실습) <br/>

- pyspark를 이용한 전처리와 머신러닝을 실습한다.
- 로컬에서 사용하려면 java, hadoop, spark를 설치하고 환경변수를 추가하면 되는데 이 과정은 다른 블로그를 참조하길 바란다.
- Databricks(커뮤니티 에디션은 무료)나 캐글 주피터노트북을 이용하면 별다른 설정없이 pyspark를 사용할 수 있다.

------

# 1. 전처리 실습

## 1.1. 주요 함수

|내용|함수|
|:---:|:---:|
|스파크 세션 만들기|spark = SparkSession.builder.appName('...').getOrCreate()|
|데이터 조회|data.show()|
|요약 통계량 조회|data.describe.show()|
|행의 수 조회|data.count()|
|조건 필터링|data.filter(...)|
|그룹별 연산|data.groupBy(groupCol).agg(...)|
|기존 컬럼으로 <br/>새 컬럼 만들기|data.withColumn('newCol', function('oldCol'))|
|조건에 따라 연산|when, others|
|칼럼 한 개 삭제|data.drop('col1')|
|칼럼 두 개 삭제|data.drop(*('col1', 'col2'))|
|parquet 파일 쓰기|data.write.parquet('...')|
|parquet 파일 읽기|data_pq = spark.read.parquet('...')|


## 1.2. 실습 목록
- 주피터 노트북으로 실습 결과를 정리했다. 다음을 클릭.
- 캐글 데이터: [Predict Future Sales](https://github.com/yrk3434/Open_Archive/blob/main/Spark/predict-future-sales-pyspark-preprocessing.ipynb)


---

# 2. 머신러닝 실습

- 인프런(스파크 머신러닝 완벽 가이드 - Part 1, 권철민) 강의를 참고했다. 강사님이 저작권 문제를 강조했으므로 함수는 강의에서 배우는 함수는 사용하되 독자적인 예제로 실습해보겠다.
