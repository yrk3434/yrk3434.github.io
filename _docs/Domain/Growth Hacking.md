---
title: Growth Hacking
category: Domain
order: 4
---


### Reference
- 인프런 강의: 그로스해킹 - 데이터와 실험을 통해 성장하는 서비스를 만드는 방법

---
# Contents
1. [그로스 해킹이란](#1-그로스-해킹이란)
2. [PMF](#2-pmf)
3. [AARRR](#3-aarrr)
    - [Acquisition](#31-acquisition)
    - Activation
    - Retention
    - Revenue
    - Referral

---

# 1. 그로스 해킹이란

> *데이터로부터 도출한 인사이트를 바탕으로 제품/서비스를 지속적으로 개선하는 것*

[위키피디아](https://ko.wikipedia.org/wiki/%EA%B7%B8%EB%A1%9C%EC%8A%A4_%ED%95%B4%ED%82%B9)<br/> 

- 그로스 해킹(Growth hacking)은 창의성, 분석적인 사고, 소셜 망을 이용하여 제품을 팔고, 노출시키는 마케팅 방법
- 고객의 반응(데이터)에 따라 제품 및 서비스를 수정해 제품과 시장의 궁합(Product-Market Fit)을 높이는 것
- 고객의 웹사이트 방문 기록, 머무른 시간, 회원 가입으로 전환되는 비율 등 분석

<br/>

1. 지표: AARRR, 핵심지표 정의와 활용, 의사결정에 이용
2. 분석환경: 데이터 파이프라인 구축, 데이터 수집 경로 결정, 툴 결정
3. 프로세스: Cross-Functional 직군이 모여 일함(UX, Dev., Data Analyst,...), 가설 검증, 플래닝, AB테스트
4. 문화: Data-Driven, 모든 조직이 한 방향으로 움직이되 데이터가 흐름

---

# 2. PMF
## 2.1. 정의
- Product-Market Fit: 서비스의 시장 적합성
- 그로스 실험의 전제조건: 서비스가 실험할 만한 가치가 있는지 체크(서비스가 문제에 대한 솔루션인지 체크, 서비스 이면의 가설 확인)

## 2.2. PMF 확인 방법

1. Retention
    - 시간이 지남에 따라 이탈하지 않고 서비스를 유지하는 이용자의 비율
    - 파란색 선(Product A): 시간이 지남에 따라 서비스 이용객의 비율이 완만하게 줄어들기 때문에 PMF가 양호하다고 말할 수 있다.

<center> <img src="https://images.squarespace-cdn.com/content/v1/528c45f9e4b06be250a9fe30/1389476491749-2P7WUPF27LRDEXUG0ATV/pm+fit+3.png?format=750w" width="50%"> </center>


2. Conversion
    - 한 단계에서 다음 단계로 넘어감
    - 아래와 그림과 같이 사용자가 서비스를 이용하기까지의 단계를 깔대기 모양에 빗대어 funnel이라 부른다. funnel의 각 단계를 지나가는 것을 conversion이라 부른다. 많은 funnel 단계를 지나간 이용자일 수록 loyalty가 있는 이용자라 할 수 있다.

<center> <img src="https://mobile-marketing-masterclass.com/wp-content/uploads/2020/02/Mobile_Marketing_Funnel.jpg" width="50%"> </center>


3. Net Promoter Score(NPS)
    - 특정 기업 및 브랜드, 제품・서비스에 대해 이용자가 얼마나 애착과 신뢰를 가졌는지를 수치화한 것
    - 서비스 추천 여부 설문조사: 0-10 점수 매김
        - Detractors: 0-6 (비판자)
        - Passives: 7-8 (중립자)
        - Promoters: 9-10 (추천자)
    - eg. 비판자 34%, 중립자 23%, 추천자 43%일 때, NPS = 43% - 34% =9%
    - 주의사항
        - retention, conversion, NPS는 서비스의 결과일 뿐 목적이 아니다. 주객전도되어 지표를 개선하기 위해 실험하지 않는다.
        - 새로운 기능을 추가하는 것으로 문제를 해결하려 하지 않는다.
    - TO-Do
        - 사용자의 의견을 직접 듣기
        - 사용자 데이터 분석

---

# 3. AARRR

<center><img src="https://miro.medium.com/max/1400/0*09d8JKcPpdKHaoLl.png" width="80%"></center>


## 3.1. Acquisition

- 사용자를 서비스로 데려오는 것

1. 사용자 구분
    - Organic vs Paid
        - 자발적 유입(Organic) <- Unknown(유입 경로를 모르는 경우)와 혼동하지 말 것
        - 마케팅으로 인한 유입(Paid)
    - 유입 채널 별 구분(유튜브 광고/지인 추천/검색/...) 

2. 지표
    - 유저 획득 지표: Signup, CAC(Customer Acquisition Cost)
    - 광고 집행 지표: 
        - 과금되는 광고상품: CPC(Cost Per Click), CPI(Cost per Install), CPA(Cost per Action, 일반적으로 complete registration 액션 당 과금), CPM(Cost Per Mile, 노출 당 과금), CPP(Cost Per Period, 기간 보장형)
        - ROAS(Return on Ads Spending): 광고로 인한 매출액/광고비

3. CAC(Customer Acquisition Cost) - 유저 획득 비용
    - 총 CAC가 아닌 채널/캠페인/날짜 별 CAC를 확인해야 함
    - LTV: Lifetime Value, 고객생애가치, 한 고객이 서비스를 사용하는 동안 창출할 것으로 기대되는 순수익 
    - CAC < LTV 여야 기업이 망하지 않는다.

4. UTM(Urchin Tracking Module) paramters

    *UTM parameters are short text codes that you add to URLs (or links) to help you track the performance of webpage or campaign.*

    - 링크를 통해 유저가 어느 경로로 서비스에 유입되었는지 추적할 수 있는 파라미터
    - 앞서 CAC를 구할 때 유입경로를 나누어 CAC를 구해야 한다고 언급했는데 UTM parameter를 통해 유저가 어떤 경로로 유입되었는지 파악할 수 있고, 효과적인 마케팅을 위한 채널 및 마케팅 방법을 결정할 수 있다.
    <center><img src="https://cdn.searchenginejournal.com/wp-content/uploads/2020/05/utm-codes-explained-5ecdde9a0cd2c-1520x800.png" width="80%"></center>
    
    - 파라미터 구성
        - Source: 유입 채널, eg. 페이스북, 블로그, 뉴스레터
        - Medium: 링크 유형, eg. organic social, paid social, email
        - Campaign: 홍보 유형, eg. summer_sale, free_trial
        - Term: 유입 키워드 
        - Content: 유저 유입된 구체적인 대상, eg. 배너, 광고 문구

    - 구글 애널리틱스에서 UTM paramter 확인 가능

    <center><img src="https://www.orbitmedia.com/wp-content/uploads/2019/02/campaign-tracking.png" width="90%"></center>

    - 구글 URL builder를 통해 다음과 같이 사용자의 유입경로를 추적할 수 있는 URL을 생성할 수 있음

    <center><img src="https://polkadotdata.com/wp-content/uploads/2019/07/pdd-utm-parameters-example-1.png" width="60%"></center>

5. Attribution
    - 유저가 앱을 설치, 실행하는데 기여한 채널 파악
    - 서비스에 따라 기여 기준 다름(정답이 없음)
    - 종합적인 판단 필요
    - 개념
        - 어트리뷰션 윈도우(룩백 윈도우): 어트리뷰션으로 인정되는 기간
        - Click-trough / View-through: 어트리뷰션으로 인정되는 행동, 클릭? 보기? eg. 유투브 클릭/미리보기 <br/>
        클릭이 뷰보다 항상 좋은 것은 아님, eg. 팝업창을 잘못 누른 것보다는 보기를 10초 이상 한 것이 앱 설치를 결정하는 데에 더 큰 attribution일 수 있다.
        - 어트리뷰션 모델: 여러 터치포인트가 있을 때 어트리뷰션 인정 기준
    <center><img src="https://mcgaw.io/wp-content/uploads/2020/07/lookback-attribution-window.jpg" width="90%"></center>

        위 그림을 빨간 점이 앱 설치 행위라 할 때, 앱 설치 행동에 기여한 채널을 파악할 때 룩백 윈도우(회색 박스) 내 행위(1월 15일 채널 클릭)를 attribution으로 결정함

    - ROAS(Return on Ads Spending) = 100%*(revenue from ads)/(cost of ads)
        - ROAS는 비율 지표이고 광고비와 매출이 정비례하지 않기 때문에 ROAS가 높다고 항상 좋은 것은 아니다.
        - eg. 광고비가 2000만원, 매출 1억: ROAS = 500% <br/>
        광고비가 10만원, 매출 150만원: ROAS 1500% <br/>
        -> 광고비를 줄이면 마케팅 대상을 엄격하기 targeting하기 때문에 ROAS는 높아지나 마케팅으로 인한 매출 자체는 줄어듦
    <center><img src="https://uploads-ssl.webflow.com/5f7bd27f467dc0a5944e00d4/6080c547d921c918173506cc_ROAS_Graph_Missed_Revenue_T0.png" width="90%"></center>
        - 다양한 조건으로 attribution 조건을 세팅해보고, 성과를 다양한 기준으로 측정해 봐야함